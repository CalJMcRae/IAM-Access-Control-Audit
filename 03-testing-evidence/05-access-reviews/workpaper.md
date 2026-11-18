# Access Reviews: Test Workpaper (AR-01, AR-02)

## AR-01: Privileged role assignments reviewed quarterly

## Population

All 10 accounts in [`privileged-accounts.csv`](../../01-scope-and-population/privileged-accounts.csv),
tested 100% against the `last_access_review_date` field, the same underlying data examined
under PAM-01.

| Result | Count |
|---|---|
| Accounts | 10 |
| Review on file within the trailing quarter | 8 |
| No review on file | 2 (wwright, athompson) |

**Result: AR-01 does not pass.** The two exceptions are the same two accounts already flagged
under PAM-01 for lacking a documented justification, this is the same underlying evidence
examined for a different question (was it reviewed, rather than was it justified), and it
fails for the same reason: nobody has looked at either account since it was granted. AR-01 is
reported separately from PAM-01 per the audit scope, but the treatment plan should treat them
as one remediation, not two.

---

## AR-02: Standard user access reviewed at least annually

## Sample

25 active users drawn at random, independent of the provisioning sample, spanning 11 of the
13 departments. Full sample: [`annual-review-sample.csv`](annual-review-sample.csv).

| Result | Count |
|---|---|
| Sample size | 25 |
| Review on file within the trailing 12 months | 23 |
| No review evidence found | 2 (csanchez, ojackson) |

**Exceptions:**

- **csanchez (Data Analyst, Data & Analytics).** No review evidence found for the audit
  period. This is the same account flagged under MFA-01 for not being enrolled in MFA, a
  review, had one occurred, would not necessarily have caught a missing MFA enrollment anyway
  (MFA status is outside the documented scope of this review, see the note on MHA-1063 in the
  sample), but the account had neither control working in its favor.
- **ojackson (Office Manager, Facilities & Admin).** No review evidence found. Notably, this
  is a department head role; a department of one head and four reports has no obvious
  secondary approver, which may partly explain why the review did not happen, there is no
  natural "manager reviews the manager" step in a one-person-deep department.

**Positive finding worth keeping:** two reviews in this sample (sjackson, dmartin) show the
review process actually catching and removing stale access, a leftover SharePoint permission
and a cross-team shared drive permission respectively. That is evidence the control works when
it runs, the gap is coverage (did the review happen at all), not the review's effectiveness
when it does happen.

**Result: AR-02 does not pass as designed**, but narrowly: 23 of 25 (92%) show review
evidence within the required window. The two gaps do not share an obvious common cause the way
the provisioning exceptions did (one department-head structural issue, one with no clear
explanation), so this reads as isolated coverage gaps rather than a single process defect.
