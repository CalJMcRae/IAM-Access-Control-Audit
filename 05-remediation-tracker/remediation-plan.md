# Remediation Plan

17 findings were consolidated into 13 remediation items. Full detail, effort, ownership, and
residual risk per item: [`remediation-tracker.csv`](remediation-tracker.csv). This document
explains the grouping and phasing logic.

## Coverage check

Every one of the 17 findings in `04-findings/findings-log.csv` maps to at least one
remediation item. The mapping is not one-to-one: some items address a single finding, some
address several at once where the underlying fix is genuinely the same action.

| Remediation | Findings addressed |
|---|---|
| REM-01 | F-05 |
| REM-02 | F-17, F-03, F-04, F-05 |
| REM-03 | F-03 |
| REM-04 | F-03, F-04 |
| REM-05 | F-07 |
| REM-06 | F-08 |
| REM-07 | F-09 |
| REM-08 | F-10 |
| REM-09 | F-11, F-12, F-13 |
| REM-10 | F-14 |
| REM-11 | F-15, F-16 |
| REM-12 | F-01, F-02 |
| REM-13 | F-06 |

## Why F-05 gets two separate remediation items, not one

REM-01 and REM-02 both touch F-05 (the MFA-bypassed admin account), but they are kept
separate deliberately. REM-01 is a same-day emergency fix: remove one account from one
exclusion group. REM-02 is a 0-30 day process fix: build the change-management workflow that
should have caught that exact kind of error. Folding them into a single line item would risk
the emergency fix waiting on the process design work before anyone acts on it. An active,
uncorrected MFA bypass on a standing Global Administrator account does not wait for a process
redesign.

## Why the phasing puts process fixes at 30-90 days, not immediately

Every remediation item that corrects a known instance (REM-01, REM-03's bundled grant, the
confirmed cases in REM-05, REM-07, REM-11, REM-12) is scheduled 0-30 days. Every item that
redesigns the underlying process (the quarterly attestation in REM-04, the daily detective
check in REM-05, the SLA and escalation in REM-06, the access-recalculation step in REM-11)
is scheduled 30-90 days. This is the same principle used in the AWS lab project's remediation
roadmap: fix what is actively wrong now, then fix the process that let it happen, rather than
treating both as equally urgent and risking neither one getting done well.

The one deliberate exception is REM-02 (the change-management workflow itself), scheduled
0-30 days rather than 30-90. It is a process fix, but it is also the prerequisite for REM-04's
quarterly attestation having anywhere to log its findings, and it directly prevents a repeat
of F-05. Given it produced three of the five Critical findings in this audit, treating it as
a 30-90 day item alongside lower-stakes process work understated how urgent it actually is.

## What "Open" status means here

Every item in the tracker is currently Open. Unlike the AWS lab project, where two
remediations were actually implemented and verified in a live environment, this audit
produces a plan, not a completed fix, the IAM environment being audited here is a population
dataset built to test against, not a live tenant this project has the ability to change. That
is a genuine limitation of this project relative to the AWS one, stated plainly rather than
implied: the audit methodology (control test plan, sampling, evidence, findings, remediation
plan) is the deliverable, not a before/after screenshot of a fixed Conditional Access policy.

## Residual risk, in aggregate

Even with every item in this tracker completed, two categories of residual risk remain,
stated explicitly rather than assumed away:

- **Detective controls catch problems after they occur, not before.** The daily orphaned-
  account check (REM-05), the quarterly privileged-access attestation (REM-04), and the
  access-recalculation step (REM-11) all reduce how long a gap can persist undetected; none
  of them prevent the underlying human or process error that creates the gap in the first
  place. That is an accepted trade-off given the size of this organization (210 employees),
  not a gap in the plan, a fully preventive control for every one of these scenarios would be
  disproportionate to the risk at this scale.
- **Sampled findings do not bound the full population.** REM-12, REM-10, and REM-11's process
  fix are based on samples of 25, 25, and 10 respectively. Fixing the instances found does not
  guarantee no other instances exist outside those samples; the process fixes are the actual
  mitigation for that uncertainty, not the one-time cleanups.
