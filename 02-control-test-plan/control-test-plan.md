# Control Test Plan

This document sets out what each control test is checking and what counts as a pass, before
any testing evidence is examined. The point of fixing this in advance is the same reason the
AWS lab project planted misconfigurations before scanning them: a finding only means something
if the test that found it was defined first, not written to match whatever the data happened
to show.

The full test matrix is in [`control-matrix.csv`](control-matrix.csv). This document explains
the methodology behind it.

## The 7 control areas, as 14 testable controls

The brief specified seven control areas. Each is broken into two controls (three for
Privileged Access Management, since it carries the most risk), because "test provisioning" is
not a testable statement on its own, "new hires have a documented approver" and "new hires get
the right baseline access" are two different things that can each independently pass or fail.

| Area | Controls |
|---|---|
| Provisioning | PROV-01 (approval evidence), PROV-02 (baseline access) |
| Deprovisioning | DEPROV-01 (timeliness), DEPROV-02 (no orphaned accounts) |
| Privileged Access Management | PAM-01 (justification and review), PAM-02 (MFA enforcement), PAM-03 (break-glass testing) |
| MFA Enforcement | MFA-01 (active user coverage), MFA-02 (legacy auth blocked) |
| Access Reviews | AR-01 (privileged, quarterly), AR-02 (standard users, annual) |
| Joiner/Mover/Leaver | JML-01 (access recalculated on role change), JML-02 (manager-of-record accuracy) |
| Change Management | CM-01 (access-related changes follow a documented process) |

## Full population versus sample, and why

Three controls (DEPROV-01, DEPROV-02, PAM-01/02/03, MFA-01, MFA-02) are tested against the
**full population** rather than a sample. These are the controls where the population itself
is small enough to test completely (25 terminations, 10 privileged accounts, 210 active
users, 6 service accounts) and where the underlying data already exists in
`01-scope-and-population/`. There is no reason to sample when the whole population is already
in hand and the test is a straightforward field comparison.

The remaining controls (PROV-01/02, AR-02, JML-01/02, CM-01) require evidence that does not
exist as a population-wide dataset, a specific provisioning ticket, a specific access review
sign-off, a specific change record, so they are tested against a **judgmental sample**. The
sample size of 25 for the provisioning and access-review tests follows the conventional ITGC
sampling convention for populations under 250 (commonly used in SOX and similar internal
control testing); the smaller samples of 10 for joiner/mover/leaver and change management
reflect that those populations (internal transfers, access-related config changes during the
period) are inherently smaller than the full 210-person roster.

## What "pass" means, and why it is not zero tolerance everywhere

Most controls are pass/fail with no tolerance, DEPROV-02 (orphaned accounts) and MFA-02
(legacy authentication) are tested as "zero exceptions means pass" because both represent a
direct, standing exposure with no acceptable baseline rate.

MFA-01 is the one control written with an explicit tolerance (98% coverage) rather than 100%.
That is a deliberate choice, not a lowered bar: a handful of exceptions during a legitimate
onboarding grace period or a documented hardware exception is normal in a real MFA rollout,
and holding every control to zero tolerance regardless of context would make the test plan
less credible, not more rigorous. Any exception found during testing is still named
individually in the findings log; the 98% threshold only affects whether the control as a
whole is rated a pass or a gap, not whether individual exceptions get reported.

## What this test plan does not cover

This audit is scoped to identity and access management in Microsoft 365 / Entra ID, as stated
in `company-profile.md`. It does not test network segmentation, physical access, or the AWS
environment covered by the separate cloud risk assessment project. Within identity and access,
it also does not test password complexity or account lockout policy, those are authentication
hygiene controls distinct from the access-control questions (who has access, was it approved,
was it removed on time) this audit is built around.
