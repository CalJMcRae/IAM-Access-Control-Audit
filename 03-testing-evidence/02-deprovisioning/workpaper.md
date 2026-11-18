# Deprovisioning: Test Workpaper (DEPROV-01, DEPROV-02)

## Population

All 25 employees terminated within the audit period (May 2026 through April 2027), tested
100%. Source: [`user-population.csv`](../../01-scope-and-population/user-population.csv),
filtered to `employment_status = Terminated`.

## DEPROV-01: Account disabled within 3 business days of termination

| Result | Count |
|---|---|
| Population | 25 |
| Disabled within 3 business days | 22 |
| Disabled, but late | 1 (MHA-1217, nlee) |
| Never disabled | 2 (reported under DEPROV-02 to avoid double-counting; see below) |

**Exception: MHA-1217 (nlee, IT Support Specialist, IT & Security).** Terminated 2026-10-22,
account not disabled until 2026-11-07, a 16-calendar-day gap. There is no ticket or note
explaining the delay. This is the one case in the sample where the process itself ran (the
account was eventually disabled), it just ran far outside the documented timeframe.

**Result: DEPROV-01 does not pass as designed.** 22 of 25 (88%) were disabled within SLA. One
exception is a timeliness failure on its own; the two accounts covered under DEPROV-02 below
are timeliness failures in the most literal sense too (the gap is unbounded, not just long),
but they are reported as their own control because "never happened" is a materially different
finding from "happened late."

## DEPROV-02: No orphaned (still-enabled) terminated accounts

| Result | Count |
|---|---|
| Population | 25 |
| Properly disabled (on time or late) | 23 |
| Still enabled as of the audit date | 2 |

**Exceptions:**

- **MHA-1213 (wnelson, Marketing Specialist).** Terminated 2026-09-17. Account remains
  Enabled as of the audit date, over a year after termination.
- **MHA-1221 (smiller, HR Generalist, People).** Terminated 2026-10-12. Account remains
  Enabled as of the audit date.

Both accounts retain their original license and group membership; neither shows any sign of
having been touched since termination. There is no deprovisioning ticket for either employee
in the records reviewed, which suggests the termination notification from People (HR) to IT &
Security either was never sent or was sent and never actioned, the evidence does not
distinguish between those two causes, and the treatment plan should address both possibilities
rather than assume one.

**Result: DEPROV-02 does not pass.** The control calls for zero exceptions; 2 of 25 terminated
employees (8%) retain a fully enabled account more than a year past their termination date.
This is the most severe finding in this control area: an orphaned enabled account with no
monitoring attached to it is indistinguishable, from the outside, from a legitimate active
account, and it existed undetected for over a year because nothing in the current process
checks terminated employees against account status after the initial (in this case, absent)
disablement step.

## Why this matters more than the 8% figure suggests

Both DEPROV-01 and DEPROV-02 failed on a small number of records, but a termination that falls
through is a standing, unattended credential sitting in the tenant for as long as nobody
happens to look, not a one-time event with a bounded blast radius like a late provisioning
ticket. The population tested here is complete (all 25 terminations, not a sample), so this
8% figure is not a sampling estimate, it is the actual rate for the audit period.
