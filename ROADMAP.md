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

Read this section first. A run that finds an unfinished active research
project continues it rather than starting something newer.

### `system1-audit` — audit harness for typed decision models

**Status:** milestones 1, 3, 6 and 7 complete, tested and pushed for review.
**Branch:** `claude/confident-wright-r99bed` (commit `d6fe495`, 2026-10-01),
based on `claude/confident-wright-rz38h2` (`e82c611`, 2026-09-30).
Repository default branch is `system1-audit-milestone-1`.

A dependency-free harness that measures three properties of "System-1" typed
decision models — models returning a typed, probabilistic answer in one
forward pass instead of generating text:

1. calibration, reported raw as well as post-temperature-fit
2. option-order sensitivity of `choice` answers
3. selective prediction: coverage at a fixed error budget

and, since milestone 6, an interval on each of them, because the project's own
falsification criteria are stated in terms of an effect being distinguishable
from zero and a point estimate cannot answer that.

Motivated by the open Apache-2.0 Laya README, which self-reports raw ECE of
0.213 (English) and 0.314 (multilingual) falling to 0.081 / 0.106 only after
temperature fitting, and notes that its `noul` primitive can follow option
labels rather than state content.

**Blocker cleared 2026-09-29.** The repository now exists and is in the
automation's authorised set; a preflight dry-run push succeeded. The milestone 1
work that was local-only on 2026-09-28 is published.

**Remaining blocker (milestones 2 and 4).** No model environment, and the
arXiv:2609.30454 gate below is still unmet. Re-tested 2026-10-01: arxiv.org
and huggingface.co were both refused by the session's egress policy, no model
runtime is installed, and no model credentials are available, so no checkpoint
can be fetched and the LLM baseline cannot be run either.

| # | Milestone | Status |
|---|---|---|
| 1 | Dependency-free metric layer; tests validating it against planted defects | done, published |
| 2 | Adapter for an open checkpoint; needs a GPU or patient CPU environment | blocked — no model environment, arXiv gate unmet |
| 3 | Disjoint fit/report split discipline, enforced by the library | done, 94 tests passing, pushed for review |
| 4 | LLM structured-output baseline on the same items | not started |
| 5 | Write-up: coverage at a fixed error budget, with honest limitations | not started |
| 6 | Significance layer: an interval on every audited quantity, plus an ECE noise floor | done, 172 tests passing, pushed for review |
| 7 | Pre-registration and power: threshold and sample size fixed before the run | done, 212 tests passing, pushed for review |

**Before milestone 2:** read arXiv:2609.30454, *Auditing System-1 Models on
Biosecurity-Relevant Benchmarks: Calibration, Selective Prediction, and
Permutation Instability in a Non-Generative Model*. Its title covers all three
audited axes, so it may be prior art for the whole premise. It was not read
when this project was scoped — arxiv.org was blocked by the session's network
egress policy. No novelty may be claimed publicly until it has been read.

**Stop condition.** Stop and write up if the order-sensitivity effect proves
indistinguishable from zero on the open checkpoint, if arXiv:2609.30454
already reports it with a stronger method, or if three milestones pass with
no runnable model environment.

**Corrected by milestone 6.** The first clause is not testable as written. Under
exact order invariance the flip rate is exactly zero, so a single flipped item
excludes zero at any `n` — the exact interval for 1 item in 40 is
`[0.0006, 0.1316]`, which excludes zero just as firmly as 1 in 1,000,000 would.
Read it instead as: stop if the unstable-item rate cannot be put above a stated
threshold and no per-position deviation clears a family-wise interval. Milestone
2 should not be run until that threshold is chosen and written down, since
choosing it after seeing the results is the same defect the disjoint-split rule
exists to prevent.

**Milestone 7 settled the sample size milestone 2 was gated on.** The threshold
is now registered in a `PreregisteredPlan` before the run, and the item counts
are computed rather than guessed: against a 0.05 unstable-rate threshold at 95%
confidence and 80% power, 39 items suffice for an assumed rate of 0.20, and a
budget of 40 items cannot resolve any rate below 0.1905 however the audit comes
out. For the per-position criterion, measured power says 150 items — a weak
planted bias (`position_weight=0.02`) resolves in 2 of 16 simulated audits at
60 items but 14 of 16 at 150. Milestone 2 should register its plan and quote
the `plan_id` with its results.

**Milestone 2 now also requires** recording `n` and the bin count with any ECE,
and reporting the noise floor beside it. Milestone 6 measured that floor moving
from 0.1211 at n=40 to 0.0228 at n=1000 at 10 bins, so an ECE quoted without
both numbers cannot be compared to anything — including the raw-versus-post-fit
comparison that motivated this project.

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
