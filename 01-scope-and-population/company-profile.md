# Company Profile: Meridian Health Analytics

## Who they are

Meridian Health Analytics is the same fictional company used across this portfolio's
[AWS cloud risk assessment](https://github.com/CalJMcRae/GRC-AWS-Project) and
[AI compliance assessment](https://github.com/CalJMcRae/AI-Compliance-Project) projects. This
project picks the story up roughly a year later: the AWS lab was framed around a ~25-person
company shortly after a Series B raise. Meridian has since scaled to 210 active employees
across engineering, clinical data operations, customer success, and the usual commercial and
back-office functions.

That growth is the reason this audit exists. A 25-person company can run identity and access
management informally, the founders know who has access to what. A 210-person company with a
clinical data operations team and enterprise healthcare customers cannot. Access decisions that
used to happen in a Slack message now need a defined process, an audit trail, and periodic
review, and nobody has gone back to check whether the process actually caught up with the
headcount.

## Why this audit, why now

Two concrete triggers:

- **A larger enterprise prospect's security questionnaire asked for evidence of access
  reviews, joiner/mover/leaver controls, and privileged access management**, none of which
  Meridian could produce on request. That gap is what prompted leadership to commission an
  internal audit before the next round of vendor due diligence.
- **Headcount growth has outpaced IT operational maturity.** The IT & Security function grew
  from an informal responsibility held by a couple of engineers to an 11-person team, but the
  provisioning and deprovisioning processes were never formally redesigned for the new scale,
  they were just run faster.

## Scope of this audit

This is an ITGC-style audit of identity and access management across Meridian's Microsoft 365
/ Entra ID environment, covering:

1. User provisioning (new hires)
2. User deprovisioning (terminations)
3. Privileged access management
4. Multi-factor authentication enforcement
5. Periodic access reviews
6. Joiner/mover/leaver (role changes, transfers, internal promotions)
7. Change management for access-related configuration changes

**Out of scope:** physical access controls, network segmentation, and the AWS environment
covered by the separate cloud risk assessment project. This audit is scoped to identity and
access alone.

**Audit period:** the trailing 12 months, May 2026 through April 2027. Population samples
(the terminated-employee list used for deprovisioning testing, in particular) are drawn from
this window.

## Headcount snapshot (as of the audit date)

210 active employees across 13 departments:

| Department | Active headcount |
|---|---|
| Engineering | 61 |
| Clinical Data Operations | 24 |
| Data & Analytics | 22 |
| Customer Success | 21 |
| Sales | 16 |
| Product | 14 |
| IT & Security | 11 |
| Finance | 9 |
| Marketing | 9 |
| People | 7 |
| Executive | 6 |
| Legal & Compliance | 5 |
| Facilities & Admin | 5 |

Plus 25 employees terminated within the trailing 12-month audit period, sampled for the
deprovisioning control test.

## A note on the data

The user population, privileged account roster, and service account inventory in this folder
are synthetic, generated for this project rather than pulled from a real tenant. They are
deliberately seeded with a set of realistic control gaps (orphaned terminated accounts still
enabled, MFA coverage short of 100%, standing privileged access without documented
justification, a break-glass account with an overdue test) so that the control testing in
this project has something genuine to find, the same approach used for the deliberate AWS
misconfigurations in the cloud risk assessment project. The findings that follow are reached
by testing the population data honestly, not written first and reverse-engineered into the
data.
