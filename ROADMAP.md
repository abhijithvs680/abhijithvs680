# ROADMAP.md — 30-Day Plan

Derived from the read-only audit of 2026-09-26. Read `PORTFOLIO.md` for the
facts behind each item and `CLAUDE.md` for the rules that govern all of it.

## Working rules

- **Nothing becomes in-progress until a branch name is recorded** in the
  Branch column of its row. An item with an empty Branch cell has not been
  started.
- All work lands on a branch. Never on `main`. Never force-pushed.
- Test before proposing review. Merge approval is always the human's.
- Items labelled **HUMAN APPROVAL REQUIRED** are not authorised by the
  presence of this file. They are listed so they are not forgotten, not so
  they can be executed. An agent may prepare, draft, or describe them; it may
  not perform them until a human approves that specific action.

## Legend

Impact and Effort are H / M / L. Status is one of: not started, in progress,
in review, done.

---

## Active research project

**None. `system1-audit` is closed as of 2026-10-03.** The next run starts a new
research project — see "Starting the next project" below.

### `system1-audit` — closed 2026-10-03

**Final state.** Milestones 1, 3, 5, 6, 7, 8 and 9 complete, tested and pushed
for review on `claude/confident-wright-383ue1` (commit `da7c5cc`). Milestones 2
and 4 closed **blocked and unstarted**.

A dependency-free audit harness for "System-1" typed decision models — models
returning a typed, probabilistic answer in one forward pass instead of generating
text — measuring calibration, option-order sensitivity, and selective prediction,
with an interval on every audited quantity, a noise floor beside every ECE, a
pre-registered sample size and threshold, aggregation priced per forward pass, and
a selective-prediction operating point a deployment can actually set.

**It never audited a model, and claims no novelty.** Both are gates that never
opened: no model runtime (`torch` absent), no model credentials, and
arXiv:2609.30454 — whose title covers all three audited axes, so it is potential
prior art for the whole premise — was refused by this environment's egress policy
on all five attempts (2026-09-29, 09-30, 10-01, 10-02, 10-03). `REPORT.md` is the
harness-only write-up, which is the project's own stop condition being honoured.

| # | Milestone | Final status |
|---|---|---|
| 1 | Dependency-free metric layer; tests validating it against planted defects | done |
| 2 | Adapter for an open checkpoint | **closed, blocked** — no model environment, arXiv gate unmet |
| 3 | Disjoint fit/report split discipline, enforced by the library | done |
| 4 | LLM structured-output baseline on the same items | **closed, blocked** — no credentials |
| 5 | Write-up: coverage at a fixed error budget, with honest limitations | done — `REPORT.md` |
| 6 | Significance layer: an interval on every audited quantity, plus an ECE noise floor | done |
| 7 | Pre-registration and power: threshold and sample size fixed before the run | done |
| 8 | Vote aggregation priced per forward pass, with a paired interval on the gain | done, **headline later withdrawn** (see below) |
| 9 | A selective-prediction operating point a real cutoff can deliver | done |

**To resume the empirical work** (not scheduled; listed so the conditions are not
lost): read arXiv:2609.30454 first, since it gates any novelty claim — needs
network access to arxiv.org. Then milestone 2 needs a GPU or patient CPU with
`torch` and `transformers` plus the Apache-2.0 Laya weights, must register a plan
and quote its `plan_id`, and must report `n` and the bin count with any ECE.
Milestone 4 needs credentials for one provider. `REPORT.md` section 6 has the
detail.

**Two self-corrections worth carrying forward as method, not just history.**
Milestone 9 found the harness was reporting a confidence threshold as meeting a
zero-error budget while it carried 0.333 realised error — an operating point that
did not exist, because a risk-coverage curve point can stop inside a group of tied
confidences and no cutoff can. Milestone 5 then withdrew milestone 8's headline
("a vote share is not a usable abstention gate"), which had generalised from one
fixture; across three fixtures the vote improves coverage on two. Both were caught
by making the claim reproducible rather than by re-reading it: the second surfaced
only because the report was written to quote a script
(`examples/report_numbers.py`) instead of transcribed numbers. **Write the figures
as a regenerable script from the start on the next project.**

**Outstanding for the human on this repository**, carried from the two 2026-10-03
journal entries: the default branch is still `system1-audit-milestone-1`, six
milestones behind the review chain. Promoting it, and merging the review branch,
are settings and merge decisions and are the human's alone.

---

## Starting the next project

The human's instruction on 2026-10-03: from the next run onward, research and
select a new, most-relevant task rather than continuing or reviving old work.

Constraints that apply to the choice, from `CLAUDE.md` and the standing routine:

- Verify current primary sources before proposing anything. Record source dates
  and links; distinguish evidence from marketing.
- Define a falsifiable question and a stop condition **before** building, and
  check licences and prior art. An unread prior-art candidate means no novelty
  claim — that is the lesson `system1-audit` paid for.
- **Check the execution environment's limits against the hypothesis before
  committing to it.** `system1-audit` spent nine milestones on a question it could
  not answer here, because the harness was buildable offline and the audit was
  not. Prefer a question this environment can actually settle, or scope the
  deliverable to what it can.
- Write every quantitative claim as a regenerable script with a test, from the
  first milestone.
- A new repository is **not** authorised by this file. Prepare it locally, then
  ask: it must be added to the routine's repository set before any later run can
  push to it.

---

## Week 1 — Presentation

Highest leverage, lowest risk. Nothing here changes settings or history.

| # | Task | Target repo | Impact | Effort | Branch | Status |
|---|---|---|---|---|---|---|
| 1 | Rewrite README in plain technical English: one-sentence purpose, architecture, honest limitations, real setup steps. Replaces generated prose. Highest-value single item in this plan. | `deepcode` | H | M | _(none)_ | not started |
| 2 | Repoint profile README at the three strongest repos; drop `LanguageModels` as a headline link. Do only after the Week 1–2 gaps close, so links point at presentable repos. | `abhijithvs680` | H | L | _(none)_ | not started |
| 3 | **HUMAN APPROVAL REQUIRED** — Add descriptions and 3–5 topics to all 19 public repositories. Repository-settings change. Agent may draft the text; the human applies it. | all public | H | L | _(none)_ | not started |
| 4 | **HUMAN APPROVAL REQUIRED** — Decide how to surface the 80 commits on `feature/platform-socket`. Options: promote the branch, or make the repository private. Both are settings/default-branch changes and are the human's call alone. | `bank-node` | H | L | _(none)_ | not started |

## Week 2 — Legitimacy

| # | Task | Target repo | Impact | Effort | Branch | Status |
|---|---|---|---|---|---|---|
| 5 | **HUMAN APPROVAL REQUIRED** — Add a LICENSE to the six showcase-track repositories. Licensing is a legal choice (MIT vs Apache-2.0 patent grant); the human picks. Agent may add the chosen file on a branch once told which. | Tier A/B | H | L | _(none)_ | not started |
| 6 | Add `.github/workflows/ci.yml` running lint plus the offline-passing test subset. A green badge on the strongest repo outweighs any README paragraph. | `deepcode`, `wokflow-automation` | H | M | _(none)_ | not started |
| 7 | Fix the 8 test-collection errors: install the `dev` extra in CI, or guard dependency-heavy tests with `pytest.importorskip`, so `pytest` is green from a clean checkout. | `deepcode` | H | M | _(none)_ | not started |
| 8 | Set canonical commit identity going forward (`git config --global user.name "Abhijith V S"`). Do **not** rewrite history to correct past variants. | local config | M | L | n/a | not started |

## Week 3 — Substance

| # | Task | Target repo | Impact | Effort | Branch | Status |
|---|---|---|---|---|---|---|
| 9 | Write a README. 19,809 LOC with zero explanation is the largest unexplained asset in the account. | `Workflow-Concept` | H | M | _(none)_ | not started |
| 10 | Write a real README covering the Supabase schema, Docker, nginx, and PWA setup. Currently 13 bytes. | `Autopay` | M | M | _(none)_ | not started |
| 11 | Pin dependencies (`==` or a lockfile) for reproducible builds. | `deepcode`, `wokflow-automation` | M | L | _(none)_ | not started |
| 12 | **HUMAN APPROVAL REQUIRED** — Enable Dependabot, secret scanning, and CodeQL on showcase repositories. Security-settings change; the human applies it in repository settings. | showcase repos | M | L | _(none)_ | not started |

## Week 4 — Consolidation

Every item in this week is a destructive or settings change. None is
pre-authorised.

| # | Task | Target repo | Impact | Effort | Branch | Status |
|---|---|---|---|---|---|---|
| 13 | **HUMAN APPROVAL REQUIRED** — Archive the duplicate; keep `Voice-Agent`. | `Voice-Agent-v2` | M | L | n/a | not started |
| 14 | **HUMAN APPROVAL REQUIRED** — Archive or delete `clothe-connect-hub` and `E-commece`; unfork `agno`. Irreversible; human executes. | various | M | L | n/a | not started |
| 15 | **HUMAN APPROVAL REQUIRED** — Rename `wokflow-automation` to `workflow-automation`. GitHub redirects, but renaming is still the owner's action. | `wokflow-automation` | M | L | n/a | not started |
| 16 | Add a short honest Status line to each stale repository README (e.g. "exploratory, not maintained"). Forecloses the "why is everything abandoned?" question at no cost. | stale repos | M | M | _(none)_ | not started |
| 17 | Fix placeholder package author metadata and the `1.0.0` / `0.1.0` version drift. | `deepcode` | L | L | _(none)_ | not started |

---

## Explicitly out of scope

Not to be done under any circumstances, and not to be proposed again:

- Back-dated commits, contribution-graph padding, or any manufactured activity.
- Inflated or unverifiable capability claims in any README.
- Presenting bot-authored code as personal engineering work.
- Making dormant projects look actively maintained. Several repositories are
  genuine one-day builds and are to be described as such.
- Publishing, excerpting, or rewriting private or employer code without
  documented employer clearance.

## External actions

**HUMAN APPROVAL REQUIRED** for all of the following, individually:
publishing to external open source, opening issues or pull requests,
commenting, or making any contribution to a repository not owned by
`abhijithvs680`. Nothing in this roadmap authorises any external action.
