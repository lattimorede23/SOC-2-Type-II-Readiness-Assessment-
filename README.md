# SOC 2 Type II Readiness Assessment — Lattimore Ledger

A fictional SaaS company GRC readiness assessment | Portfolio and skills demonstration

**Lattimore Ledger is a fictional entity invented for this exercise. It shares the author's name only because the author (Derrick Lattimore) created the fictional scenario; this does not indicate any real commercial relationship. It is not a real operating company, has no customers or employees, and is not affiliated with any real business, past or present.**

---

## About This Project

This repository contains a SOC 2 Type II readiness assessment for a fictional B2B SaaS payroll and HR platform, built to demonstrate GRC and audit-readiness skills — control narrative writing, gap analysis, and executive reporting — for a commercial framework rather than the DoD/federal frameworks (STIG, NIST 800-53, RMF) covered elsewhere in this portfolio.

Everything in this repository — the company, its infrastructure, personnel, and findings — is fictional. It was created as a training and portfolio exercise to practice producing readiness-assessment artifacts the way they would be developed ahead of a real SOC 2 Type II examination, using the AICPA 2017 Trust Services Criteria (with the Fall 2022 Revised Points of Focus).

No content in this repository describes a real company, system, or individual.

## The Fictional Company

**Lattimore Ledger** — a Series B B2B SaaS company (\~120 employees) providing a cloud-hosted payroll and HR platform for mid-market employers, processing Social Security numbers, banking data, and compensation records on customers' behalf.

| Attribute | Detail |
| :---: | :---: |
| Environment | AWS (EKS, RDS PostgreSQL, S3), multi-tenant shared-schema architecture |
| Framework | AICPA 2017 Trust Services Criteria, with Revised Points of Focus (2022) |
| Scope | Security (Common Criteria), Availability, Confidentiality |
| Assessor | Derrick Lattimore |

## Deliverables

| File | Description |
| :---: | :---: |
| SYSTEM\_DESCRIPTION.md | Company overview, system boundaries, infrastructure and architecture, service commitments |
| CONTROLS.md | 20 control narratives across Security, Availability, and Confidentiality, each with TSC mapping and implementation status |
| GAP\_ASSESSMENT.md | 18 gaps traced by ID to their source control, with severity rating, remediation owner, and target date |
| EXEC\_SUMMARY.md | Overall readiness posture, top 5 risks traced to specific gap IDs, and recommended path to audit-readiness |

The four documents are cross-referenced by ID: any control in CONTROLS.md marked less than fully implemented has exactly one matching row in GAP\_ASSESSMENT.md, every system or role named in a control narrative is established in SYSTEM\_DESCRIPTION.md, and every risk in EXEC\_SUMMARY.md traces back to a specific gap — mirroring how a real SOC 2 readiness package ties together across documents.

## Why This Project

Every other project in this portfolio demonstrates technical control implementation against a DoD-flavored technical checklist (STIG, NIST 800-53). This one is deliberately different: SOC 2's Trust Services Criteria are principles rather than a prescriptive control list, requiring judgment calls about what satisfies a given criterion rather than a lookup against a fixed baseline. It demonstrates the ability to write for a commercial audience, reason about an organization rather than a single system, and produce a forward-looking readiness assessment rather than a point-in-time compliance scan.

## Disclaimer

This is a training and portfolio artifact only. The company, infrastructure, personnel, and findings described are entirely fictional and do not represent any actual organization, system, or individual
