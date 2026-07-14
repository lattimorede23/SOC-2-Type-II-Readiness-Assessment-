# Gap Assessment — Lattimore Ledger SOC 2 Readiness Assessment

**DISCLAIMER — FICTIONAL TRAINING EXERCISE.** This document is part of a SOC 2 Type II readiness assessment created as a portfolio and training exercise. **Lattimore Ledger is a fictional entity invented for this exercise. It is not a real operating company and is not affiliated with any real business.** All gaps, ratings, and remediation plans are fictional. This is not a SOC 2 report.

|  |  |
| :---- | :---- |
| **Document** | GAP\_ASSESSMENT.md (3 of 4\) |
| **Prepared by** | Derrick Lattimore, Assessor |
| **Assessment date** | July 2026 |
| **Related documents** | CONTROLS.md (source control narratives — every gap ID below traces to a control ID there), SYSTEM\_DESCRIPTION.md (all named systems, vendors, and roles), EXEC\_SUMMARY.md (top risks drawn from this table) |

**Traceability rule applied:** every control in CONTROLS.md with a status of *Partially Implemented* or *Not Implemented* has exactly one row below, keyed to the same control ID. Controls SEC-08 and AVL-01 are fully implemented and therefore have no gap rows. Remediation owners are roles defined in SYSTEM\_DESCRIPTION.md §7.

**Severity scale:** **High** — directly undermines a service commitment or exposes Restricted data (SSN/banking); likely to drive a qualified opinion or exceptions in a Type II examination. **Moderate** — control deficiency an auditor would expect remediated before the observation period; contained risk. **Low** — hygiene/documentation deficiency, low standalone risk.

---

## Gap Table

| Gap ID | Control ID | Current State | Gap Description | Severity | Remediation Owner | Target |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| GAP-01 | SEC-01 | Partially Implemented | Security policies drafted but never management-approved; no review cadence; no acknowledgment tracking. Auditors test governance first — unapproved policies undercut every control that cites them. | Moderate | CTO | Sep 2026 |
| GAP-02 | SEC-02 | Not Implemented | No formal security awareness training program; no annual refresh; no completion tracking. | Moderate | People Operations Manager | Oct 2026 |
| GAP-03 | SEC-03 | Partially Implemented | Background checks skipped for 3 of 5 engineering contractors; 2 unscreened contractors hold production access to payroll data. | High | People Operations Manager | Aug 2026 |
| GAP-04 | SEC-04 | Not Implemented | No documented risk assessment has ever been performed. Foundational gap: CC3-series criteria cannot be met, and risk-responsive controls (CC9.1, vendor management) have no baseline to respond to. | High | CTO | Oct 2026 |
| GAP-05 | SEC-05 | Partially Implemented | Deprovisioning for legacy admin console and SFTP portal is manual, unchecklisted, unmeasured. Sampled terminations found one admin-console account active 47 days post-termination. | High | Security Engineer | Sep 2026 |
| GAP-06 | SEC-06 | Partially Implemented | Legacy admin console (bulk payroll data access) and customer SFTP portal sit outside Okta SSO; local credentials, no MFA. | High | VP of Engineering | Nov 2026 (migrate console behind SSO); SFTP portal Dec 2026 |
| GAP-07 | SEC-07 | Not Implemented | No periodic access review has ever occurred on production AWS, database, admin console, or Okta groups. Access accretes on role change. | High | Security Engineer | First review Sep 2026, quarterly thereafter |
| GAP-08 | SEC-09 | Partially Implemented | CI/CD scanning only. No remediation SLAs, no finding tracking to closure, no recurring scanning of the running environment. | Moderate | Security Engineer | Oct 2026 |
| GAP-09 | SEC-10 | Partially Implemented | No documented alert triage procedure; no security-specific alert use cases; admin-console activity entirely unmonitored despite being the highest-sensitivity human access path. | Moderate | Security Engineer (use cases), DevOps Lead (pipeline) | Nov 2026 |
| GAP-10 | SEC-11 | Partially Implemented | IR plan untested, names departed individuals instead of roles, and omits customer/regulatory notification triggers — untenable for a processor of SSN and banking data. | High | Security Engineer | Revised plan Sep 2026; tabletop Nov 2026 |
| GAP-11 | SEC-12 | Partially Implemented | Two engineers hold standing direct production/database access; no documented emergency change path; no retroactive review of out-of-band changes. Undermines the change-management control it coexists with. | High | VP of Engineering | Oct 2026 |
| GAP-12 | SEC-13 | Not Implemented | No vendor risk process. Subprocessor SOC 2 reports never collected; Firstline ACH Services contract (recipient of banking data) predates current MSA and its data-protection terms are unevaluated. | High | CTO | Dec 2026 |
| GAP-13 | SEC-14 | Partially Implemented | Tenant isolation in shared-schema database assured only by code review; no automated cross-tenant test suite; no penetration testing of isolation. | High | VP of Engineering | Test suite Oct 2026; pentest Jan 2027 |
| GAP-14 | AVL-02 | Partially Implemented | Backups run but restoration has never been tested; recovery capability is unproven. | High | DevOps Lead | First restore test Aug 2026, annually thereafter |
| GAP-15 | AVL-03 | Not Implemented | No DR plan; RTO/RPO undefined; single-region architecture with no documented regional-failure strategy despite hard payroll deadlines and a 99.9% commitment. | High | DevOps Lead (plan), CTO (RTO/RPO approval) | Plan \+ targets Nov 2026; DR test Feb 2027 |
| GAP-16 | CONF-01 | Not Implemented | No data classification scheme; handling requirements for SSN/banking data are undocumented tribal knowledge, leaving downstream controls (access, logging, retention) without a defined basis. | Moderate | Security Engineer | Sep 2026 |
| GAP-17 | CONF-02 | Partially Implemented | Full bank account numbers found in payroll-service debug logs shipped to Datadog — Restricted data resident in a third-party platform outside its intended boundary. Staging data-masking is convention, not control. | High | VP of Engineering (log remediation), Security Engineer (standards) | Purge \+ scrubbing Aug 2026; standards Sep 2026 |
| GAP-18 | CONF-03 | Not Implemented | No retention schedule; two former customers' complete datasets retained two years past offboarding, contrary to MSA deletion commitments. | High | Security Engineer (schedule), DevOps Lead (deletion tooling) | Schedule Sep 2026; legacy data disposition Oct 2026 |

## Severity Distribution

| Severity | Count | Gap IDs |
| :---- | :---- | :---- |
| High | 13 | GAP-03, 04, 05, 06, 07, 10, 11, 12, 13, 14, 15, 17, 18 |
| Moderate | 5 | GAP-01, 02, 08, 09, 16 |
| Low | 0 | — |

Thirteen of eighteen gaps rate High. That is not inflation; it reflects a payroll platform carrying SSNs and banking data with no risk assessment, no access reviews, untested recovery, and Restricted data leaking into logs. A first-time Series B candidate with a one-person security function commonly looks like this — the point of a readiness assessment is to see it plainly before an auditor does.

## Sequencing Note (feeds EXEC\_SUMMARY.md)

Remediation is sequenced so foundational items land first: GAP-04 (risk assessment) and GAP-16 (classification) define the basis other controls reference; GAP-17 (data in logs) and GAP-03 (unscreened access) are live exposures remediated immediately regardless of audit timing; GAP-06/GAP-05 (legacy system access) are the largest engineering lift and drive the critical path to audit readiness.  
