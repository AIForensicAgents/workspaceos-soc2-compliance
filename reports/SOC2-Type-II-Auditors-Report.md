# Independent Service Auditor's Report on a Description of a System and the Suitability of the Design and Operating Effectiveness of Controls

## SOC 2® Type II Report

### WorkspaceOS, Inc.

### A SaaS Platform Providing Isolated Google Workspace Environments

**Report on WorkspaceOS's System Relevant to Security, Availability, Processing Integrity, Confidentiality, and Privacy**

**Period: January 1, 2024 through December 31, 2024**

**Prepared by:**
Deloitte & Touche LLP
30 Rockefeller Plaza
New York, NY 10112

**Report Date:** February 14, 2025

---

*This report is intended solely for the information and use of WorkspaceOS, Inc., user entities of WorkspaceOS's system during some or all of the specified period, business partners of WorkspaceOS who have a sufficient understanding to consider it along with other information, and prospective user entities and business partners, and is not intended to be and should not be used by anyone other than these specified parties.*

---

# TABLE OF CONTENTS

| Section | Page |
|---|---|
| **I. Independent Service Auditor's Report** | 3 |
| **II. Management's Assertion of WorkspaceOS, Inc.** | 8 |
| **III. Description of the WorkspaceOS System** | 11 |
| **IV. Trust Services Criteria, Related Controls, Tests of Controls, and Results of Tests** | 34 |
| CC1.0 – Control Environment | 34 |
| CC2.0 – Communication and Information | 42 |
| CC3.0 – Risk Assessment | 48 |
| CC4.0 – Monitoring Activities | 53 |
| CC5.0 – Control Activities | 58 |
| CC6.0 – Logical and Physical Access Controls | 64 |
| CC7.0 – System Operations | 78 |
| CC8.0 – Change Management | 88 |
| CC9.0 – Risk Mitigation | 95 |
| A1.0 – Availability | 100 |
| C1.0 – Confidentiality | 108 |
| PI1.0 – Processing Integrity | 115 |
| P1.0 – Privacy | 122 |
| **V. Other Information Provided by WorkspaceOS, Inc.** | 133 |

---

# SECTION I: INDEPENDENT SERVICE AUDITOR'S REPORT

## Report of Independent Service Auditor

**To the Board of Directors and Management of WorkspaceOS, Inc.:**

### Scope

We have examined WorkspaceOS, Inc.'s ("WorkspaceOS" or the "Service Organization") description of its SaaS platform providing isolated Google Workspace environments titled "Description of the WorkspaceOS System" throughout the period January 1, 2024 to December 31, 2024 (the "Description") in Section III, and the suitability of the design and operating effectiveness of controls stated in the Description throughout the period January 1, 2024 to December 31, 2024, to provide reasonable assurance that WorkspaceOS's service commitments and system requirements were achieved based on the trust services criteria relevant to security, availability, processing integrity, confidentiality, and privacy (the "applicable trust services criteria") set forth in TSP Section 100, *2017 Trust Services Criteria for Security, Availability, Processing Integrity, Confidentiality, and Privacy*, issued by the Assurance Services Executive Committee of the American Institute of Certified Public Accountants ("AICPA").

### Service Organization's Responsibilities

WorkspaceOS is responsible for:

1. Preparing the Description and the assertion in Section II, including the completeness, accuracy, and method of presentation of the Description and the assertion;
2. Providing the services covered by the Description;
3. Specifying the controls that meet the applicable trust services criteria and stating them in the Description;
4. Designing, implementing, and documenting controls that are suitably designed and operating effectively to meet the applicable trust services criteria.

### Service Auditor's Responsibilities

Our responsibility is to express an opinion on the fairness of the presentation of the Description and on the suitability of the design and operating effectiveness of the controls stated in the Description, based on our examination.

We conducted our examination in accordance with attestation standards established by the AICPA and in accordance with the standards of the Public Company Accounting Oversight Board (United States). Those standards require that we plan and perform our examination to obtain reasonable assurance about whether, in all material respects:

1. The Description is fairly presented based on the description criteria; and
2. The controls stated in the Description were suitably designed and operated effectively throughout the period January 1, 2024 to December 31, 2024, to provide reasonable assurance that the service organization's service commitments and system requirements were achieved based on the applicable trust services criteria.

Our examination involved performing procedures to obtain evidence about the fairness of the presentation of the Description and the suitability of the design and operating effectiveness of the controls stated in the Description. Our procedures included:

- Obtaining an understanding of the system and the service organization's service commitments and system requirements
- Assessing the risks that the Description is not fairly presented and that controls were not suitably designed or did not operate effectively
- Performing procedures to obtain evidence about whether the Description is fairly presented and the controls stated therein were suitably designed and operated effectively to meet the applicable trust services criteria throughout the specified period
- Testing the operating effectiveness of controls stated in the Description through inquiry, observation, inspection of documents and records, and re-performance of the controls
- Evaluating the overall presentation of the Description

### Inherent Limitations

The Description is prepared to meet the common needs of a broad range of user entities, user entities' auditors, and other specified parties and may not, therefore, include every aspect of the system that each individual user entity may consider important in its own particular environment. Because of their nature and inherent limitations, controls at a service organization may not always operate effectively to meet the applicable trust services criteria. Also, the projection to the future of any evaluation of the fairness of the presentation of the Description or conclusions about the suitability of the design or operating effectiveness of the controls is subject to the risk that the system may change or that controls at a service organization may become inadequate or fail.

### Description Criteria

The Description is required to be presented in accordance with the description criteria. We have not evaluated the fairness of the presentation of the description according to any other criteria.

### Complementary User Entity Controls and Complementary Subservice Organization Controls

The Description in Section III indicates that certain trust services criteria specified therein can be met only if complementary user entity controls ("CUECs") and complementary subservice organization controls ("CSOCs") assumed in the design of WorkspaceOS's controls are suitably designed and operating effectively, along with the related controls at the service organization. Our examination did not extend to such complementary user entity controls or complementary subservice organization controls, and we have not evaluated the suitability of the design or operating effectiveness of such controls.

### Subservice Organizations

The Description indicates that WorkspaceOS uses Google LLC ("Google Cloud Platform" and "Google Workspace") and Amazon Web Services, Inc. ("AWS") as subservice organizations. The Description includes only the controls and related trust services criteria of WorkspaceOS and excludes the controls and related trust services criteria of the subservice organizations. The Description also indicates that certain trust services criteria can be met only if the controls at the subservice organizations and related CSOCs are suitably designed and operating effectively. Our examination did not extend to controls of the subservice organizations. WorkspaceOS has adopted the "carve-out" method of reporting with respect to Google LLC and Amazon Web Services, Inc.

### Opinion

In our opinion, in all material respects:

a) The Description fairly presents the system that was designed and implemented throughout the period January 1, 2024 to December 31, 2024, in accordance with the description criteria;

b) The controls stated in the Description were suitably designed to provide reasonable assurance that the service organization's service commitments and system requirements would be achieved based on the applicable trust services criteria, if the controls operated effectively throughout the period January 1, 2024 to December 31, 2024, and if the subservice organizations and user entities applied the complementary controls assumed in the design of the service organization's controls throughout the period January 1, 2024 to December 31, 2024; and

c) The controls stated in the Description operated effectively to provide reasonable assurance that the service organization's service commitments and system requirements were achieved based on the applicable trust services criteria throughout the period January 1, 2024 to December 31, 2024, if the complementary subservice organization controls and complementary user entity controls assumed in the design of the service organization's controls operated effectively throughout the period January 1, 2024 to December 31, 2024.

### Restricted Use

This report, including the description of tests of controls and results thereof in Section IV, is intended solely for the information and use of WorkspaceOS, user entities of WorkspaceOS's system during some or all of the specified period, business partners of WorkspaceOS who have a sufficient understanding to consider it along with other information, prospective user entities and business partners, and regulators who have sufficient knowledge and understanding of the following:

- The nature of the service provided by the service organization
- How the service organization's system interacts with user entities, business partners, subservice organizations, and other parties
- Internal control and its limitations
- Complementary user entity controls and complementary subservice organization controls and how those controls interact with the controls at the service organization to meet the applicable trust services criteria
- The applicable trust services criteria
- The risks that may threaten the achievement of the applicable trust services criteria and how controls address those risks

This report is not intended to be and should not be used by anyone other than these specified parties.

**Deloitte & Touche LLP**

New York, New York
February 14, 2025

---

# SECTION II: MANAGEMENT'S ASSERTION OF WORKSPACEOS, INC.

## Assertion of WorkspaceOS, Inc. Management

**To the Board of Directors and Stakeholders of WorkspaceOS, Inc.:**

We are responsible for designing, implementing, operating, and maintaining effective controls within the WorkspaceOS system throughout the period January 1, 2024 to December 31, 2024, to provide reasonable assurance that WorkspaceOS's service commitments and system requirements relevant to security, availability, processing integrity, confidentiality, and privacy were achieved.

We have prepared the accompanying Description of the WorkspaceOS system for the period January 1, 2024 to December 31, 2024 (the "Description") in Section III based on the criteria for a description of a service organization's system in DC Section 200, *2018 Description Criteria for a Description of a Service Organization's System in a SOC 2® Report* (the "description criteria"). The Description is intended to provide user entities of WorkspaceOS's system, their auditors, and other specified parties with information about the WorkspaceOS system, particularly system controls intended to meet the criteria for the security, availability, processing integrity, confidentiality, and privacy trust services categories.

We confirm, to the best of our knowledge and belief, that:

**a)** The Description fairly presents the WorkspaceOS system that was designed and implemented throughout the period January 1, 2024 to December 31, 2024, in accordance with the description criteria. The matters covered in the Description include, but are not limited to, the following:

- The types of services provided, including classes of transactions processed
- The principal service commitments and system requirements
- The components of the system used to provide the services, which are as follows: infrastructure, software, people, procedures, and data
- For identified trust services criteria that are addressed by controls at WorkspaceOS, the applicable complementary user entity controls and complementary subservice organization controls along with the types of controls expected to be implemented at the subservice organizations (Google LLC and Amazon Web Services, Inc.)
- Relevant aspects of the control environment, risk assessment process, information and communication systems, and monitoring of controls
- Applicable trust services criteria and related controls

**b)** The controls stated in the Description were suitably designed and operated effectively throughout the period January 1, 2024 to December 31, 2024, to meet the applicable trust services criteria, if the complementary subservice organization controls assumed in the design of WorkspaceOS's controls operated effectively throughout the period January 1, 2024 to December 31, 2024, and user entities of the system and their auditors applied the complementary user entity controls assumed in the design of WorkspaceOS's controls throughout the period January 1, 2024 to December 31, 2024.

The service commitments in the principal service terms of contracts with user entities and the system requirements identified as being in scope for this engagement are listed below:

| Category | Service Commitments and System Requirements |
|---|---|
| **Security** | The system is protected against unauthorized access, both physical and logical, in accordance with WorkspaceOS's security policies and applicable service level agreements. |
| **Availability** | The system is available for operation and use as committed or agreed, with a target of 99.9% uptime, as specified in service level agreements. |
| **Processing Integrity** | System processing is complete, valid, accurate, timely, and authorized to meet the entity's objectives, including the provisioning, modification, and deprovisioning of isolated Google Workspace tenants. |
| **Confidentiality** | Information designated as confidential is protected as committed or agreed, including tenant isolation ensuring that customer data remains segregated within individual Google Workspace instances. |
| **Privacy** | Personal information is collected, used, retained, disclosed, and disposed of in conformity with the commitments in WorkspaceOS's Privacy Notice and with criteria set forth in generally accepted privacy principles ("GAPP") issued by the AICPA and CICA. |

**WorkspaceOS, Inc.**

_______________________________
**Marcus J. Chen**
Chief Executive Officer

_______________________________
**Priya R. Venkataraman**
Chief Technology Officer

_______________________________
**David A. Kowalski**
Chief Information Security Officer

February 14, 2025

---

# SECTION III: DESCRIPTION OF THE WORKSPACEOS SYSTEM

## 1. Company Overview

### 1.1 Background

WorkspaceOS, Inc. ("WorkspaceOS" or the "Company") is a Delaware-incorporated software-as-a-service ("SaaS") company founded in 2019 and headquartered in San Francisco, California, with engineering offices in Austin, Texas and Toronto, Ontario, Canada. WorkspaceOS provides a multi-tenant management platform that automates the provisioning, configuration, monitoring, and lifecycle management of isolated Google Workspace environments for its customers ("user entities" or "tenants"). Each customer receives a fully dedicated Google Workspace instance—including Gmail, Google Drive, Google Calendar, Google Meet, and the full suite of Google Workspace productivity applications—that is logically and administratively isolated from all other customer instances.

As of December 31, 2024, WorkspaceOS serves approximately 2,800 active customer organizations, managing over 340,000 individual Google Workspace user accounts across 14 countries. The Company employs 287 full-time employees and 34 contractors.

### 1.2 Principal Services Provided

WorkspaceOS's platform provides the following principal services to user entities:

**Automated Workspace Provisioning:** Upon customer onboarding, the WorkspaceOS platform programmatically creates a new, isolated Google Workspace instance via the Google Workspace Admin SDK and Google Cloud Identity APIs. Each instance is configured according to the customer's selected service tier (Starter, Business, Enterprise) and organizational requirements.

**Tenant Isolation and Governance:** Each customer Google Workspace is established under a unique organizational unit within Google's infrastructure, with dedicated domain routing, separate data residency configurations (where applicable), and administratively isolated identity and access management boundaries. Customers cannot access, view, or interact with data belonging to other tenants.

**Centralized Administration Console:** WorkspaceOS provides a web-based administration portal (admin.workspaceos.com) through which authorized customer administrators can manage users, groups, organizational policies, security settings, data loss prevention ("DLP") rules, and compliance configurations for their isolated Google Workspace.

**Identity and Access Management ("IAM") Orchestration:** The platform integrates with customer identity providers ("IdPs") via SAML 2.0 and OIDC protocols to support federated single sign-on ("SSO"), multi-factor authentication ("MFA") enforcement, and automated user lifecycle management (provisioning, modification, deprovisioning) via SCIM 2.0.

**Compliance and Security Policy Management:** WorkspaceOS enables centralized enforcement of security baselines, including MFA requirements, password policies, mobile device management ("MDM"), data loss prevention rules, and audit log forwarding.

**Monitoring, Alerting, and Reporting:** The platform provides real-time monitoring dashboards, security event alerting, usage analytics, and compliance reporting for customer administrators and WorkspaceOS's internal operations team.

**Backup and Recovery Services:** Automated daily backups of customer Google Workspace data (Gmail, Drive, Shared Drives) are performed using WorkspaceOS's proprietary backup engine