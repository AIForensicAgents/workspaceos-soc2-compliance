# WorkspaceOS — System Architecture and Security Design

**Document Classification:** Confidential — Internal Engineering  
**Version:** 2.0.0  
**Last Updated:** July 2025  
**Authors:** Platform Architecture Team  
**Review Cadence:** Quarterly or upon material infrastructure change

---

## Table of Contents

1. System Overview
2. Tenant Isolation Model
3. Provisioning Pipeline
4. Authentication and Authorization
5. Data Flow Diagrams
6. Encryption Architecture
7. Monitoring and Observability
8. Backup and Recovery
9. Network Architecture
10. Third-Party Integrations
11. Appendices

---

## 1. System Overview

### 1.1 Purpose

WorkspaceOS is a multi-tenant platform that provisions, manages, and secures **fully isolated Google Workspace environments** on behalf of each customer. Rather than sharing a single Google Workspace organization across tenants, WorkspaceOS creates a dedicated Google Workspace instance per customer — complete with its own domain, admin console, organizational units, data-loss prevention policies, and identity configuration. The platform then layers unified billing, observability, policy orchestration, and lifecycle management on top of these isolated instances.

### 1.2 Design Principles

| Principle | Implication |
|---|---|
| **Tenant-per-Workspace** | Every customer receives a discrete Google Workspace subscription bound to a unique primary domain. No shared organizational units, no shared Drive namespaces, no shared directory. |
| **Zero-Trust by Default** | Every inter-service call is mutually authenticated. No implicit trust is granted based on network location alone. |
| **Least Privilege Everywhere** | Service accounts, human operators, and integrations receive the minimum scopes and IAM roles necessary, reviewed quarterly. |
| **Automation First** | Every provisioning, configuration, and de-provisioning step is codified. Manual console access is break-glass only. |
| **Defense in Depth** | Isolation is enforced at the identity layer, the network layer, the data layer, and the application layer simultaneously. |
| **Observable by Design** | Every subsystem emits structured telemetry. Alerts fire on anomaly, not just threshold. |

### 1.3 High-Level Component Map

```
┌─────────────────────────────────────────────────────────┐
│                     WorkspaceOS Platform                │
│                                                         │
│  ┌──────────┐  ┌──────────────┐  ┌──────────────────┐  │
│  │ Control  │  │ Provisioning │  │   Policy Engine   │  │
│  │  Plane   │  │   Pipeline   │  │  (OPA / Rego)     │  │
│  │  (API)   │  │              │  │                    │  │
│  └────┬─────┘  └──────┬───────┘  └────────┬───────────┘  │
│       │               │                    │              │
│  ┌────▼─────────────────▼────────────────────▼──────────┐  │
│  │              Tenant State Store (PostgreSQL)         │  │
│  │              + Event Bus (Kafka / Pub/Sub)           │  │
│  └────┬─────────────────┬────────────────────┬──────────┘  │
│       │                 │                    │              │
│  ┌────▼─────┐  ┌────────▼───────┐  ┌────────▼──────────┐  │
│  │  Auth /  │  │  Observability │  │  Billing & Usage  │  │
│  │  AuthZ   │  │  Stack         │  │  Aggregator       │  │
│  │  Gateway │  │                │  │                    │  │
│  └──────────┘  └────────────────┘  └───────────────────┘  │
│                                                         │
└───────────────────────┬─────────────────────────────────┘
                        │
          ┌─────────────▼──────────────┐
          │  Google Workspace Admin SDK │
          │  + Google Cloud Platform    │
          │  (per-tenant GCP projects)  │
          └────────────────────────────┘
```

### 1.4 Stakeholder Roles

| Role | Scope |
|---|---|
| **Platform Operator** | WorkspaceOS engineering staff; manages control plane infrastructure |
| **Tenant Admin** | Customer's designated IT administrator; manages their isolated Workspace |
| **Tenant End User** | Customer employee consuming Google Workspace services |
| **Security Team** | Cross-cutting; owns threat modeling, pen-test cadence, incident response |
| **Compliance Officer** | Validates adherence to SOC 2, ISO 27001, GDPR, and customer-specific frameworks |

---

## 2. Tenant Isolation Model

### 2.1 Isolation Philosophy

WorkspaceOS treats **tenant isolation as a non-negotiable safety invariant**, not a feature. Every layer in the stack enforces isolation independently so that a failure in one layer does not cascade into cross-tenant data exposure.

### 2.2 Isolation Boundaries

#### 2.2.1 Google Workspace Layer

Each customer is provisioned with:

- A **unique Google Workspace subscription** tied to a customer-owned or WorkspaceOS-managed domain (e.g., `acmecorp.workspaceos.app`).
- A **dedicated super-admin service account** whose credentials are stored in a tenant-specific secret in HashiCorp Vault (or Google Secret Manager), never co-mingled.
- **Separate organizational units** configured by the provisioning pipeline according to the customer's requested structure.
- **Independent data residency settings** configured at the Workspace-organization level, honoring the customer's jurisdictional requirements.

Because Google Workspace organizations are inherently separate identity and data silos, this architecture provides **hard isolation at the SaaS layer** — Drive files, Gmail messages, Calendar events, and Admin Console settings from Tenant A are cryptographically and logically inaccessible from Tenant B.

#### 2.2.2 GCP Project Layer

Each tenant's administrative automation (Cloud Functions for event-driven config, Cloud Scheduler for compliance checks, logging sinks) runs inside a **dedicated GCP project** nested under a WorkspaceOS-controlled GCP organization node. Resource hierarchy:

```
WorkspaceOS GCP Organization
└── Folder: "Tenants"
    ├── Folder: "tenant-acmecorp"
    │   ├── Project: acmecorp-workspace-mgmt
    │   └── Project: acmecorp-audit-logging
    ├── Folder: "tenant-globex"
    │   ├── Project: globex-workspace-mgmt
    │   └── Project: globex-audit-logging
    └── ...
```

IAM bindings are set at the project level. Cross-project access is denied by default via Organization Policy constraints (`constraints/iam.allowedPolicyMemberDomains`).

#### 2.2.3 Control Plane Data Layer

The WorkspaceOS control plane database (PostgreSQL) uses **Row-Level Security (RLS)** enforced via a `tenant_id` column present on every tenant-scoped table. Application database connections set `app.current_tenant` at session start; RLS policies filter all reads and writes transparently. As a secondary control, the application layer ORM also appends tenant filters, creating a two-layer guard.

Schema-level isolation (separate schemas per tenant) is available as an upgrade for customers requiring database-level audit evidence of separation.

#### 2.2.4 Network Layer

Tenant management traffic flows through **dedicated VPC Service Controls perimeters** per GCP project, preventing data exfiltration even if a service account token is compromised. Details in Section 9.

#### 2.2.5 Secret/Credential Layer

Every tenant's Google Workspace super-admin service account key, OAuth refresh tokens, and API keys are stored in a **tenant-namespaced path** within HashiCorp Vault:

```
secret/data/tenants/{tenant_id}/google/service_account_key
secret/data/tenants/{tenant_id}/google/oauth_refresh_token
secret/data/tenants/{tenant_id}/integrations/{integration_name}/api_key
```

Vault policies ensure that a control plane service scoped to `tenant-acmecorp` cannot read paths under `tenant-globex`.

### 2.3 Isolation Verification

- **Automated Isolation Tests:** A CI pipeline runs nightly, attempting cross-tenant data access via API, database, Vault, and GCP IAM. Any success is a P0 incident.
- **Chaos Injection:** Quarterly exercises deliberately inject misconfigurations (e.g., removing RLS policies, broadening IAM roles) in a staging environment to verify that secondary controls detect and alert.
- **Penetration Testing:** Annual third-party pen tests explicitly include cross-tenant escalation scenarios.

---

## 3. Provisioning Pipeline

### 3.1 Overview

The provisioning pipeline transforms a customer signup event into a fully operational, policy-compliant Google Workspace environment. The pipeline is implemented as a **state machine** orchestrated by Temporal (durable workflow engine), ensuring idempotency, retryability, and auditability.

### 3.2 Provisioning Stages

```
┌─────────┐    ┌───────────┐    ┌───────────────┐    ┌───────────────┐
│ Intake  │───▶│ Identity  │───▶│  Workspace    │───▶│   Policy      │
│ & Valid.│    │ Bootstrap │    │  Provisioning │    │   Application │
└─────────┘    └───────────┘    └───────────────┘    └───────────────┘
                                                            │
┌─────────────┐    ┌───────────────┐    ┌──────────┐       │
│ Integration │◀───│ Observability │◀───│  Smoke   │◀──────┘
│ Hooks       │    │ Setup         │    │  Tests   │
└─────────────┘    └───────────────┘    └──────────┘
```

#### Stage 1 — Intake and Validation

- Customer submits provisioning request via WorkspaceOS API or admin console.
- Input validation: domain ownership verification (DNS TXT record), billing pre-authorization, compliance questionnaire (data residency, industry vertical).
- Tenant record created in state store with status `PENDING_PROVISION`.
- Event published: `tenant.provision.requested`.

#### Stage 2 — Identity Bootstrap

- A GCP Folder and two GCP Projects are created for the tenant.
- A service account is created in the management project with Domain-Wide Delegation authority scoped to the minimum Admin SDK and Gmail/Drive/Calendar scopes.
- Service account key is generated, encrypted, and stored in Vault at the tenant-namespaced path.
- A WorkspaceOS platform identity (OIDC subject) is created for the tenant admin.

#### Stage 3 — Google Workspace Provisioning

- Using the Google Workspace Reseller API (or, for Enterprise tiers, the Google Channel Services API), a new Workspace subscription is created and bound to the customer's verified domain.
- The first super-admin user is created via the Directory API.
- MX records, SPF, DKIM, and DMARC DNS records are configured (if WorkspaceOS manages DNS; otherwise, instructions are provided to the tenant).
- Organizational Unit hierarchy is created per the customer's requested template.
- License assignment (Business Starter/Standard/Plus, Enterprise Standard/Plus) is executed.

#### Stage 4 — Policy Application

- The Policy Engine evaluates the customer's selected compliance profile (e.g., "HIPAA-ready", "SOC 2 baseline", "Financial services") and generates a set of Workspace Admin Console settings.
- Settings are applied via the Admin SDK, Groups Settings API, and Chrome Policy API:
  - Data Loss Prevention rules (Drive and Gmail).
  - Sharing restrictions (external sharing disabled by default).
  - 2-Step Verification enforcement.
  - Mobile device management policy.
  - Vault retention rules.
  - Context-Aware Access policies.
- Applied settings are checksummed and stored for drift detection.

#### Stage 5 — Smoke Tests

- Automated tests verify:
  - Super-admin can authenticate.
  - Directory API returns expected OU structure.
  - A test email routes correctly.
  - Drive file creation and sharing restrictions behave as configured.
  - Audit log events are flowing to the tenant's logging project.
- On failure, the pipeline halts and alerts the provisioning team; the tenant status moves to `PROVISION_FAILED` with diagnostic context.

#### Stage 6 — Observability Setup

- Log sinks are configured to export Google Workspace audit logs (Admin, Login, Drive, Gmail, SAML) to the tenant's GCP audit-logging project.
- A BigQuery dataset is created for long-term log analytics.
- Alerting policies are instantiated in Google Cloud Monitoring based on the tenant's compliance profile.
- A tenant-specific Grafana dashboard is auto-generated from templates.

#### Stage 7 — Integration Hooks

- Webhook endpoints are registered for customer-requested integrations (SIEM, ITSM, IdP federation).
- SCIM provisioning connector is activated if the customer uses an external IdP.
- Tenant status transitions to `ACTIVE`.
- Event published: `tenant.provision.completed`.

### 3.3 De-Provisioning

De-provisioning follows the reverse sequence with additional safeguards:

1. Tenant admin initiates off-boarding; a 30-day grace period begins.
2. Data export is offered via Google Takeout or the Data Transfer API.
3. After grace period, the Workspace subscription is cancelled, GCP projects are scheduled for deletion (30-day GCP soft-delete window), Vault secrets are destroyed, and the tenant record is marked `DEPROVISIONED`.
4. Audit records are retained for the contractually agreed period (default: 7 years).

### 3.4 Provisioning SLOs

| Metric | Target |
|---|---|
| Time from request to `ACTIVE` | < 15 minutes (P95) |
| Provisioning success rate | > 99.5% without manual intervention |
| De-provisioning completion | < 24 hours after grace period |

---

## 4. Authentication and Authorization

### 4.1 Authentication Architecture

WorkspaceOS supports three distinct authentication contexts:

#### 4.1.1 Tenant Admin Authentication to WorkspaceOS Control Plane

- **Protocol:** OpenID Connect (OIDC) Authorization Code Flow with PKCE.
- **Identity Provider:** WorkspaceOS operates its own Keycloak cluster (or Auth0 tenant) as the OIDC provider.
- **MFA:** Mandatory for all tenant admin accounts. Supported factors: TOTP, WebAuthn/FIDO2 hardware keys, push notification.
- **Session Management:** Access tokens are short-lived (15 minutes). Refresh tokens are bound to the device fingerprint and rotated on each use. Absolute session lifetime: 12 hours.
- **Federation:** Tenant admins may optionally federate their corporate IdP (Okta, Azure AD, Ping) via SAML 2.0 or OIDC, in which case Keycloak acts as a broker.

#### 4.1.2 Tenant End User Authentication to Google Workspace

- End users authenticate directly to Google's identity stack via the customer's Workspace login page.
- WorkspaceOS configures **third-party SAML/OIDC SSO** within the Workspace Admin Console if the customer requires federation with their corporate IdP.
- WorkspaceOS does not intermediate end-user Workspace authentication; this preserves Google's native security posture (risk-based challenges, BeyondCorp signals).

#### 4.1.3 Service-to-Service Authentication

- All internal microservices authenticate via **mTLS** using short-lived certificates issued by an internal CA (SPIFFE/SPIRE).
- External API calls to Google APIs use **OAuth 2.0 service account credentials** (JWT bearer assertion flow) with Domain-Wide Delegation.
- Service account tokens are requested with the minimum required scopes and are cached for no longer than their expiry minus a 5-minute buffer.

### 4.2 Authorization Architecture

#### 4.2.1