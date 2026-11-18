# MFA Enforcement: Test Workpaper (MFA-01, MFA-02)

## MFA-01: Active users enrolled in MFA per Conditional Access policy CA01

## Population

All 210 active employees, tested 100%. Source:
[`user-population.csv`](../../01-scope-and-population/user-population.csv).

| Result | Count | Rate |
|---|---|---|
| Active population | 210 | |
| MFA enrolled | 199 | 94.8% |
| Not MFA enrolled | 11 | 5.2% |

**Exceptions (11 of 210):**

| Username | Department | Job title |
|---|---|---|
| eharris | Engineering | Engineering Manager |
| lmiller | Engineering | Senior Software Engineer |
| janderson | Engineering | Software Engineer |
| sjones | Engineering | Software Engineer |
| erobinson | Product | Product Manager |
| csanchez | Data & Analytics | Data Analyst |
| alewis | Clinical Data Operations | Clinical Data Ops Manager |
| sking | Clinical Data Operations | Senior Clinical Data Analyst |
| dhall | Finance | Senior Financial Analyst |
| mtaylor2 | Legal & Compliance | Compliance Analyst |
| ojackson | Facilities & Admin | Office Manager |

**Result: MFA-01 does not pass.** The test plan set a 98% coverage threshold specifically to
allow for a normal onboarding grace period; actual coverage is 94.8%, below that threshold,
and none of the 11 exceptions found carries any documented grace-period or exception
justification, these are gaps, not pending enrollments. Two of the eleven (eharris, an
Engineering Manager, and alewis, the Clinical Data Operations Manager) are management-tier
accounts rather than individual contributors, which is a heavier finding than the raw
percentage suggests: a manager's account typically carries access to their team's work and
often approval authority, so the exposure per unenrolled account is not uniform across the
list.

This is also the one control area where the exception count (11) is large enough, and spread
across enough departments (6 of 13), to point at a gap in the enrollment process itself rather
than 11 unrelated individual oversights. Policy CA01 is documented as applying to all users
with no stated exceptions, so there is no policy basis for any of these 11 accounts to be
unenrolled.

---

## MFA-02: Legacy authentication blocked tenant-wide per Conditional Access policy CA02

## Population

All 6 accounts in [`service-accounts.csv`](../../01-scope-and-population/service-accounts.csv),
cross-checked against the user and privileged populations for any other indication of legacy
auth use, tested 100%.

| Result | Count |
|---|---|
| Accounts tested | 6 |
| Legacy authentication blocked | 5 |
| Legacy authentication still permitted | 1 (svc-backup) |

**Exception:**

- **svc-backup.** Documented in `service-accounts.csv` as still permitting legacy (basic)
  authentication, consistent with the gap already flagged in `identity-environment.md` when
  CA02 was documented. This account also has no MFA-equivalent control and has not had its
  password rotated in over a year, so legacy auth is not an isolated gap on this account, it
  is one symptom of the account generally sitting outside the controls the rest of the tenant
  was migrated to.

**Result: MFA-02 does not pass.** 1 of 6 service accounts (17%) permits legacy authentication
against a policy documented as applying tenant-wide with no stated exceptions. No active user
or privileged account tested showed evidence of legacy authentication use.
