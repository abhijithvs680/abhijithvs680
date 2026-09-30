# JOURNAL.md — Work Log

Append-only. Newest first. Add an entry only **after** work is verified.
Never edit or remove a past entry.

Entries record public repositories only. Do not record names or metadata of
private or employer repositories here — this repository is public.

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
