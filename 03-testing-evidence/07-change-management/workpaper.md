# Change Management: Test Workpaper (CM-01)

## Sample

10 access-related changes (Conditional Access policy changes and privileged role
assignments) identified during the audit period. Four of the ten were deliberately selected
because they trace back to accounts or findings already flagged elsewhere in this audit,
specifically to test whether those findings share a root cause rather than being independent
coincidences. Full sample: [`change-sample.csv`](change-sample.csv).

| Result | Count |
|---|---|
| Sample size | 10 |
| Complete, approved record predating implementation | 6 |
| No approval record found | 2 (CR-01, CR-02) |
| No record at all (predates the change management process) | 1 (CR-09) |
| Record exists and was approved, but implementation did not match what was approved | 1 (CR-10) |

**Exceptions:**

- **CR-01 and CR-02** are the change-management side of the PAM-01, AR-01, and PR-022
  findings already reported on wwright and mjackson2: both accounts received standing Global
  Administrator access bundled into onboarding, with no separate change request or approval
  for the privileged grant itself. This is the root cause those earlier findings were missing:
  the access wasn't approved and then forgotten, it was never submitted for approval as a
  distinct decision in the first place.
- **CR-09** explains the athompson finding (PAM-01, AR-01) the same way, but for a different
  reason: the grant predates the company's adoption of a formal change management process
  entirely, from when the founding engineers held tenant-wide admin informally. There is
  nothing to approve retroactively; the finding here is that no process exists to catch
  legacy grants like this one after the fact, either.
- **CR-10** is the most significant finding in this control area and arguably in the whole
  audit: the approved change was to add bg-admin2 to the Conditional Access exclusion group
  ahead of a scheduled rotation test. The account actually added was mjackson2, a copy-paste
  error in updating group membership. Nobody caught the discrepancy between what was approved
  and what was implemented, and it was never reversed. This single change-management failure
  is the direct, traceable cause of the PAM-02 finding (a standing admin account incorrectly
  excluded from MFA enforcement), it is not a coincidence that the two findings involve the
  same account; one caused the other.

**Result: CM-01 does not pass.** 4 of 10 sampled changes (40%) show a missing, absent, or
inaccurate change record. This is the highest exception rate of any control tested in this
audit, and it is the one that reframes several earlier findings: PAM-01, AR-01, PR-022, and
PAM-02 are not four separate control failures, they are four symptoms of the same underlying
gap, access-related changes are not consistently going through an approval and verification
step before (or instead of) being made.

## Why this control was tested last, and what it changes

The control test plan sequenced change management after the other six areas for a reason:
testing it last, once several individual-account findings already existed, made it possible
to deliberately check whether those findings traced back to a common cause rather than
treating each as independent. They did. The treatment plan should prioritize fixing the
change management gap itself (CR-10 in particular, since it is a live, uncorrected MFA
exposure caused by a documented process failure) over treating each downstream finding as its
own separate remediation item.
