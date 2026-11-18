# Joiner/Mover/Leaver: Test Workpaper (JML-01, JML-02)

## Sample

The "joiner" and "leaver" halves of this lifecycle are already covered by the provisioning
and deprovisioning control areas; this test covers the "mover" case specifically, internal
transfers and promotions, which is the scenario where access tends to accumulate rather than
simply get granted or revoked. 10 moves identified from the HR change log during the audit
period, not a sample of the point-in-time roster (movers are identified by the change itself,
not by a static population). Full sample: [`mover-sample.csv`](mover-sample.csv).

## JML-01: Access recalculated to match the new role, not accumulated alongside the old

| Result | Count |
|---|---|
| Sample size | 10 |
| Access matches new role only | 8 |
| Prior-role access retained (access creep) | 2 (ksullivan, swatts) |

**Exceptions:**

- **ksullivan**, transferred from Engineering to IT & Security four months before the audit
  date, still holds the Engineering-AllRepos group alongside the new IT & Security baseline
  access. The new access was granted correctly; the old access simply was never removed.
- **swatts**, transferred from People to IT & Security roughly a year before the audit date,
  still holds access to the People department's HR records system, which includes personal
  data. This is the more serious of the two: a transfer out of HR should be one of the more
  obvious triggers to remove access to personal data, and it is also the oldest unaddressed
  case in the sample (about a year, versus four months for ksullivan), which suggests this is
  not simply a backlog that will clear itself given more time.

**Result: JML-01 does not pass.** 2 of 10 sampled moves (20%) show retained access from the
prior role. Both exceptions share the same pattern: the new access was provisioned correctly,
but nothing in the move process actually removes the old access, provisioning for movers
appears to work, deprovisioning for movers does not.

## JML-02: Manager-of-record updated to reflect the new reporting line

| Result | Count |
|---|---|
| Sample size | 10 |
| Manager-of-record updated | 9 |
| Manager-of-record stale | 1 (bcollins) |

**Exception:**

- **bcollins**, transferred from Finance to Legal & Compliance. Access itself was updated
  correctly (Finance access removed, Legal & Compliance access added), but the manager field
  in Entra ID still shows the former Finance manager three months after the move. This matters
  beyond an org-chart accuracy question: access reviews (AR-02) are run by the manager of
  record, so a stale manager assignment means the wrong person would be asked to review this
  employee's access until the field is corrected.

**Result: JML-02 does not pass**, narrowly: 9 of 10 (90%) correct, one isolated case rather
than a pattern.

## Why the access-creep finding is the one worth escalating

Of the three exceptions found across JML-01 and JML-02, the two access-creep cases are the
ones that compound over time rather than self-correct: a stale manager field gets caught the
next time anyone looks at the org chart, but retained access to a system nobody is actively
monitoring for exactly this employee can persist indefinitely. Both access-creep cases
involved a department change that should have triggered an access removal and did not, which
points at the same root cause identified in deprovisioning: this organization's processes are
better at granting access than at taking it away.
