# PORTFOLIO.md — Public Repository Facts

Durable reference derived from the read-only audit of 2026-09-26.
Facts here are measured, not estimated. Refresh on re-audit.

## Scope and privacy

This hub is a **public** repository and records facts about **public
repositories only**.

Private and employer repositories are excluded from this hub entirely. Their
names, counts, languages, sizes, branch names, topics, and any other
identifying metadata must not be recorded here. They remain private, they are
not portfolio material, and no part of them — including excerpts, summaries,
rewrites, or derivative public projects — may be used for portfolio work
without documented owner and employer clearance.

**Audit coverage:** 19 public repositories were enumerated. 17 were cloned and
analysed in depth; one is an upstream fork and was skipped as third-party
code, and one is this profile repository.

**Measurement notes:** maintenance state uses *commit dates*, not `pushed_at`
(several repositories show a recent `pushed_at` from settings changes with no
new commits). LOC counts source files only, excluding lockfiles, notebooks,
and vendored assets.

---

## Tier A — Substantial, role-relevant

| Repo | Lang | LOC | Commits | Last commit | Tests | State |
|---|---|---|---|---|---|---|
| `deepcode` | Python | 22,611 | 55 | 2026-04-10 | 17 files / 1,012 LOC | Dormant |
| `Advanced-Multi-Agent-AI-Research-Platform` | Python | 15,448 | 5 | 2026-01-02 | 3 real files / 408 LOC | Stale |
| `wokflow-automation` | Python | 2,063 | 12 | 2026-07-23 | 2 files | Dormant |
| `Voice-Agent` | Python | 901 | 1 | 2026-07-16 | none | Single-commit |
| `AI-Voice-Service` | Python | 809 | 2 | 2026-02-16 | none | Stale |
| `bank-node` | TS/JS | 0 on `main` | 1 on `main` | 2026-09-25 | none | See note |

**`bank-node` structural note.** The default branch `main` holds a single
11-byte README. The actual project — 235 files, 80 commits, last commit
2026-09-25 — lives on `feature/platform-socket`. Two further branches carry
it as well. Visitors currently see an empty repository.

**`Advanced-Multi-Agent-AI-Research-Platform` caveat.** 15,448 LOC landed in
essentially one commit across five days. `tests/e2e` and `tests/unit` are
largely empty `__init__.py` files. Reads as a generated scaffold rather than
iteratively built software. Medium risk to highlight.

## Tier B — Real work, secondary signal

| Repo | Lang | LOC | Commits | Last commit | Note |
|---|---|---|---|---|---|
| `Workflow-Concept` | TypeScript | 19,809 | 2 | 2026-09-06 | No README, no description; default branch `master` |
| `Autopay` | TypeScript | 2,939 | 9 | 2026-03-31 | 13-byte README; Vite + Supabase + Docker + nginx |
| `GoodDoc` | JavaScript | 2,324 | 12 | 2025-11-11 | Longest sustained history (Mar–Nov 2025); co-authored |
| `multi_agent_terminal` | Python | 1,625 | 4 | 2025-12-05 | LangGraph; 3 test files; 6.3 KB README |
| `PII-data-masking` | Python | 855 | 6 | 2026-07-06 | GLiNER + FastAPI + React; default branch `master` |
| `Patient-Interoperability-Gateway` | Python | 729 | 2 | 2026-07-15 | Django; 3.8 KB README |

## Tier C — Low signal

| Repo | LOC | Commits | Finding |
|---|---|---|---|
| `Voice-Agent-v2` | 901 | 1 | Near-exact duplicate of `Voice-Agent`; every tracked file byte-identical except `README.md` and `AI_USAGE.md` |
| `clothe-connect-hub` | 7,009 | 3 | All commits authored by `gpt-engineer-app[bot]`; zero human commits |
| `LanguageModels` | 0 | 2 | 2 files, 8 KB, one notebook |
| `MIC` | 364 | 2 | 6 files; chest X-ray DenseNet coursework |
| `E-commece` | 6,134 | 1 | 2023; no README; typo in repository name |
| `agno` | — | — | Upstream fork, no meaningful divergence |

---

## Showcase candidates — conditional, not yet ready

`deepcode`, `wokflow-automation`, and `bank-node` are the strongest technical
assets. **None is ready to showcase today.** Each becomes a showcase candidate
only after its listed gaps are closed.

**`deepcode`** — strongest codebase in the account: genuine multi-agent
architecture (hierarchical planner, reasoning loop, critic agent, adaptive
controller, context builder, reranker), pgvector RAG, Textual TUI, evals
harness, and 55 commits of visible iterative debugging.
Blocking gaps:
- README is generated word-salad prose and actively undermines the code it
  describes. Must be rewritten in plain technical English.
- 8 of 18 test files fail at collection on missing dependencies; 22 tests
  collect cleanly.
- No LICENSE, no CI, no description, no topics.
- Placeholder package author metadata.
- Version drift: `pyproject` says `1.0.0`, config says `APP_VERSION = "0.1.0"`.
- Dependencies pinned with `>=` only; builds are not reproducible.
- Defaults `HOST = "0.0.0.0"`.

**`wokflow-automation`** — best engineering-judgment story in the account
despite its size. The LLM emits only a compact IR; deterministic Python
generates everything mechanical (ids, positions, edges, PHP-style encoding).
Round-trip tests run against real production documents.
Blocking gaps:
- Repository name contains a typo (`wokflow`).
- No LICENSE, no CI, no description, no topics.
- Dependencies pinned with `>=` only.

**`bank-node`** — 80 commits of sustained, recent, real-world debugging.
Blocking gaps:
- The work is invisible: the default branch is effectively empty. Resolving
  this requires a human decision (see `ROADMAP.md`).
- 11-byte README, no LICENSE, no CI, no description, no topics.

---

## Do not highlight

Do not recommend, pin, link from the profile, or present any of the following
as portfolio work:

- **`Voice-Agent-v2`** — duplicate of `Voice-Agent`. Showing both reads as
  padding. Keep `Voice-Agent`, which has the better README and an
  `AI_USAGE.md` disclosure.
- **`clothe-connect-hub`** — entirely bot-authored. Presenting it as personal
  engineering work would be a false claim.
- **`LanguageModels`** — 2 files, 8 KB. Currently linked from the profile
  README as a headline project; it does not support that billing.
- **`MIC`** — coursework scale.
- **`E-commece`** — 2023, single commit, no README, misspelled name.
- **`agno`** — a fork. Forks dilute the profile grid.

Private and employer repositories are out of scope for highlighting by
policy, independent of their contents. See **Scope and privacy** above.

**Also note:** the profile `README.md` currently showcases `AI-Voice-Service`,
`LanguageModels`, and `PII-data-masking`. Two of those three are among the
weakest public artifacts.

---

## Account-wide findings (public repositories)

**Zero-exception gaps:**
- No LICENSE file in any of the 18 non-fork public repositories.
- No `.github/` directory in any of the 17 analysed public repositories — no
  CI, no issue or PR templates, no CODEOWNERS, no Dependabot configuration.
- No topics on any public repository; 17 of the 18 non-fork public
  repositories have no description.

**Documentation:** no README in `Workflow-Concept` and `E-commece`; stub
READMEs in `Autopay` (13 bytes) and `bank-node` (11 bytes).

**Testing:** tests exist in only 4 of the 17 analysed public repositories
(`deepcode`, `Advanced-Multi-Agent-AI-Research-Platform`,
`wokflow-automation`, `multi_agent_terminal`). No repository has CI, so no
suite is verified by anything other than a local run.

**Iteration depth:** 4 public repositories have a single commit; 5 more have
two. 9 of 17 are effectively code dumps with no visible iteration.

**Security hygiene — verified clean:**
- Across all branches of all 17 analysed public repositories, no `.env`,
  `.pem`, `.key`, `.p12`, `id_rsa`, or credentials file has ever been
  committed.
- No live-pattern secrets present at HEAD (checked `sk-`, `AIza`, `AKIA`,
  `ghp_`, `xox*`, `SG.`, Twilio `AC…`, and PEM private-key blocks).
- `.env.example` used correctly in 7 repositories; `.gitignore` in 11 of 17.
- `wokflow-automation` explicitly changed a Uvicorn bind from `0.0.0.0` to
  `127.0.0.1`.

**Security hygiene — gaps:**
- No Dependabot, CodeQL, secret scanning, or `SECURITY.md` in any analysed
  public repository.
- Dependencies pinned with `>=` only in `deepcode` and `wokflow-automation`.
- `deepcode` defaults `HOST = "0.0.0.0"`; the safer default used in
  `wokflow-automation` was not applied there.

**Metadata:**
- Commits authored under five name variants: `Abhijith`, `Abhijith v s`,
  `Abhijith V S`, `abhijith`, `abhijithvs680`.
- Default branch is `master` on `PII-data-masking` and `Workflow-Concept`,
  `main` elsewhere.

**Naming issues:** `wokflow-automation` and `E-commece` are misspelled.
`Workflow-Concept` and `wokflow-automation` are adjacent names covering
different projects, which makes the public grid harder to navigate.
