# SOC 2 Type II Readiness Assessment: Lattimore Ledger

A readiness assessment for a fictional B2B SaaS payroll and HR platform, prepared as it would be before a SOC 2 Type II examination. It is scoped to the Security, Availability, and Confidentiality Trust Services Criteria (AICPA 2017 TSC, with the 2022 revised points of focus).

> **Fictional entity.** Lattimore Ledger was invented for this exercise. It shares the author's surname only because the author created the scenario. It is not a real company, has no customers or employees, and has no affiliation with any real business. All infrastructure, personnel, and findings are fictional.

---

## The fictional company

| Attribute | Detail |
| :-- | :-- |
| Business | Series B B2B SaaS (about 120 employees): cloud payroll and HR for mid-market employers |
| Data processed | Social Security numbers, banking data, and compensation records, on customers' behalf |
| Environment | AWS (EKS, RDS PostgreSQL, S3), multi-tenant shared-schema architecture |
| Scope | Security (Common Criteria), Availability, Confidentiality |
| Framework | AICPA 2017 Trust Services Criteria, revised points of focus (2022) |

---

## Deliverables

Read the documents in this order:

| # | Document | Contents |
| :-- | :-- | :-- |
| 1 | [`SYSTEM_DESCRIPTION.md`](SYSTEM_DESCRIPTION.md) | Company overview, system boundary, infrastructure and architecture, service commitments |
| 2 | [`CONTROLS.md`](CONTROLS.md) | 20 control narratives (SEC-01 to SEC-14, AVL-01 to AVL-03, CONF-01 to CONF-03), each with TSC mapping and implementation status |
| 3 | [`GAP_ASSESSMENT.md`](GAP_ASSESSMENT.md) | 18 gaps (13 High, 5 Moderate), each traced to its source control, with severity, remediation owner, and target date |
| 4 | [`EXEC_SUMMARY.md`](EXEC_SUMMARY.md) | Overall readiness posture, top 5 risks traced to specific gap IDs, recommended path to audit readiness |

---

## How the documents tie together

| Link | Rule | Check |
| :-- | :-- | :-- |
| Controls to gaps | Every control rated *Partially Implemented* or *Not Implemented* has exactly one gap row keyed to its control ID | 18 of 20 controls have a gap, with one row each. The other 2 (SEC-08 encryption, AVL-01 capacity management) are fully implemented. Each gap's "Current State" matches its control's status. |
| Gaps to executive summary | Every risk in the executive summary cites the gap IDs behind it | All 9 cited gap IDs exist in the gap table |
| Controls to system description | Every system, vendor, and role named in a control is established in the system description | |

The first two checks were verified programmatically on 2026-10-14.

---

## Why this project

The rest of this portfolio works against prescriptive, DoD-oriented checklists:

- [STIG hardening](https://github.com/lattimorede23/windows11-stig-hardening-lab)
- [NIST RMF](https://github.com/lattimorede23/nist-rmf-authorization-package)
- [NIST 800-53 mapping for Entra ID](https://github.com/lattimorede23/entra-id-zero-trust-iam-lab)

SOC 2 is different. The Trust Services Criteria are principles, not a fixed control list, so each criterion needs a judgment call about what satisfies it. This project shows three skills that checklists don't exercise:

- Writing for a commercial audience.
- Reasoning about a whole organization rather than one system.
- Producing a forward-looking readiness assessment instead of a point-in-time compliance scan.

---

## Revision history

**2026-10-14.** Renamed `SYSTEM_DESCRIPTION1.md` to `SYSTEM_DESCRIPTION.md`. The `1` was an upload artifact, and every document already referred to the file by its intended name. The README was restructured with linked deliverables and the traceability checks above. Assessment content is unchanged.

**Author:** Derrick Lattimore Jr. · B.S. Information Security, Shawnee State University
