# Executive Summary — SOC 2 Type II Readiness Assessment

**DISCLAIMER — FICTIONAL TRAINING EXERCISE.** This document is part of a SOC 2 Type II readiness assessment created as a portfolio and training exercise. **Lattimore Ledger is a fictional entity invented for this exercise. It is not a real operating company and is not affiliated with any real business.** This is not a SOC 2 report.

|  |  |
| :---- | :---- |
| **Document** | EXEC\_SUMMARY.md (4 of 4\) |
| **Prepared for** | Lattimore Ledger executive leadership (fictional) |
| **Prepared by** | Derrick Lattimore, Assessor |
| **Scope** | Security, Availability, Confidentiality (AICPA 2017 TSC, Revised Points of Focus — 2022\) |
| **Supporting detail** | 20 control narratives (CONTROLS.md), 18 gaps (GAP\_ASSESSMENT.md) |

## Overall Readiness Posture: **Not Ready — estimated 7–9 months to audit-ready**

Lattimore Ledger's engineering foundation is genuinely strong: encryption (SEC-08) and capacity management (AVL-01) passed cleanly, and the Okta/CI-CD backbone means many fixes are process work, not rebuilds. But of 20 controls assessed, only 2 are fully implemented. 18 carry gaps, 13 rated High. The pattern is consistent with a Series B company that built product first and governance never: the deficiencies concentrate in exactly the areas an auditor tests hardest — risk assessment, access review, incident readiness, and proof (not assumption) of recovery.

**Proceeding directly to a Type II audit today would produce a qualified report or worse, at full audit cost.** The recommendation is: remediate (months 1–5), engage an auditor for a Type I to validate control design (month \~6), then open a Type II observation window of at least 3–6 months. A Type II report attests to controls operating over a period — the observation window cannot start until controls actually operate, which is what drives the 7–9 month estimate.

## Top 5 Risks (traced to GAP\_ASSESSMENT.md by ID)

1. **Restricted data is leaking outside its boundary today — GAP-17 (CONF-02).** Full bank account numbers were found in debug logs shipped to Datadog. This is not an audit-readiness issue; it is a live confidentiality exposure requiring immediate purge and log-scrubbing controls, independent of any audit decision.  
     
2. **No foundation of risk governance — GAP-04 (SEC-04), with GAP-01 (SEC-01).** No risk assessment has ever been performed and policies were never approved. These are the first things an auditor tests, and every other control formally hangs off them. Cheap to fix, blocking if unfixed.  
     
3. **Unreviewed, over-persistent access to payroll data — GAP-07 (SEC-07), GAP-05 (SEC-05), GAP-06 (SEC-06).** No access review has ever run, terminated-user access survived 47 days on the admin console, and the two legacy systems holding the most sensitive human access paths have no SSO and no MFA. Together these form the single most likely source of a reportable incident.  
     
4. **Recovery is unproven against hard payroll deadlines — GAP-15 (AVL-03) and GAP-14 (AVL-02).** Single-region architecture, no DR plan, no defined RTO/RPO, and backups that have never been restore-tested — against a 99.9% commitment and customers' legal obligation to pay employees on schedule. An availability failure during a payroll window is the company's worst commercial scenario.  
     
5. **Third parties holding banking data are unvetted — GAP-12 (SEC-13).** No vendor risk process exists; the ACH subprocessor that receives account and routing numbers operates under a stale contract with unevaluated data-protection terms, and no subprocessor SOC 2 report has ever been reviewed.

## Recommended Decision

Approve the remediation program in GAP\_ASSESSMENT.md (owners and dates assigned there), fund the two engineering-heavy items now (legacy system SSO migration — GAP-06; DR program — GAP-15) since they drive the critical path, and defer auditor engagement for Type I until the September–October 2026 remediation wave lands. Revisit adding Processing Integrity in the second audit cycle (see SYSTEM\_DESCRIPTION.md §6 — its exclusion this cycle is a judgment call leadership should ratify explicitly).  
