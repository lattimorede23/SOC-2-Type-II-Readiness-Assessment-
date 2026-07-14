# Control Narratives — Lattimore Ledger SOC 2 Readiness Assessment

**DISCLAIMER — FICTIONAL TRAINING EXERCISE.** This document is part of a SOC 2 Type II readiness assessment created as a portfolio and training exercise. **Lattimore Ledger is a fictional entity invented for this exercise. It is not a real operating company and is not affiliated with any real business.** All controls, implementation states, and findings are fictional. This is not a SOC 2 report.

|  |  |
| :---- | :---- |
| **Document** | CONTROLS.md (2 of 4\) |
| **Prepared by** | Derrick Lattimore, Assessor |
| **Framework** | AICPA 2017 Trust Services Criteria (with Revised Points of Focus — 2022\) |
| **Related documents** | SYSTEM\_DESCRIPTION.md (system context), GAP\_ASSESSMENT.md (gaps for any control below marked less than Implemented), EXEC\_SUMMARY.md |

**Status key:** **Implemented** — operating and expected to produce evidence over a Type II period. **Partially Implemented** — control exists but has a design or operating deficiency; matching row in GAP\_ASSESSMENT.md. **Not Implemented** — control does not exist; matching row in GAP\_ASSESSMENT.md.

**Note on control counts:** A full SOC 2 examination maps controls to all \~33 common criteria plus category criteria, typically producing 60–100+ controls. This readiness assessment deliberately samples 20 controls across the highest-risk areas for a first-time candidate. That sampling choice is stated here so the package is not mistaken for full-population coverage.

---

## Security (Common Criteria)

### SEC-01 — Security Governance and Policy Management

**TSC mapping:** CC1.1, CC1.3, CC5.3 **Description:** Management maintains a documented information security policy set covering acceptable use, access control, data handling, incident response, and change management. Policies are approved by the CTO, reviewed at least annually, and acknowledged by all personnel at hire and upon material revision. The CTO is the designated executive owner of the security program, with day-to-day execution assigned to the Security Engineer (see SYSTEM\_DESCRIPTION.md §7). **Status:** **Partially Implemented.** Policy documents exist (drafted by the Security Engineer) but have never been formally approved by management, carry no review cadence, and acknowledgment is not tracked. → GAP-01

### SEC-02 — Security Awareness Training

**TSC mapping:** CC1.4, CC2.2 **Description:** All personnel complete security awareness training at hire and annually thereafter, covering phishing, credential handling, and the confidentiality obligations attached to customer payroll data. Completion is tracked by People Operations, and non-completion is escalated to the employee's manager. **Status:** **Not Implemented.** Training is currently an informal slide deck shown during onboarding, with no annual refresh and no completion tracking. → GAP-02

### SEC-03 — Personnel Screening

**TSC mapping:** CC1.4 **Description:** Background checks (identity, criminal history, and — for roles with access to banking data — credit history where permitted by law) are completed before any employee or contractor is granted access to production systems or customer data. People Operations initiates screening; access is not provisioned until results are cleared. **Status:** **Partially Implemented.** Background checks are consistently run for full-time employees but were skipped for three of five current engineering contractors, two of whom hold production access. → GAP-03

### SEC-04 — Risk Assessment Process

**TSC mapping:** CC3.1, CC3.2, CC3.4 **Description:** Management performs a documented risk assessment at least annually and upon significant change (new subprocessor, major architectural change, new data type). The assessment identifies threats to the achievement of service commitments, rates likelihood and impact, and produces a treatment plan with owners. Results are reviewed with the CEO. **Status:** **Not Implemented.** No formal risk assessment has ever been performed. Risk decisions are made ad hoc in engineering leadership meetings without documentation. This is foundational: several other criteria (CC3.x, CC9.1) expect controls to be responsive to an identified risk baseline. → GAP-04

### SEC-05 — Access Provisioning and Deprovisioning

**TSC mapping:** CC6.1, CC6.2, CC6.3 **Description:** Workforce access is provisioned through Okta based on role-mapped groups; SCIM pushes account creation and deactivation to integrated applications. Upon termination, People Operations triggers offboarding in the HR system, which deactivates the Okta identity and, via SCIM, downstream application access. Production AWS access is granted through IAM Identity Center groups tied to Okta. **Status:** **Partially Implemented.** The Okta/SCIM flow operates for integrated systems, but the two legacy systems outside SSO (admin console, SFTP portal — SYSTEM\_DESCRIPTION.md §3) require manual deprovisioning with no checklist, and no one measures whether termination-day deactivation actually occurs. Sampled terminations found one former Customer Success employee's admin-console account active 47 days post-termination. → GAP-05

### SEC-06 — Authentication and MFA

**TSC mapping:** CC6.1 **Description:** All workforce authentication to systems in the assessment boundary requires SSO through Okta with phishing-resistant MFA enforced. Password policy (length, rotation on compromise, breach screening) is enforced centrally in Okta. **Status:** **Partially Implemented.** MFA is enforced for all Okta-fronted access including AWS. The legacy admin console and customer SFTP portal authenticate with local credentials and no MFA — and the admin console exposes bulk customer payroll data to support staff. → GAP-06 **Judgment call flagged for second review:** this could be written as a *design* deficiency (the control as designed excludes two systems) rather than an *operating* deficiency. I've treated it as design, because the control language claims coverage of "all systems in the assessment boundary" and the legacy systems are in-boundary. A reviewer might instead narrow the control language to Okta-fronted systems and raise the legacy gap purely in the gap assessment — that would make the control "Implemented" on paper while the same risk persists, which I think is the wrong call, but it is a defensible alternative reading.

### SEC-07 — Periodic Access Reviews

**TSC mapping:** CC6.2, CC6.3 **Description:** System owners review user access to production AWS, the production database, the admin console, and Okta group membership quarterly. Reviews confirm each account maps to an active employee with a current business need; exceptions are remediated within 10 business days and reviews are retained as evidence. **Status:** **Not Implemented.** No periodic access review has ever been conducted. Access accretes as employees change roles. → GAP-07

### SEC-08 — Encryption of Data at Rest and in Transit

**TSC mapping:** CC6.1, CC6.7 (supports C1.1) **Description:** Customer data is encrypted at rest using AWS KMS-managed keys across RDS, S3, and EBS. TLS 1.2+ is enforced for all external connections, including transmission of ACH files to Firstline ACH Services. Key management (rotation, access to KMS administrative actions) is restricted to the DevOps Lead and Security Engineer. **Status:** **Implemented.** Verified via Terraform configuration and AWS account inspection. No gap row.

### SEC-09 — Vulnerability Management

**TSC mapping:** CC7.1 **Description:** Container images and dependencies are scanned in the CI/CD pipeline; the production environment is scanned on a recurring schedule. Findings are triaged by severity with defined remediation SLAs (e.g., critical: 15 days; high: 30 days), tracked to closure, and reported to the CTO monthly. **Status:** **Partially Implemented.** Pipeline scanning exists and blocks builds on critical CVEs. However, there are no remediation SLAs, no tracking of findings to closure outside the pipeline, and no recurring authenticated scanning of the running environment. → GAP-08

### SEC-10 — Logging and Security Monitoring

**TSC mapping:** CC7.2 **Description:** CloudTrail, GuardDuty, and application logs are centralized in Datadog. Alerts are defined for security-relevant events (GuardDuty findings, IAM policy changes, anomalous admin-console activity, failed authentication bursts) and route to an on-call rotation. Alert handling is documented and reviewed. **Status:** **Partially Implemented.** Log centralization and GuardDuty are in place, and infrastructure alerts route to the DevOps on-call. But there is no documented triage procedure, no defined set of security alert use cases (admin-console activity is not monitored at all), and no evidence of alert review. → GAP-09

### SEC-11 — Incident Response

**TSC mapping:** CC7.3, CC7.4, CC7.5 **Description:** A documented incident response plan defines severity levels, roles, escalation paths, customer and regulatory notification obligations (material for breaches of SSN/banking data), and post-incident review requirements. The plan is tested at least annually via tabletop exercise. **Status:** **Partially Implemented.** An IR plan document exists (drafted by the Security Engineer) but has never been tested, does not define customer notification triggers or timelines, and names individuals rather than roles (two of whom have left the company). → GAP-10

### SEC-12 — Change Management

**TSC mapping:** CC8.1 **Description:** All production changes flow through the CI/CD pipeline: changes require a pull request with at least one independent reviewer approval, automated tests, and pipeline-controlled deployment. Direct production changes are prohibited; emergency changes follow a documented expedited path with retroactive review. **Status:** **Partially Implemented.** PR review and pipeline-gated deploys are enforced via branch protection. However, two senior engineers retain standing direct access to production infrastructure and the production database "for emergencies," there is no documented emergency change procedure, and no retroactive review of out-of-band changes. → GAP-11

### SEC-13 — Vendor and Subprocessor Risk Management

**TSC mapping:** CC9.2 **Description:** Before engagement and annually thereafter, vendors with access to customer data (SYSTEM\_DESCRIPTION.md §5) are risk-assessed: security posture review (SOC 2 report or equivalent), contractual data protection terms, and confirmation of breach notification obligations. The subprocessor list is maintained and material changes are communicated to customers per MSA terms. **Status:** **Not Implemented.** No vendor review process exists. AWS and Okta SOC 2 reports have never been collected or reviewed; the Firstline ACH Services contract predates the current MSA and its data protection terms have not been evaluated — a significant exposure given it receives banking data. → GAP-12

### SEC-14 — Tenant Data Isolation

**TSC mapping:** CC6.1 (supports C1.1) **Description:** The multi-tenant, shared-schema architecture (SYSTEM\_DESCRIPTION.md §3) enforces tenant isolation through application middleware that scopes every query to the authenticated `tenant_id`. Isolation is protected by mandatory code review on data-access code paths and automated tests asserting cross-tenant queries fail. **Status:** **Partially Implemented.** Middleware enforcement exists and code review is mandatory (SEC-12), but there is no automated cross-tenant test suite and no periodic penetration testing targeting tenant isolation. For a shared-schema payroll platform, isolation assurance resting solely on code review is thin. → GAP-13 **Judgment call flagged for second review:** whether tenant isolation is a *control* or an *architectural property* is debatable — some assessors would describe it only in the system description. I've written it as a control because the ongoing activities (review, testing) are what keep the property true, and an auditor will ask how isolation is assured, not just how it's designed.

## Availability

### AVL-01 — Capacity Management and Scaling

**TSC mapping:** A1.1 **Description:** Production capacity is monitored in Datadog with alert thresholds on compute, database, and queue depth. EKS workloads autoscale; capacity is explicitly reviewed ahead of month-end payroll processing windows, the platform's peak-load and highest-sensitivity periods (SYSTEM\_DESCRIPTION.md §1.1). **Status:** **Implemented.** Monitoring, autoscaling, and pre-payroll-window checks are operating. No gap row.

### AVL-02 — Backup and Restoration

**TSC mapping:** A1.2 **Description:** RDS automated backups (point-in-time recovery, 35-day retention) and S3 versioning are enabled. Backup jobs are monitored for failure. Restoration is tested at least annually by performing a full restore to an isolated environment and validating data integrity, with results documented. **Status:** **Partially Implemented.** Backups run and are monitored. A restoration has never been tested. Untested backups are an assumption, not a control. → GAP-14

### AVL-03 — Disaster Recovery and Business Continuity

**TSC mapping:** A1.2, A1.3 (with CC7.5) **Description:** A documented disaster recovery plan defines RTO/RPO targets consistent with the 99.9% availability commitment and payroll processing deadlines, covers regional failure scenarios for the single-region (us-east-1) architecture, and is tested annually. **Status:** **Not Implemented.** No DR plan exists; RTO/RPO have never been defined. The architecture is single-region, and a regional event during a payroll window would directly breach both the availability commitment and customers' legal payday obligations. → GAP-15

## Confidentiality

### CONF-01 — Data Classification

**TSC mapping:** C1.1 **Description:** A data classification policy defines categories (e.g., Restricted — SSNs, banking data; Confidential — compensation, HR records; Internal; Public) with mandatory handling requirements per category, including where each class may be stored and who may access it. Classification drives access decisions in SEC-05/SEC-07. **Status:** **Not Implemented.** No classification scheme exists; handling expectations are tribal knowledge. → GAP-16

### CONF-02 — Confidential Data Handling in Non-Production Contexts

**TSC mapping:** C1.1 **Description:** Confidential customer data is prohibited from non-production environments and from log output. Staging/test environments use synthetic or masked data; application logging standards prohibit emission of SSNs, bank account numbers, and compensation values, with automated log scrubbing as a backstop before logs reach Datadog. **Status:** **Partially Implemented.** Staging uses masked data by convention, but the convention is undocumented and unverified. Application logs were sampled during this assessment and full bank account numbers were found in payroll-service debug logs shipped to Datadog — meaning Restricted data currently sits in a third-party log platform outside its intended boundary. → GAP-17

### CONF-03 — Data Retention and Disposal

**TSC mapping:** C1.2 **Description:** A retention schedule defines how long each class of customer data is kept, consistent with MSA commitments and payroll record-keeping requirements. Upon contract termination, customer data is deleted or returned within the contractual window, with deletion verified and evidenced (including S3 objects and backups aging out). **Status:** **Not Implemented.** No retention schedule exists. Two former customers' full datasets remain in production two years after offboarding, in direct tension with the MSA deletion commitment (SYSTEM\_DESCRIPTION.md §1.1). → GAP-18  
