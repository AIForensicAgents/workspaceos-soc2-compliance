# WorkspaceOS Information Security Policy

**Document ID:** WOS-ISP-001
**Version:** 2.0
**Classification:** Internal – Confidential
**Effective Date:** January 15, 2025
**Last Reviewed:** January 15, 2025
**Next Review Date:** July 15, 2025
**Policy Owner:** Chief Information Security Officer (CISO), WorkspaceOS
**Approved By:** Board of Directors, WorkspaceOS Inc.

---

## Document Control

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | June 1, 2024 | CISO Office | Initial release |
| 1.1 | September 30, 2024 | CISO Office | Updated incident response timelines and added vendor risk tiers |
| 2.0 | January 15, 2025 | CISO Office | Comprehensive revision — added BCP/DR, expanded network security, privacy alignment updates |

| Reviewer | Role | Date | Signature |
|----------|------|------|-----------|
| [Name] | Chief Information Security Officer | January 10, 2025 | [On file] |
| [Name] | Chief Technology Officer | January 12, 2025 | [On file] |
| [Name] | Data Protection Officer | January 13, 2025 | [On file] |
| [Name] | General Counsel | January 14, 2025 | [On file] |
| [Name] | Chief Executive Officer | January 15, 2025 | [On file] |

**Review Schedule:** This policy shall be reviewed semi-annually or upon any material change in business operations, regulatory landscape, threat environment, or following a significant security incident — whichever occurs first.

---

## Table of Contents

1. Security Policy Statement
2. Scope
3. Data Classification
4. Access Control
5. Encryption Standards
6. Incident Response
7. Business Continuity and Disaster Recovery
8. Vendor and Third-Party Management
9. Employee Security
10. Change Management
11. Data Retention and Disposal
12. Monitoring, Logging, and Auditing
13. Physical Security
14. Network Security
15. Privacy Alignment
16. Policy Enforcement and Exceptions
17. Definitions and Glossary
18. References and Related Documents

---

## 1. Security Policy Statement

**Policy ID:** WOS-ISP-001-SEC
**Effective Date:** January 15, 2025

### 1.1 Purpose

WorkspaceOS provides isolated, fully managed Google Workspace environments to enterprise customers, ensuring that each customer's data, identity infrastructure, administrative controls, and collaboration tools operate within a logically and administratively separated tenancy. The security of these environments is foundational to our business, our contractual obligations, and the trust our customers place in us.

This Information Security Policy establishes the framework of principles, requirements, and governance structures that protect the confidentiality, integrity, and availability of all information assets managed, processed, stored, or transmitted by WorkspaceOS.

### 1.2 Policy Statement

WorkspaceOS is committed to:

- **Protecting customer data** as if it were our own most sensitive asset, recognizing that each isolated Google Workspace environment contains our customers' intellectual property, personal data, and business-critical communications.
- **Maintaining strict tenant isolation** such that no customer can access, infer, or influence another customer's environment, data, configurations, or operational metadata.
- **Implementing defense in depth** through layered technical, administrative, and physical controls aligned with industry best practices and recognized frameworks including ISO/IEC 27001:2022, NIST Cybersecurity Framework 2.0, SOC 2 Type II Trust Services Criteria, and CIS Controls v8.
- **Complying with all applicable laws and regulations** including but not limited to the General Data Protection Regulation (GDPR), California Consumer Privacy Act (CCPA/CPRA), HIPAA where applicable through BAAs, and regional data protection legislation in all jurisdictions where we operate.
- **Continuously improving** our security posture through regular risk assessments, penetration testing, threat intelligence integration, employee training, and lessons learned from incidents and near-misses.
- **Fostering a culture of security** where every employee, contractor, and partner understands their role in protecting information assets and feels empowered to report concerns without fear of retaliation.

### 1.3 Guiding Principles

1. **Least Privilege:** Access to systems, data, and administrative functions shall be limited to the minimum necessary for an individual or service to perform its authorized function.
2. **Zero Trust:** No user, device, or network segment shall be inherently trusted. Verification shall be required continuously and contextually.
3. **Tenant Isolation by Design:** Architectural and operational controls shall ensure complete separation of customer environments at every layer of the technology stack.
4. **Security by Default:** All systems, configurations, and services shall ship with the most restrictive security settings enabled by default.
5. **Transparency:** Customers shall have visibility into the security controls protecting their environments, and shall be notified promptly of incidents affecting their data.
6. **Accountability:** Clear ownership shall be assigned for every information asset, security control, and risk treatment decision.

### 1.4 Management Commitment

The Board of Directors and Executive Leadership Team of WorkspaceOS acknowledge their responsibility for information security governance and commit to providing the resources, authority, and organizational support necessary to implement and maintain this policy and its subordinate standards, procedures, and guidelines.

---

## 2. Scope

**Policy ID:** WOS-ISP-002-SCP
**Effective Date:** January 15, 2025

### 2.1 Organizational Scope

This policy applies to:

- All WorkspaceOS legal entities, subsidiaries, and branches regardless of geographic location.
- All employees (full-time, part-time, temporary) of WorkspaceOS.
- All contractors, consultants, interns, and temporary workers engaged by WorkspaceOS.
- All third-party service providers, vendors, partners, and agents who access, process, store, or transmit WorkspaceOS or customer information.
- The Board of Directors and executive leadership in their governance capacity.

### 2.2 Technical Scope

This policy governs:

- **Customer Workspace Environments:** All isolated Google Workspace tenancies provisioned, managed, and maintained by WorkspaceOS on behalf of customers, including but not limited to Gmail, Google Drive, Google Calendar, Google Meet, Google Chat, Google Vault, Admin Console configurations, and associated APIs.
- **WorkspaceOS Management Platform:** The proprietary orchestration, provisioning, monitoring, and administration platform(s) used by WorkspaceOS to manage customer tenancies.
- **Internal Corporate Systems:** WorkspaceOS's own IT infrastructure, corporate Google Workspace instance, internal tools, development environments, CI/CD pipelines, and supporting systems.
- **Cloud Infrastructure:** All cloud services (Google Cloud Platform, and any supplementary cloud providers) used to host, operate, or support WorkspaceOS services.
- **Network Infrastructure:** All networks, both physical and virtual, including corporate offices, data processing facilities, VPNs, SD-WAN, and cloud virtual networks.
- **Endpoints:** All devices (company-owned and authorized personal devices under BYOD policy) used to access WorkspaceOS systems or customer data.
- **Data in All States:** Data at rest, data in transit, and data in processing across all systems within scope.

### 2.3 Information Scope

This policy covers all information assets including but not limited to:

- Customer data within isolated Workspace environments (emails, files, calendar entries, contacts, chat logs, video recordings, audit logs).
- Customer configuration and administrative metadata.
- WorkspaceOS proprietary source code, algorithms, and intellectual property.
- Employee personal data and HR records.
- Financial and business records.
- Security logs, audit trails, and incident records.
- Vendor and partner contracts and communications.

### 2.4 Exclusions

This policy does not directly govern the internal security practices of customers within their isolated Workspace environments, except where WorkspaceOS is contractually obligated to enforce baseline security configurations (such as minimum password policies, MFA enforcement, or DLP rules) as part of the managed service agreement.

---

## 3. Data Classification

**Policy ID:** WOS-ISP-003-DCL
**Effective Date:** January 15, 2025

### 3.1 Purpose

A consistent data classification framework ensures that information assets receive security controls proportionate to their sensitivity and the impact of unauthorized disclosure, modification, or loss.

### 3.2 Classification Levels

#### 3.2.1 Level 1 — Restricted

**Label:** RESTRICTED
**Color Code:** Red
**Description:** Information whose unauthorized disclosure, alteration, or destruction would cause severe harm to WorkspaceOS, its customers, or individuals. This includes customer data within isolated Workspace tenancies, personally identifiable information (PII) subject to regulatory protection, authentication credentials, encryption keys, security vulnerability details, and information subject to legal privilege.

**Handling Requirements:**
- Access limited to specifically authorized individuals with documented business need.
- Encrypted at rest (AES-256 or equivalent) and in transit (TLS 1.2+ minimum, TLS 1.3 preferred).
- Stored only in approved, access-controlled systems with full audit logging.
- No transmission via unencrypted channels under any circumstances.
- Printing restricted and requires authorization; physical copies must be secured in locked storage and shredded when no longer needed (cross-cut, P-4 minimum).
- Subject to Data Loss Prevention (DLP) monitoring and controls.
- Retention and disposal governed by specific schedules with verified destruction.

#### 3.2.2 Level 2 — Confidential

**Label:** CONFIDENTIAL
**Color Code:** Orange
**Description:** Information intended for internal use whose unauthorized disclosure could cause moderate harm. This includes internal business strategies, financial forecasts, non-public product plans, vendor contracts, internal security policies and standards, aggregate analytics, and employee performance data.

**Handling Requirements:**
- Access limited to WorkspaceOS employees and authorized contractors with relevant business need.
- Encrypted at rest and in transit.
- Stored in access-controlled systems with audit logging.
- External sharing requires explicit authorization from the data owner and appropriate NDAs.
- DLP controls applied to prevent accidental external transmission.

#### 3.2.3 Level 3 — Internal

**Label:** INTERNAL
**Color Code:** Yellow
**Description:** Information intended for general internal use whose unauthorized disclosure would cause limited harm. This includes internal communications, procedural documentation, meeting notes not containing sensitive content, and general operational data.

**Handling Requirements:**
- Accessible to all WorkspaceOS employees; contractor access based on role.
- Should be encrypted in transit; encryption at rest recommended.
- Stored in standard corporate systems.
- Not to be shared externally without review.

#### 3.2.4 Level 4 — Public

**Label:** PUBLIC
**Color Code:** Green
**Description:** Information explicitly approved for public distribution. This includes published marketing materials, public-facing documentation, press releases, and published blog posts.

**Handling Requirements:**
- No access restrictions after approval.
- Must be reviewed and approved for public release by authorized personnel.
- Integrity controls to prevent unauthorized modification of published materials.

### 3.3 Customer Data Classification

All customer data within isolated Workspace environments shall be classified as **Restricted** by default, regardless of the customer's own internal classification of that data. WorkspaceOS personnel shall treat all customer data with the highest level of protection.

### 3.4 Classification Responsibilities

- **Data Owners:** Responsible for assigning initial classification to information assets they create or are accountable for. Department heads serve as default data owners for their functional areas.
- **Data Custodians:** Responsible for implementing and maintaining the technical controls appropriate to the classification level.
- **All Personnel:** Responsible for handling information in accordance with its classification and reporting suspected misclassification.

### 3.5 Reclassification

Information shall be reviewed for appropriate classification when:
- Its content or context changes materially.
- The business need for restricted access changes.
- Regulatory requirements change.
- During periodic reviews (at minimum annually).

Reclassification authority rests with the data owner, with guidance from the Information Security team and Data Protection Officer as needed.

---

## 4. Access Control

**Policy ID:** WOS-ISP-004-ACC
**Effective Date:** January 15, 2025

### 4.1 Purpose

Access controls ensure that only authorized individuals and systems can access information assets, and only to the extent necessary for their legitimate, documented purpose.

### 4.2 Access Control Principles

- **Least Privilege:** Users and services shall receive the minimum level of access required to perform their authorized functions.
- **Need-to-Know:** Access to information shall be granted based on a demonstrated, documented business requirement.
- **Separation of Duties:** Critical functions shall be divided among multiple individuals to prevent fraud, error, and abuse. No single individual shall have the ability to provision a customer environment, access its data, and approve their own access.
- **Zero Trust Architecture:** All access requests shall be verified regardless of network location, with continuous authentication and authorization based on user identity, device health, location, and behavioral context.

### 4.3 Identity and Authentication

#### 4.3.1 Identity Management

- All access to WorkspaceOS systems shall be through unique, individually assigned accounts. Shared accounts are prohibited except where technically unavoidable (e.g., specific service accounts), in which case they must be documented, approved, and subject to enhanced monitoring.
- Identity lifecycle management (provisioning, modification, deprovisioning) shall be integrated with HR processes such that access is granted upon verified onboarding, modified upon role changes within one business day, and revoked immediately upon termination or contract end.
- A centralized Identity Provider (IdP) shall serve as the authoritative source for all WorkspaceOS employee and contractor identities.

#### 4.3.2 Authentication Requirements

| System Category | Minimum Authentication Requirement |
|----------------|-----------------------------------|
| Customer Workspace Admin Access | Phishing-resistant MFA (FIDO2/WebAuthn hardware keys required) + Context-aware access policies |
| WorkspaceOS Management Platform | Phishing-resistant MFA (FIDO2/WebAuthn hardware keys required) + Context-aware access policies |
| Production Infrastructure (GCP) | Phishing-resistant MFA + Just-in-Time (JIT) access with time-limited sessions (maximum 4 hours) |
| Internal Corporate Systems | MFA required (FIDO2 preferred; TOTP acceptable for non-sensitive systems) |
| VPN/Remote Access | MFA required + Device certificate validation |
| API/Service-to-Service | mTLS with certificate-based authentication + OAuth 2.0 with short-lived tokens |

- Passwords, where used in conjunction with MFA, shall meet the following complexity requirements: minimum 14 characters, no maximum length restriction up to 128 characters, checked against known breached password databases, no forced periodic rotation (rotation required only upon suspected compromise per NIST SP 800-63B guidance).
- Session timeouts: Administrative sessions shall time out after 15 minutes of inactivity. Standard sessions shall time out after 60 minutes of inactivity. Re-authentication required for sensitive operations.
- Account lockout: After 5 consecutive failed authentication attempts, the account shall be locked for a minimum of 30 minutes, with alerts generated to the security team after 3 failed attempts.

### 4.4 Authorization

#### 4.4.1 Role-Based Access Control (RBAC)

WorkspaceOS shall implement RBAC with the following defined role categories:

| Role Category | Description | Example Permissions |
|--------------|-------------|-------------------|
| Super Administrator | WorkspaceOS executive-level emergency access | Full platform access with break-glass procedures; requires dual authorization |
| Platform Administrator | Manages the WorkspaceOS orchestration platform | Provision/deprovision tenancies, manage platform configuration; no access to customer data |
| Customer Environment Administrator | Manages specific customer Workspace configurations per customer contract | Modify customer Workspace settings, manage customer user accounts; access scoped to assigned customer(s) only |
| Security Operations | Monitors security, responds to incidents | Access to security logs, SIEM, incident management tools; read-only customer metadata access for investigation (with approval) |
| Engineering | Develops and maintains the WorkspaceOS platform | Access to development/staging environments, code repositories, CI/CD; no production customer data access |
| Support | Provides technical support to customers | Access to customer-authorized troubleshooting tools; no direct customer data access without explicit customer consent and ticket documentation |
| Read-Only/Audit | Internal and external auditors | Read-only access to specified logs, configurations, and documentation |

#### 4.4.2 Customer Data Access Controls

Access to customer data within isolated Workspace environments is the most tightly controlled category:

- **Default Deny:** No WorkspaceOS employee shall have standing access to any customer's Workspace data (emails, files, chat messages, etc.).
- **Just-in-Time Access:** When access