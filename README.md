# IAM & Access Control Audit

An ITGC-style audit of identity and access management for a 210-person company, covering
provisioning, deprovisioning, privileged access, MFA enforcement, access reviews,
joiner/mover/leaver, and change management. Every control was defined and scored before any
evidence was examined, so the findings came from testing, not from writing a conclusion first
and building data to match it.

## Scenario

**Meridian Health Analytics** is the same fictional company used across this portfolio's
[AWS cloud risk assessment](https://github.com/CalJMcRae/GRC-AWS-Project) and
[AI compliance assessment](https://github.com/CalJMcRae/AI-Compliance-Project) projects,
picked up roughly a year later. The company has grown from about 25 staff to 210, running
identity on Microsoft 365 / Entra ID. Growth outpaced process: a larger enterprise prospect's
security questionnaire asked for evidence of access reviews, privileged access management, and
joiner/mover/leaver controls that the company could not produce on request, which is what
triggered this audit. Full detail: [`01-scope-and-population/company-profile.md`](01-scope-and-population/company-profile.md).

## Approach

1. **Build the population first, before deciding what to look for.** 210 active and 25
   terminated employees, a 10-account privileged access roster, and 6 service accounts, built
   with realistic, deliberately seeded gaps the same way the AWS lab project planted
   deliberate misconfigurations before scanning them.
2. **Define the control test plan before touching the evidence.** 14 testable controls across
   the 7 required areas, each with a stated pass/fail threshold and sampling method decided in
   advance: [`02-control-test-plan/`](02-control-test-plan/).
3. **Test every control area and report what was actually found**, full population where the
   data allowed it (deprovisioning, privileged access, MFA), a judgmental sample where it did
   not (provisioning, access reviews, joiner/mover/leaver, change management):
   [`03-testing-evidence/`](03-testing-evidence/).
4. **Consolidate into a findings register**, scored with the same Likelihood x Impact
   convention used across this portfolio: [`04-findings/`](04-findings/).
5. **Build a phased remediation plan**, grouping findings that share a root cause rather than
   treating every exception as its own isolated fix: [`05-remediation-tracker/`](05-remediation-tracker/).
6. **Convert the plan into Jira-style tickets**, last, once the findings and remediation items
   were already settled: [`06-tickets/tickets.csv`](06-tickets/tickets.csv).

## Key results

All 14 controls failed as designed to some degree. Testing produced **17 findings** (5
Critical, 5 High, 5 Medium, 2 Low), consolidated into **13 remediation items** and **24
tickets**.

| Control area | Result |
|---|---|
| Provisioning | 2 of 25 sampled accounts missing approval evidence; 1 account granted access beyond its documented baseline |
| Deprovisioning | 2 of 25 terminated employees retained a fully enabled account over a year past termination; 1 disabled 16 days late |
| Privileged Access Management | 2 of 8 standing admin accounts with no justification or review since being granted; 1 standing admin account excluded from MFA enforcement |
| MFA Enforcement | 94.8% coverage against a 210-person population (11 gaps); 1 service account still permits legacy authentication |
| Access Reviews | Same 2 privileged accounts unreviewed as above; 2 of 25 sampled standard users with no review evidence |
| Joiner/Mover/Leaver | 2 of 10 sampled internal transfers retained access from their prior role, one involving personal data; 1 stale manager-of-record |
| Change Management | 4 of 10 sampled access-related changes had a missing or inaccurate record |

**The headline finding is not any single control failure.** Three of the five Critical
findings trace directly back to one root cause: inconsistent change management for
access-related changes. A single copy-paste error in a Conditional Access group update, never
caught because no verification step existed, is the specific, traceable cause of a standing
admin account losing MFA enforcement. Full detail: [`04-findings/findings-summary.md`](04-findings/findings-summary.md).

Three additional findings (service account privilege issues) were not produced by any of the
14 planned control tests; they turned up while reviewing evidence already gathered for other
controls, and are reported separately as incidental rather than folded in as if the test plan
had predicted them.

## Limitations, stated plainly

- All data is synthetic, generated for this project with deliberately seeded control gaps, not
  pulled from a real tenant.
- Unlike the AWS lab project, nothing here was implemented and verified in a live environment.
  This project's deliverable is the audit methodology and the resulting plan, a control test
  plan fixed in advance, full-population and sampled testing, a findings register, and a
  phased remediation plan, not a before/after fix.
- `06-tickets/tickets.csv` is a structured dataset shaped like a Jira export, not tickets
  entered into a live Jira instance.
- Several findings were deliberately threaded through multiple control areas so the
  change-management testing (run last) could trace root causes. That is a real audit
  technique, testing change management after other findings exist specifically to check
  whether they share a cause, but it also means this project's findings are more
  interconnected than an audit of 210 genuinely independent people and accounts would likely
  produce by chance.

## Skills demonstrated

ITGC-style control design and testing · identity and access management (Microsoft 365 / Entra
ID, Conditional Access) · attribute sampling methodology · privileged access management ·
joiner/mover/leaver control testing · root-cause analysis across control domains · risk-based
remediation planning · audit-to-ticket translation
