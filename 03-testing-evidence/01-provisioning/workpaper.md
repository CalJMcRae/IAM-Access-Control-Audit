# Provisioning: Test Workpaper (PROV-01, PROV-02)

## Sample

25 new hires drawn at random from the 210 active employees hired within the audit period (May
2026 through April 2027), using a seeded random draw so the sample is reproducible rather than
hand-picked. Full sample and underlying ticket data: [`provisioning-sample.csv`](provisioning-sample.csv).

## PROV-01: Documented requester and approver predating account creation

| Result | Count |
|---|---|
| Sample size | 25 |
| Approver documented and predates account creation | 22 |
| Approver missing | 2 (PR-003, PR-016) |
| Account created later than hire date | 1 (PR-009) |

**Exceptions:**

- **PR-003 and PR-016** both went through the self-service HRIS onboarding form rather than
  the manual IT request form, and neither carries an approver name or approval date, only a
  requester. The account was still created in both cases, which means the control that is
  supposed to gate account creation on approval did not actually gate anything for these two
  intake paths.
- **PR-009** shows account creation 13 calendar days after the hire date, outside the
  same-day expectation documented in `identity-environment.md`. The ticket notes a delayed
  start-date change that did not make it into the original HRIS sync batch, this looks like a
  process timing gap rather than a missed approval, but it still means the employee had no
  account for nearly two weeks of their first pay period.

**Result: PROV-01 does not pass as designed.** 2 of 25 sampled accounts (8%) were created
with no approval evidence at all, concentrated in one specific intake path (the self-service
form) rather than spread evenly across all provisioning. That concentration is itself useful:
it points at a gap in one form, not a universal breakdown of the approval step.

## PROV-02: Granted access matches the documented role baseline

| Result | Count |
|---|---|
| Sample size | 25 |
| Access matches documented baseline | 24 |
| Access exceeds baseline without separate justification | 1 (PR-022) |

**Exception:**

- **PR-022 (mjackson2, IT Manager)** was granted standing Global Administrator access as part
  of the IT Manager onboarding checklist, bundled in alongside the baseline IT & Security
  group, with no separate elevated-access request or approval on file for the admin role
  itself. This is the same account flagged in the privileged access management testing
  (PAM-01) for lacking a documented justification, the provisioning evidence shows where that
  gap actually originated: the role's onboarding checklist grants tenant-wide admin by
  default, rather than privileged access being requested and approved as its own step.

**Result: PROV-02 does not pass as designed.** 1 of 25 sampled accounts (4%) received access
beyond the documented baseline, but it is a structural gap (a checklist that bundles
privileged access into a standard role's onboarding) rather than an isolated one-off, and it
connects directly to a separate finding in the privileged access management testing.

## What this sample does and does not tell us

A 25-item sample out of 210 active hires cannot rule out that other intake paths or other
roles have similar gaps that this sample did not happen to catch. What it does establish is
that both exceptions found trace back to a specific, nameable process (one onboarding form,
one role's checklist) rather than random individual error, which is the more useful finding
for a remediation plan: fix the form and the checklist, not 210 individual tickets.
