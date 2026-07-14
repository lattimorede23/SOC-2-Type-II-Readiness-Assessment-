# System Description — Lattimore Ledger Payroll & HR Platform

**DISCLAIMER — FICTIONAL TRAINING EXERCISE.** This document is part of a SOC 2 Type II readiness assessment created as a portfolio and training exercise. **Lattimore Ledger is a fictional entity invented for this exercise. It is not a real operating company, has no customers or employees, and is not affiliated with any real business, past or present.** This document is not a SOC 2 report and was not prepared by or for a licensed CPA firm.

|  |  |
| :---- | :---- |
| **Document** | SYSTEM\_DESCRIPTION.md (1 of 4\) |
| **Prepared by** | Derrick Lattimore, Assessor |
| **Assessment date** | July 2026 |
| **Framework** | AICPA 2017 Trust Services Criteria (with Revised Points of Focus — 2022\) |
| **Categories in scope** | Security (Common Criteria), Availability, Confidentiality |
| **Related documents** | CONTROLS.md, GAP\_ASSESSMENT.md, EXEC\_SUMMARY.md |

---

## 1\. Company Overview

Lattimore Ledger is a fictional Series B B2B SaaS company (\~120 employees) providing a cloud-hosted payroll and HR platform for mid-market employers (roughly 200–5,000 employees per customer). Customers use the platform to onboard employees, manage HR records, run payroll, and remit direct deposits.

The service processes highly sensitive employee data on behalf of customers, including Social Security numbers, bank routing and account numbers, compensation data, and home addresses. Customer procurement and vendor due diligence teams routinely request a SOC 2 Type II report; the absence of one is beginning to block enterprise deals, which is the business driver for this readiness assessment.

### 1.1 Service Commitments and System Requirements

Lattimore Ledger's principal service commitments, as stated in its Master Service Agreement (MSA) and service documentation:

- **Security:** Customer data is protected against unauthorized access, both logical and physical (physical protection inherited from AWS).  
- **Availability:** The platform maintains a 99.9% monthly uptime commitment, with heightened operational sensitivity during customer payroll processing windows (typically the 13th–15th and 28th–31st of each month).  
- **Confidentiality:** Customer employee data is used solely to deliver the service, is accessible only to authorized personnel with a business need, and is deleted or returned upon contract termination per MSA terms.

These commitments define why the assessment scope is Security \+ Availability \+ Confidentiality. See Section 6 for the scoping rationale on the two excluded categories.

## 2\. System Boundaries

**In scope:**

- The production Lattimore Ledger platform (web application, APIs, background payroll processing services) and the AWS environment hosting it  
- The CI/CD pipeline that builds and deploys production code  
- Corporate identity and access management (Okta) as it governs access to production and to systems processing customer data  
- Supporting organizational processes: personnel onboarding/offboarding, security policy management, vendor management, incident response

**Out of scope:**

- The public marketing website (static site, no customer data, separately hosted)  
- Corporate endpoint management and office networking, except where endpoints are the access path to production (addressed within logical access controls)  
- Financial reporting controls (SOC 1 territory)

## 3\. Infrastructure and Architecture

All production infrastructure runs in AWS, primary region us-east-1.

| Component | Implementation |
| :---- | :---- |
| Compute | Containerized microservices on Amazon EKS (Kubernetes) |
| Database | Amazon RDS for PostgreSQL, Multi-AZ. **Multi-tenant, shared-schema model**: every row carries a `tenant_id`, and application-layer middleware scopes all queries to the authenticated tenant |
| Object storage | Amazon S3 (payroll documents, generated reports, uploaded files) |
| Encryption | AWS KMS-managed keys; encryption at rest on RDS, S3, and EBS; TLS 1.2+ enforced for all external connections |
| Edge / ingress | CloudFront \+ Application Load Balancer \+ AWS WAF |
| Infrastructure as code | Terraform, stored in GitHub, applied through the CI/CD pipeline |
| CI/CD | GitHub \+ GitHub Actions; deploys to EKS via pipeline service account |
| Identity | Okta (SSO \+ SCIM provisioning) for workforce access to SaaS tools and AWS (via IAM Identity Center) |
| Monitoring & logging | Datadog (metrics, APM, log aggregation, alerting); AWS CloudTrail and GuardDuty enabled in all accounts |

**Legacy components (relevant to findings):** Two production-adjacent systems predate the Okta rollout and sit outside SSO:

1. **Legacy admin console** — an internal support tool used by Customer Success to view customer records; local username/password authentication.  
2. **Customer SFTP portal** — used by a minority of customers to bulk-upload employee data files; runs on a standalone EC2 instance with locally managed credentials.

Both are referenced in CONTROLS.md (SEC-06) and GAP\_ASSESSMENT.md.

## 4\. Data Flows

1. **Inbound employee data:** Customer HR administrators enter or bulk-upload employee records through the web application (TLS), or via the legacy SFTP portal. Data lands in RDS; uploaded files persist in S3.  
2. **Payroll processing:** On a customer-initiated payroll run, background services calculate gross-to-net pay and generate an ACH file.  
3. **Funds movement:** ACH files are transmitted over an encrypted connection to **Firstline ACH Services (fictional)**, the platform's money-movement subprocessor, which originates direct deposits. Lattimore Ledger does not hold customer funds.  
4. **Outbound notifications:** Transactional email (pay stub notifications, password resets) is sent via SendGrid. Email bodies contain no compensation or banking data; they link back to the authenticated portal.  
5. **Support access:** Customer Success staff access customer records through the legacy admin console to resolve tickets.

## 5\. Subprocessors and Key Vendors

| Vendor | Function | Data exposure |
| :---- | :---- | :---- |
| Amazon Web Services | Infrastructure hosting | All customer data (encrypted) |
| Firstline ACH Services *(fictional)* | ACH origination / direct deposit | Bank account/routing numbers, net pay amounts, employee names |
| Okta | Workforce identity | Employee identity attributes; no customer data |
| GitHub | Source code, CI/CD | Source code; no customer data by policy |
| Datadog | Monitoring, log aggregation | Logs and telemetry (application logs may transit; log hygiene addressed in CONF-02) |
| SendGrid | Transactional email | Employee names and email addresses |

Physical and environmental security controls for production infrastructure are inherited from AWS and carved out of this assessment; AWS's own SOC 2 report is relied upon (the "carve-out" method).

## 6\. Scoping Rationale — Excluded Categories

**Processing Integrity — excluded, but this is a genuine judgment call.** An argument exists that payroll calculation accuracy (gross-to-net, tax withholding) is the core of what customers buy, which is squarely Processing Integrity territory. It is excluded here because (a) customer due diligence requests to date ask for Security/Availability/Confidentiality, and (b) adding PI would require controls over calculation validation and tax-table change management that meaningfully expand a first audit. **Recommendation: revisit PI for the second audit cycle.** A reviewer could reasonably disagree with deferring it.

**Privacy — excluded.** The platform processes personal information (SSNs, addresses) but does so as a processor on behalf of business customers. The Privacy category addresses commitments made to data subjects (notice, choice, consent); Lattimore Ledger's commitments run to its business customers and are confidentiality commitments contractually. Data subjects' relationship is with the employer, not the platform. This is the conventional scoping for a B2B processor, but it should be re-examined if the company ever offers employee-facing features that create direct commitments to data subjects.

## 7\. Organization and Security-Relevant Roles

Lattimore Ledger has **no dedicated compliance or GRC function**; security responsibility is concentrated in one engineer. This structure is itself relevant to several findings.

| Role | Security-relevant responsibilities |
| :---- | :---- |
| Chief Executive Officer | Ultimate accountability; approves security budget |
| Chief Technology Officer (CTO) | Executive owner of security program; Security Engineer reports here |
| VP of Engineering | Owns SDLC, change management, and engineering staffing; DevOps Lead reports here |
| Security Engineer (1 FTE) | Vulnerability management, security tooling, incident response, policy drafting |
| DevOps Lead | AWS infrastructure, CI/CD, backups, monitoring, capacity |
| People Operations Manager | Onboarding/offboarding initiation, background checks, training logistics |
| VP of Customer Success | Owns support staff who access customer data via the admin console |

All roles named as remediation owners in GAP\_ASSESSMENT.md are drawn from this table.  
