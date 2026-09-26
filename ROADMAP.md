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
