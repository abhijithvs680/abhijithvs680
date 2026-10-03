# JOURNAL.md — Work Log

Append-only. Newest first. Add an entry only **after** work is verified.
Never edit or remove a past entry.

Entries record public repositories only. Do not record names or metadata of
private or employer repositories here — this repository is public.

---

## 2026-10-03 — Milestone 9 of `system1-audit`: an operating point a cutoff can deliver

**Type:** research project (active) · **Repository:** `abhijithvs680/system1-audit`
· **Branch:** `claude/confident-wright-383ue1` · **Commit:** `7c2d0eb`

Preflight passed before any work: origin matched the intended repository, the
tree was clean, and a `git push --dry-run` to a throwaway ref succeeded without
a proxy error and without creating the ref. A second dry-run for the real branch
ran immediately before the push. The branch was based on the active project head
`d6bb65f`, not on the default branch, so milestones 3, 6, 7 and 8 are carried
forward. The branch did not exist on the remote, so the push created it; the
milestone 1 commit is an ancestor of it, so nothing published was rewritten and
no force-push was used.

**Why this milestone**

Milestones 2 and 4 are still blocked, re-tested today: arxiv.org refused by this
environment's egress policy (`CONNECT tunnel failed, response 403`), `torch` and
`transformers` both absent, no model credentials present. The recommended next
step on the roadmap was milestone 5, the write-up of "coverage at a fixed error
budget" — but the quantity that write-up would report was not the one the library
returned, so the write-up would have documented a defect. Milestone 8 had found
that a risk-coverage curve point can stop inside a group of equally confident
items, which no cutoff can reproduce, and had fixed it only in `voting.py`. The
selective-prediction module itself still answered with the curve prefix, and that
is what the README quickstart, the demo and `selective_coverage_interval` called.

**Two defects, both reproduced before being fixed**

- **A reported threshold could breach the budget it was given.** `coverage_at_risk`
  and `threshold_at_risk` were independent maximisations over the same curve, so
  the pair described an operating point that does not exist. On confidences
  `[0.9, 0.9, 0.9, 0.5]` with the third item wrong, at a **zero** error budget,
  `threshold_at_risk` returned `0.9`; that cutoff answers three items and carries
  a realised error rate of **0.333**, while the largest coverage any cutoff
  achieves at that budget is 0.0. The number was reported as within budget while
  breaching it, which for a guardrail layer is the wrong direction to be wrong in.
- **The interval bounded an unachievable quantity, and distinct confidences did
  not excuse it.** This was the non-obvious half. The tie correction looks
  irrelevant when every observed confidence differs, which is the normal case for
  a probability output. It is not, for any resampled quantity: the bootstrap draws
  with replacement, so ties appear in nearly every resample, and on each one the
  prefix statistic can return coverage no cutoff could deliver.
  `selective_coverage_interval` now resamples the feasible quantity.

**What changed**

`operating_point` returns a threshold together with the coverage and error rate
it realises, consistent by construction and selected on realised risk;
`feasible_coverage_at_risk` is the coverage alone; `coverage_at_risk` is kept and
documented as a bound, which is the right answer to "can any cutoff beat this"
and the wrong answer to "where do I set the threshold". The reachable points turn
out to be identifiable from the curve alone — it is sorted by descending
confidence, so the last point of each tie group is exactly the state of a cutoff
set there — which also makes the free function O(n log n) rather than milestone
8's O(n²) threshold scan, and moves it beside the curve it derives from,
re-exported from `voting.py` so existing callers keep working.

**Measured, not asserted**

60 items over 4 distinct confidence values, as a coarse score or a vote share
produces, at a 10 percent error budget: the curve bound is 0.5500 and the best
any cutoff achieves is 0.4167. The interval's point estimate was the first number
and is now the second. The demo's own profile has distinct confidences and is
unchanged, which is the expected result and the reason a synthetic tied fixture
carries the test.

**Limitations, stated in the repository too**

Both findings are properties of the harness, established on constructed inputs
with hand-computed answers and on property checks over randomised tie patterns.
Neither is a measurement of any model. The tied case is reached by real systems —
a vote share over K passes takes at most K+1 values — but how often it binds on a
System-1 checkpoint is unmeasured, because no checkpoint has been audited. One
bias in `selective_coverage_interval` is fixed and one is not: the in-sample
maximisation remains, and the README says so.

**Verification**

250 tests pass, up from 237, run from a clean detached worktree of the committed
tree under the command the README documents
(`PYTHONPATH=src python3 -m unittest discover -s tests`); `examples/demo.py`
exits 0. The repository configures no linter, but `ruff` and `mypy` are available
in this environment and were run against both the base commit and the result:
identical findings on each (5 ruff, 3 mypy, all pre-existing and outside this
change), so nothing new was introduced and nothing pre-existing was swept up. The
diff was scanned for credentials, private or employer material and generated
junk: none present.

**Not done**

No pull request, no merge, no tag, no release, no settings, visibility,
default-branch or access change. No novelty is claimed anywhere:
arXiv:2609.30454 remains unread. Nothing outside `system1-audit` and this hub
branch was modified, and no external or third-party repository was contacted.

---

## 2026-10-02 — Milestone 8 of `system1-audit`: voting priced per forward pass

**Type:** research project (active) · **Repository:** `abhijithvs680/system1-audit`
· **Branch:** `claude/confident-wright-azzjhj` · **Commit:** `d6bb65f`

Preflight passed before any work: origin matched, tree clean, and a
`git push --dry-run` to a throwaway ref succeeded without a proxy error and
without creating the ref. A second dry-run for the real branch ran immediately
before the push. The branch was based on the active project head `d6fe495`, not
on the default branch, so milestones 3, 6 and 7 are carried forward rather than
dropped. `claude/confident-wright-azzjhj` did not exist on the remote, so the
push created it and no published history was touched.

**Why this milestone**

Milestones 2 and 4 are still blocked, re-tested today: arxiv.org was refused by
this environment's egress policy (`CONNECT tunnel failed, response 403`), no
model runtime is installed (`torch` absent), and no model credentials are
available. The runnable item was the half of H3 that needs no model. The
permutation audit already reported vote accuracy beside single-pass accuracy,
which compares the quantity H3 explicitly rejects — accuracy rather than
coverage at a fixed error budget — and charged the vote nothing for the eight
forward passes it used. Added `src/system1_audit/voting.py` (399 lines) and
`tests/test_voting.py` (314 lines).

**Three results, all measured**

- **A vote share is not a usable abstention gate, and this is a negative
  result.** Against a planted position bias, over 150 items at a 10% error
  budget, majority voting held accuracy at exactly 0.2533 and took coverage
  from 0.2800 to 0.0000 — paired gain `-0.2800 [-0.3600, -0.2000]`, excluding
  zero on the downside — for eight forward passes per item. The vote share
  collapsed to a single distinct value across all 150 items, so no threshold
  could select a lower-error subset. Accuracy is blind to this because accuracy
  never consults the confidence. Averaging the probability vectors instead
  keeps 150 distinct values and keeps the gate.
- **The risk-coverage curve reports coverage no real threshold can deliver.**
  The curve walks one item at a time, so its answer can stop inside a group of
  equally confident items; a deployed cutoff answers every item at or above it.
  Where the curve claimed 0.0067, the implementable coverage was 0.0000,
  because all 150 items tied. `threshold_feasible_coverage` reports the
  implementable number and a test asserts it never exceeds the curve's.
- **Coverage per pass has a ceiling that disqualifies it as a verdict.**
  Coverage cannot exceed 1.0, so the ratio cannot exceed `1 / passes`, and a
  one-pass baseline above `1/K` wins by construction. The docstring states the
  ceiling and a test asserts it. This is the honest limit on what the milestone
  settles: pricing voting against a *larger model* needs that model, so only
  the within-model question is answered.

**Two defects found by the harness's own no-op test, both fixed**

Voting on an order-invariant model must be an exact no-op, since every pass is
one call repeated. Comparing confidences bitwise missed that, because averaging
`K` bitwise-identical floats does not return that float — the mean of six
copies of `0.7` is `0.7000000000000001`. And a zero-width interval at zero was
labelled "unresolved", which claims the sample was too small to answer a
question it had answered exactly; that case is now reported separately. Both
have tests that reproduce the reason, not just the fix.

**Stated as a limitation, not buried**

The aggregation numbers come from the synthetic deciders, whose defects are of
exactly the kind averaging cancels by construction: the jitter is drawn per
display order and the position term sits on one display slot. That probability
averaging reaches accuracy 1.0 on those fixtures is a property of the fixture,
not a prediction about any model. The README and the research notes say so
where the numbers appear.

**A primary-source re-check that resolved an open item**

`raw.githubusercontent.com` was reachable today, so the Laya README was
retrieved as the raw file (100,070 bytes) rather than through a summarising
fetch. That settles the discrepancy the milestone 6 entry recorded and could
not resolve: the raw-ECE readings of 0.213 and 0.466 were **both correct** and
sit in different sections describing different evaluation sets, alongside
0.175, 0.246, 0.144 and a multilingual 0.733. The correction that follows is to
this project's own notes, which had attributed 0.213 to the wrong checkpoint
row, not to the README. Three of the six figures state no sample size and none
states a bin count, which is the milestone 6 point now evidenced from the
primary source.

**Stop condition**

Milestones 6, 7 and 8 have now all run with no model environment, which meets
the third clause of the project's stop condition. The notes now recommend
milestone 5 be written up as a harness-only report with the empirical H1/H2/H3
claims dropped, unless a model environment becomes available first. No novelty
is claimed anywhere: arXiv:2609.30454 remains unread.

**Verification**

237 tests pass, up from 212, run from a clean detached worktree of the
committed tree under the command the README documents
(`PYTHONPATH=src python3 -m unittest discover -s tests`); `examples/demo.py`
exits 0. The 212-test baseline was confirmed from the same clean checkout
before the change. No linter is configured in that repository. The diff was
scanned for credentials, employer or client material, and generated junk: none
present.

**Not done**

No pull request, no merge, no tag, no release, no settings or visibility
change, no default-branch change. Nothing outside `system1-audit` and this hub
branch was modified. No external or third-party repository was contacted.

---

## 2026-10-01 — Milestone 7 of `system1-audit`: pre-registered sample size and exact power

**Type:** research project (active) · **Repository:** `abhijithvs680/system1-audit`
· **Branch:** `claude/confident-wright-r99bed` · **Commit:** `d6fe495`

Preflight passed before any work: origin matched, tree clean, and a
`git push --dry-run` to a throwaway ref succeeded without a proxy error and
without creating the ref. A second dry-run for the real branch was run
immediately before the push. The branch was based on the active project head
`e82c611`, not on the default branch, so milestones 3 and 6 are carried
forward rather than dropped.

**Why this milestone**

Milestones 2 and 4 are still blocked, re-tested today: arxiv.org and
huggingface.co were both refused by this environment's egress policy, no model
runtime is installed, and no model credentials are available. The next item
that is actually runnable is the one the roadmap names as a precondition for
milestone 2 — "milestone 2 should not be run until that threshold is chosen
and written down". Milestone 6 also left a result it could not explain: an
interval that fails to exclude the null reads identically whether the effect
is absent or the sample was too small to see it, and only the first falsifies
H2. Added `src/system1_audit/prereg.py` (562 lines) and
`tests/test_prereg.py` (459 lines).

**Four results, all measured**

- **Collecting more items can lose the power the plan registered.** Exact
  binomial power is not monotone in `n`, because the rejection count is an
  integer. At a 0.05 threshold and 95% confidence, 33 items need 5 unstable
  items and reach power 0.8179; 34 items need 6 and fall to 0.7004. A plan
  that registered 33 and collected 34 would be underpowered by its own
  criterion with no step having been wrong. `plan_for_rate` now registers
  `stable_from` — the count beyond which no larger sample in range dips back
  below the requested power, 39 here — and one test pins the 33/34 pair.
- **A fixed item budget caps the finding before the audit starts.** Against a
  0.05 threshold at 95% confidence and 80% power, the smallest resolvable true
  instability rate is 0.1905 at 40 items, 0.1340 at 100, 0.0926 at 300 and
  0.0814 at 500.
- **Milestone 6's unexplained "2 of 8" was a power problem, not a correction
  problem.** The 2026-09-30 entry below read it as a family-wise false
  positive. Measured with `empirical_power`: under the null with jitter on,
  the per-position verdict fired in 1 of 40 simulated audits at 60 items —
  inside its nominal 5%, so the Bonferroni correction added in milestone 6 is
  controlling. With `position_weight=0.02` planted it fired 2 of 16 at 60
  items, 14 of 16 at 150 and 16 of 16 at 300; at `0.05` it fired 20 of 20 at
  60, 150 and 300. So 150 items is where the weak bias becomes resolvable,
  which is the number milestone 2 needs for its per-position criterion. The
  past entry is left unedited, as this file requires; the correction is
  recorded in the project notes and here.
- **The demo's own order-sensitivity result does not settle anything.** Read
  against the plan it should have had, the 8-item demo audit returns
  `inconclusive_underpowered` at power 0.203, 31 items short of 39. The
  harness now says that about its own showcase output.

**A pre-existing defect fixed in passing**

`binomial_tail_at_least`, added in milestone 6, raised `OverflowError` for
`n >= 1030` on correct input: it summed terms with exact integer coefficients
and `math.comb(1030, 515)` exceeds the largest representable float, so the
multiplication failed before any arithmetic error could occur. The first call
of `required_items_for_rate` at the default item cap hit it immediately. The
tail is now anchored at the largest term in the summation range and walked
outward with the pmf ratio, the anchor computed through `lgamma`. Verified
against the integer sum it replaced over 8 sample sizes, 6 probabilities and
12 cut-points each: largest disagreement 3.3e-13. Two tests pin the regression
at `n = 1500` and `n = 5000`.

**Verified before commit**

- 212 tests pass under the command the README documents
  (`PYTHONPATH=src python3 -m unittest discover -s tests`), up from 172. All
  172 earlier tests still pass against the rewritten tail.
  `python3 -m compileall` clean.
- The suite got faster, 13.1s to 7.1s, because the rewritten tail drops terms
  once they stop moving the sum.
- Every figure quoted above was read off a run, not recalled.
- Demo output is byte-identical under `PYTHONHASHSEED` of 1, 999 and 12345.
- The duality shortcut behind `minimum_unstable_items` is asserted to agree
  with inverting Clopper-Pearson directly across a grid of 4 sample sizes, 5
  thresholds and 3 confidence levels, rather than trusted.
- No lint ran: `ruff` is not installed in this environment and the repository
  carries no lint configuration. Stated rather than implied.
- Diff scanned for credentials, personal, employer and client identifiers and
  build artifacts: no matches at all this time. Commit author is
  `Abhijith V S` with the GitHub noreply address every prior commit in that
  repository used.

**Not done**

Milestones 2 and 4 were not attempted; both need a model environment, and
milestone 2's arXiv gate is still unmet, so arXiv:2609.30454 remains unread
and no novelty is claimed anywhere. No pull request, merge, issue, comment,
review, discussion, release or tag. No repository created, deleted, archived,
renamed or transferred. No settings, visibility, default branch, security
configuration, access or billing changed. The default branch is untouched at
`7d2270e`. No fork, star, follow, or contact with any third-party maintainer.
The access-preflight refs were never created. Only the two review branches
were pushed: `claude/confident-wright-r99bed` in `system1-audit`, and this one
here.

---

## 2026-09-30 — Milestone 6 of `system1-audit`: intervals on every audited quantity

**Type:** research project (active) · **Repository:** `abhijithvs680/system1-audit`
· **Branch:** `claude/confident-wright-rz38h2` · **Commit:** `e82c611`

Preflight passed before any work: origin matched, tree clean, and a
`git push --dry-run` to a throwaway ref succeeded without a proxy error and
without creating the ref. A second dry-run for the real branch was run
immediately before the push.

**Why this milestone**

The project could not reach its own stop condition. H2 is falsified "if the flip
rate is at chance-level zero and per-position mass sits within sampling noise of
`1/K`", and the harness reported `mean_flip_rate` and `max_position_deviation`
as bare point estimates with no uncertainty attached. Added
`src/system1_audit/significance.py` (670 lines): `Interval`, Wilson and
Clopper-Pearson binomial intervals, exact binomial tails, a seeded percentile
bootstrap, an ECE noise floor, and applied layers for the order-sensitivity and
selective-prediction audits.

**Three results, all measured**

- **The stop condition as written always passes.** Under exact order invariance
  the flip rate is exactly zero, so one flipped item excludes zero at any `n`:
  1 of 40 gives `[0.0006, 0.1316]`, 1 of 1000 gives `[0.0000, 0.0056]`, and both
  "exclude zero" identically. The verdict is now
  `unstable_rate_exceeds(threshold)`, and H2 is restated in the project notes.
- **A bootstrap interval for a maximum is not a confidence interval.** An earlier
  draft of this milestone exposed one for `max_position_deviation`. On an
  order-*invariant* decider it measured `0.0000 [0.0016, 0.0250]` — the point
  estimate outside its own bounds, because the maximum of `K` absolute deviations
  is positively biased under resampling. Tested against zero it would have
  reported position bias on a model with none. The field was removed; one test
  keeps it removed and a second reproduces the defect as the reason.
- **A reported ECE cannot be read without `n` and the bin count.** Binned ECE is
  positively biased, so a perfectly calibrated model scores above zero. The floor
  moves from 0.1211 at n=40 to 0.0228 at n=1000 at 10 bins. On the demo's 8
  items the observed ECE of 0.2374 is *below* the floor's mean of 0.2791
  (p = 0.583) — the headline calibration figure is entirely explained by binning
  noise.

**A pre-existing test defect fixed in passing**

The 2026-09-29 entry below records "94 tests pass". From a clean checkout under
the command the README documents, 2 of those 94 **error**: `test_splits.py`
builds its subprocess environment from scratch so `PYTHONHASHSEED` is the only
variable, which also drops `PYTHONPATH`, so the child could import the package
only where it happened to be pip-installed. Reproduced at clean `HEAD` before
touching it. The earlier claim was true of that environment and not of a fresh
checkout. The past entry is left unedited, as this file requires.

**A source discrepancy, recorded rather than resolved**

Re-checking the Laya README on 2026-09-30 read the `laya` English raw mean ECE
as 0.466, where the 2026-09-28 note in the project recorded 0.213. The post-fit
figures and the multilingual raw figure matched on both dates. Whether the page
changed, the earlier reading took a different row, or a summarising fetch erred
cannot be determined from here. Both readings are recorded in the project notes
and neither is presented as settled. Nothing depends on which is right: the
README states neither `n` nor the bin count, which is what makes either number
unreadable.

**Verified before commit**

- 172 tests pass under the documented command
  (`PYTHONPATH=src python3 -m unittest discover -s tests`), up from 94 of which
  2 errored. `python3 -m compileall` clean.
- Every figure quoted above was re-derived and read off a run, not recalled.
- Demo output is byte-identical under `PYTHONHASHSEED` of 1, 999 and 12345.
- The family-wise false positive is reported honestly: with a weak planted bias
  at n=60 over 8 independent item sets, the sign on the planted position was
  recovered 8 of 8 times, the verdict fired in only 3, and one of those 3 fired
  on a different position — the same trial fires with the bias removed, so it is
  a false positive, not a misattribution.
- No lint ran: `ruff` is not installed in this environment and the repository
  carries no lint configuration. Stated rather than implied.
- Diff scanned for credentials, personal, employer and client identifiers, and
  build artifacts. The only pattern hits were pre-existing prose false positives
  ("price-per-token", "off-schema token", a "cannot reset password" support-ticket
  fixture, and the no-employer-code policy line itself). Commit author is
  `Abhijith V S` with a GitHub noreply address.

**Not done**

Milestone 2 was not attempted: it needs a model environment, and its gate is
unmet — arxiv.org refused this session on 2026-09-30 by both a direct request
and the fetch tool, so arXiv:2609.30454 is still unread and no novelty is
claimed anywhere. No pull request, merge, issue, comment, review, discussion,
release or tag. No repository created, deleted, archived, renamed or
transferred. No settings, visibility, default branch, security configuration,
access or billing changed. The default branch `system1-audit-milestone-1` is
untouched at `7d2270e`. No fork, star, follow, or contact with any third-party
maintainer. The access-preflight refs were never created. Only the two review
branches were pushed: `claude/confident-wright-rz38h2` in `system1-audit`, and
this one here.

---

## 2026-09-29 — Milestone 3 of `system1-audit`: disjoint fit/report splits

**Type:** research project (active) · **Repository:** `abhijithvs680/system1-audit`
· **Branch:** `claude/confident-wright-rz38h2` · **Commit:** `65c3bc4`

The repository blocked on 2026-09-28 now exists and is writable from an agent
session. Preflight passed before any work: origin matched, tree clean, and a
`git push --dry-run` to a throwaway ref succeeded without a proxy error and
without creating the ref.

**What was done**

Milestone 3: moved the project's own disjoint-split evaluation rule out of the
notes and into the library, as `src/system1_audit/splits.py`.

- `deterministic_split` / `split_questions` assign each item to the fit or
  report side by SHA-256 of its own identifier. Identical across processes, and
  unchanged when the dataset grows — a shuffle re-draws every assignment when an
  item is added, invalidating numbers reported against an earlier version of the
  dataset. Blank or duplicate identifiers are rejected, not collapsed.
- `held_out_calibration` fits the temperature on the fit side, reports on the
  report side, and reports the in-sample figure alongside it.
- `check_disjoint` refuses overlapping identifier sets.

Milestone 2 was not attempted: it needs a model environment, and its roadmap
gate (read arXiv:2609.30454 first) is still unmet — arxiv.org was refused by
this environment's egress policy again on 2026-09-29.

**Two results, both measured**

- The in-sample NLL advantage is guaranteed non-negative, because the in-sample
  temperature minimises NLL over exactly the reported items. The ECE advantage
  is **not** guaranteed: the fit targets NLL, and ECE is a binned statistic it
  does not optimise, so the ECE gap changes sign across deciders. The demo
  prints NLL optimism `+0.0011` and ECE optimism `-0.0152` on one audit.
- Temperature scaling recovers a planted sharpening proportionally: sharpness
  1/2/4/8 recovers temperatures 0.7822/1.5643/3.1286/6.2571, a constant ratio
  of 0.7822. Derivable rather than coincidental, since sharpness and `T` enter
  the softmax only through `sharpness / T`. The suite asserts the ratio.

Both are properties of the harness and its synthetic decider. Neither is
evidence about any real model, and no novelty is claimed.

**A correction, not a finding**

`README.md` previously claimed that fitting and reporting a temperature on the
same items "will understate ECE". Measurement showed that is not reliably true.
The wording was corrected in both `README.md` and `RESEARCH_NOTES.md`, and the
argument for disjoint splits now rests on the NLL guarantee. The earlier claim
is named as corrected rather than quietly replaced.

**Verified before commit**

- 94 tests pass, up from 55, under the command the README documents
  (`PYTHONPATH=src python3 -m unittest discover -s tests`).
- A defect in the new tests was caught and fixed before commit: they were first
  written against `pytest`, which `unittest discover` silently skipped — the
  documented command reported 55 passing while 38 new tests never ran, and the
  suite would have gained a third-party dependency in a project whose stated
  design property is having none. Rewritten in `unittest`.
- The demo's split section was moved onto a larger generated set. Eight curated
  items split 5/3, which fitted a temperature on five points and drove it into
  the search bound. Variants carry distinct state text, because repeating
  identical states under new identifiers would place the same decision on both
  sides of the split — the leakage the module exists to prevent.
- Every figure quoted above and in the notes was re-derived and matched to four
  decimal places before being written down.
- Demo output and split assignments are byte-identical under `PYTHONHASHSEED`
  of 1, 999 and 12345.
- Diff scanned for credentials, personal, employer and client identifiers, and
  build artifacts. The only pattern hits were prose false positives
  ("price-per-token", a "cannot reset password" support-ticket example). Commit
  author is `Abhijith V S` with a GitHub noreply address.

**Not done**

No pull request, merge, issue, comment, review, discussion, release or tag. No
repository created, deleted, archived, renamed or transferred. No settings,
visibility, default branch, security configuration, access or billing changed.
The default branch `system1-audit-milestone-1` is untouched at `7d2270e`. No
fork, star, follow, or contact with any third-party maintainer. The
access-preflight ref was never created. Only the two review branches were
pushed: `claude/confident-wright-rz38h2` in `system1-audit`, and this one here.

---

## 2026-09-28 — Research and milestone 1 of `system1-audit`

**Type:** research + new project · **Branch:** `claude/portfolio-management-hub`
(this entry only). The project's own branch,
`claude/system1-audit-milestone-1`, exists **locally only** and was not
pushed, because its repository could not be created.

**What was done**

Researched the "System-1" typed decision model class that appeared in
September 2026, then built and tested milestone 1 of an audit harness for it.

Sources checked on 2026-09-28:

- <https://github.com/NandhaKishorM/laya> — retrieved and read. Apache-2.0.
  Non-autoregressive encoder checkpoints (421M / 322M). Self-reported latency
  32.8 ms single-question multilingual on a Tesla T4. Self-reported
  calibration: ECE 0.213 English and 0.314 multilingual **raw**, 0.081 and
  0.106 after temperature fitting. Its own stated limitations include
  near-chance zero-shot typed-decision accuracy for base checkpoints and that
  the `noul` primitive can follow option labels instead of state content.
- TypeSafe AI's Jev announcement, Tom's Hardware, MarkTechPost, Analytics
  India Magazine and arXiv:2609.30454 — **all blocked** by the session's
  network egress policy and therefore **not read**. Claims attributed to Jev
  in the project's notes are recorded as second-hand search summaries, not as
  retrieved sources.

The harness measures raw and post-fit calibration, option-order sensitivity,
and selective-prediction coverage at a fixed error budget. 1,307 lines of
Python, no third-party dependencies, Apache-2.0 with a NOTICE file.

**Verified before commit**

- 55 unit tests pass. Metric values are asserted against hand-computed
  numbers, not against the implementation's own output.
- The harness is validated against a synthetic decider with deliberately
  planted defects, so a test can assert it recovers a bias injected on
  purpose. Injected first-position weight of 0.9 is recovered as measured
  position bias of 0.9 against a 0.25 uniform.
- Suite and demo output are byte-identical under `PYTHONHASHSEED=1` and
  `PYTHONHASHSEED=999`, confirming audits reproduce across processes.
- Two defects found and fixed before the commit: the synthetic decider seeded
  from `hash()` of strings, which is salted per interpreter run and would have
  made audits irreproducible; and the dataset audit re-querying the model to
  compute position bias, doubling inference calls against a real model.
- Scanned the tree for credentials, personal or employer identifiers and
  generated files. No matches. The commit author is `Abhijith V S` with a
  GitHub noreply address, so no personal or employer email enters public
  history.

**Honest limitations recorded in the project**

No real model has been audited; every number in the demo describes the
harness, not any product. Only the `choice` primitive is implemented. No
vendor adapter was written, because the package's call signature could not be
verified from this environment and a guessed adapter would present untested
code as a tested integration. arXiv:2609.30454 may be prior art for the entire
premise and must be read before any public novelty claim.

**Blocked**

The account owner approved creating `abhijithvs680/system1-audit`, choosing
Apache-2.0, and pushing the review branch. Two independent authorisation
limits prevented the first and third:

- `POST /user/repos` returned `403 Resource not accessible by integration`.
- `git push --dry-run` to the intended URL was refused by the git proxy:
  the repository is not in the session's authorised set, so no credential is
  injected.

One attempt each, no workaround attempted. The repository must be created by
the account owner and added to the automation's authorised repository set
before any run can push to it.

**Not done**

No merge, pull request, issue, comment, discussion, release or tag. No
repository was created, deleted, archived, renamed or transferred. No
settings, visibility, default branch, security configuration, access or
billing were changed. No fork, star, follow or contact with any third-party
maintainer. Nothing outside `abhijithvs680/abhijithvs680` was written to, and
only the review branch was pushed there.

---

## 2026-09-26 — Create portfolio management hub

**Type:** documentation · **Branch:** `claude/portfolio-management-hub`
(created fresh from `origin/main`)

**What was done**

Added four files to this profile repository, populated from the read-only
audit logged below:

- `CLAUDE.md` — standing operating contract (81 lines)
- `PORTFOLIO.md` — durable audited facts about public repositories
- `ROADMAP.md` — 30-day plan, grouped by week, with approval gates
- `JOURNAL.md` — this log

**Verified before commit**

- Branch was created from `origin/main` at `ba8d315`; `main` was not modified.
- `README.md` was not edited.
- Exactly four files changed, all additions: `CLAUDE.md`, `JOURNAL.md`,
  `PORTFOLIO.md`, `ROADMAP.md`. No other file in the working tree changed.
- Privacy scan: an explicit denylist of private repository names returned zero
  matches across all four files. The full diff was reviewed for other
  non-public identifiers, email addresses, secrets, tokens, and URLs
  containing credentials — none present.
- A first draft of `PORTFOLIO.md` did list private repository names and
  metadata. That draft was **never committed and never pushed**. It was
  rewritten before any git history existed, replacing the private inventory
  with a generic exclusion policy and removing private identifiers from the
  do-not-highlight and naming sections and from all account totals.

**Not done**

No merge, no pull request, no issue, no comment, no discussion, no release,
no tag. No repository settings, visibility, default branch, security
settings, access, or billing were changed. No repository was created,
deleted, archived, renamed, or transferred. Nothing outside
`abhijithvs680/abhijithvs680` was touched. Only the new branch was pushed.

---

## 2026-09-26 — Read-only account audit

**Type:** audit · **Branch:** none (read-only; no branch created)

**What was done**

Full read-only audit of the public repositories owned by `abhijithvs680`,
covering inventory and maintenance state, portfolio strength, quality gaps,
duplicate and low-signal repositories, a 30-day improvement plan, a candidate
first code change, and a proposed instruction-file structure.

**Method**

- Enumerated public repositories via the GitHub API.
- Cloned 17 public repositories read-only into a temporary session scratchpad
  using `--filter=blob:none`. One public repository is an upstream fork and
  was skipped as third-party code.
- Measured commit counts, author names, date ranges, branch structure, file
  trees, source LOC, and test presence directly from git history.
- Scanned every branch of every cloned repository for committed credential
  files and for live-pattern secrets at HEAD.
- Private repositories were **not** cloned and their contents were **not**
  read. This session was scoped to `abhijithvs680/abhijithvs680`, and no
  additional repository was attached.

**Verified findings of note**

- `bank-node`: the default branch holds an 11-byte README while 235 files and
  80 commits sit on a non-default branch. The work is invisible to visitors.
- `Voice-Agent-v2` is a near-exact duplicate of `Voice-Agent`; every tracked
  file is byte-identical except `README.md` and `AI_USAGE.md`.
- `clothe-connect-hub`: all three commits are bot-authored.
- No credential file has ever been committed on any branch of any cloned
  public repository, and no live-pattern secrets are present at HEAD.
- No public repository has a LICENSE, a `.github/` directory, or topics.
- `deepcode` `tests/test_config.py::test_defaults` is environment-dependent
  and fails whenever settings are present in the environment. Reproduced in a
  clean virtualenv: passes on a clean environment; fails with
  `LLM_MODEL=llama3` exported; fails with a developer env file present;
  passes again once removed. 22 tests collect cleanly; 8 of 18 test files
  fail at collection on missing dependencies.

**Not done**

Nothing was created, edited, or deleted in any repository. No branches,
commits, pushes, pull requests, issues, comments, discussions, workflows,
hooks, releases, or settings changes. No repositories were attached to the
session. The working tree was left clean and unchanged at `ba8d315`.
