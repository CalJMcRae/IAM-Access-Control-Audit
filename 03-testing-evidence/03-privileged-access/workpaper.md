# Privileged Access Management: Test Workpaper (PAM-01, PAM-02, PAM-03)

## Population

All 10 accounts in [`privileged-accounts.csv`](../../01-scope-and-population/privileged-accounts.csv),
tested 100%. This includes 8 standing privileged accounts and 2 break-glass accounts.

## PAM-01: Standing privileged access has a documented justification and a current review

| Result | Count |
|---|---|
| Standing privileged accounts | 8 |
| Documented justification on file | 6 |
| No justification on file | 2 (wwright, athompson) |
| Review on file within the trailing quarter | 6 |
| No review on file at all | 2 (wwright, athompson, same two accounts) |

**Exceptions:**

- **wwright (IT Manager).** Standing Global Administrator with no justification ticket found;
  the access appears to have been granted at hire with no documented request, and has never
  been reviewed since.
- **athompson (VP Engineering).** Standing Global Administrator, no justification on file, no
  review on file since the account was created in 2023. The note on this account describes it
  as a legacy grant from the company's early-stage period, when the founding engineers held
  tenant-wide admin informally. That makes it the oldest unreviewed privileged grant in the
  tenant, roughly three and a half years without anyone revisiting whether a VP of Engineering
  still needs standing admin rights over the whole identity platform, rather than scoped
  access to the systems Engineering actually owns.

**Result: PAM-01 does not pass.** 2 of 8 standing privileged accounts (25%) have neither a
justification nor a review history. Both exceptions are also the same two accounts with no
review on file, there is no case in this population of a justified account that simply missed
its latest review, the gap is total for both.

## PAM-02: Privileged accounts enforce MFA, and break-glass is the only Conditional Access exclusion

| Result | Count |
|---|---|
| Standing privileged accounts | 8 |
| MFA enrolled | 7 |
| Not MFA enrolled | 1 (mjackson2) |
| Conditional Access exclusions found outside the two documented break-glass accounts | 0 |

**Exception:**

- **mjackson2 (IT Manager).** Standing Global Administrator, justification on file, but not
  enrolled in MFA. The note on this account states it was excluded from the break-glass
  exclusion group in error, meaning a real, named, standing admin account is currently
  sitting in the same Conditional Access carve-out that is supposed to be reserved for the two
  emergency-access accounts only. This is a materially different, and more serious, finding
  than "forgot to enroll": it means the exclusion group itself is not restricted to the two
  accounts it is documented to contain.

**Result: PAM-02 does not pass.** 1 of 8 standing privileged accounts (12.5%) lacks MFA, and
the root cause is a misconfigured exclusion group rather than an individual enrollment gap,
which means this finding is also a Conditional Access configuration issue, not only an
account-level one. This is the same account flagged under provisioning testing (PR-022) for
having Global Administrator bundled into its onboarding checklist without a separate approval,
two independent tests converging on the same account is a stronger signal than either finding
alone.

## PAM-03: Break-glass accounts tested and rotated on the documented quarterly cadence

| Result | Count |
|---|---|
| Break-glass accounts | 2 |
| Tested within the trailing quarter | 1 (bg-admin1) |
| Overdue | 1 (bg-admin2) |

**Exception:**

- **bg-admin2.** Last successful test and rotation logged 2025-11-02. As of the audit date
  that is well over a year overdue against the documented quarterly cadence. An untested
  break-glass account is a risk in both directions: nobody knows whether the credential still
  works in an actual emergency, and because it is excluded from Conditional Access by design,
  an unrotated, long-lived credential sitting outside normal monitoring is itself an exposure.

**Result: PAM-03 does not pass.** 1 of 2 break-glass accounts (50%) is overdue for its
documented test and rotation.

## Summary across the three PAM controls

Every PAM control in this area produced at least one exception, and the exceptions are not
evenly distributed, they cluster on a small number of accounts (mjackson2 appears in two
separate findings across two control areas; athompson and wwright are the tenant's two oldest
unreviewed admin grants). That clustering, not the raw exception count, is the strongest
argument for prioritizing a roster-level review over trying to fix each finding as an isolated
ticket.
