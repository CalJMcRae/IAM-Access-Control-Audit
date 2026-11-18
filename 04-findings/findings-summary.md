# Findings Summary

17 findings came out of testing the 14 controls in `02-control-test-plan/control-matrix.csv`.
Every one of the 7 control areas produced at least one finding; none of the 14 controls
passed as designed. Full detail, scoring, and ownership: [`findings-log.csv`](findings-log.csv).

## By severity

| Band | Count |
|---|---|
| Critical | 5 |
| High | 5 |
| Medium | 5 |
| Low | 2 |
| **Total** | **17** |

## Three of the findings are incidental, not from the control matrix

F-11, F-12, and F-13 (the three service-account issues: an over-privileged sync account, an
over-scoped CI/CD service principal, and an orphaned file-share migration account) were not
produced by any of the 14 planned control tests. They turned up while reviewing
`service-accounts.csv` for MFA-02 and the privileged access controls, because looking at that
data for one question (does this account allow legacy auth, is it in the privileged roster)
meant also looking at the role it holds. They are reported here rather than left out, because
a finding that was genuinely visible in evidence already gathered should not be dropped just
because it falls outside the original test script, but they are also labelled clearly as
incidental so the distinction between "the test plan caught this" and "this was noticed along
the way" stays honest.

## The root-cause chain is the headline finding, not any single control failure

Five findings (F-03, F-04, F-05, F-07, F-17) are rated Critical, and three of those five
(F-03, F-04, F-05) are not independent, they are traced directly to F-17: a 40% exception rate
in sampled change management records for access-related changes. The chain runs:

- **F-17** (change management is inconsistently followed) explains **why** F-03 (a checklist
  bundles Global Administrator into onboarding with no separate approval) and F-04 (a legacy
  admin grant has never been reviewed) were able to happen and go unnoticed for years.
- **F-17** also explains **exactly how** F-05 happened: a single copy-paste error in a
  Conditional Access group update, never caught, left a standing admin account with no MFA
  enforcement.

That means the single highest-priority remediation in this audit is not any one of the four
findings individually, it is fixing the change management gap itself. Fixing F-03, F-04, and
F-05 one at a time without fixing F-17 would leave the same failure mode free to produce a
fifth version of this finding next quarter.

**F-07 and F-15** are the two Critical findings that are not part of that chain. Both are a
different root cause (JML and deprovisioning do not reliably remove access when someone
leaves or changes role), and both get their own treatment in the remediation tracker rather
than being folded into the change-management fix, correctly identifying that two different
systemic gaps exist (governing *changes to* access, and *removing* access that is no longer
needed) is itself part of the audit's value; collapsing them into one story would overstate
how connected the findings actually are.

## What this audit is not claiming

A 25-item or 10-item sample finding a given exception rate is not a precise population-wide
estimate, the deprovisioning and MFA findings (F-07, F-09) are stronger in that specific
respect because those two controls were tested against the full population rather than a
sample. The sampled findings (provisioning, access reviews, JML, change management) establish
that each gap exists and is not vanishingly rare, not an exact rate across all 210 active
employees or every historical change.
