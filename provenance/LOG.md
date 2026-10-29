# Provenance Log

Every customization and decision made while building **Paperclip for Real Estate**, in order.
Each entry records: who decided, what, why, and the artifact it produced.
This log is the source material for the companion article and video.

Format: `ID · date · actor · type · summary` followed by rationale and artifacts.
Actors: `human` (Dylan Lennon), `claude` (Claude Code, Sonnet 5.5).
Types: `goal`, `decision`, `customization`, `setup`, `result`.

---

## P-001 · 2026-10-29 · human · goal
Dylan runs an AI mastermind for real estate agents and wants to know how to create and run a
real estate company from code.
- Artifact: original prompt (conversation turn 1)

## P-002 · 2026-10-29 · human · decision
Use the open-source Paperclip project (https://github.com/paperclipai/paperclip) as the
orchestration layer instead of building agent orchestration from scratch.
- Why: Paperclip already provides org charts, budgets, approval gates and a dashboard.
- Artifact: https://github.com/paperclipai/paperclip

## P-003 · 2026-10-29 · claude · decision
Read the upstream README before committing to the approach. Finding: Paperclip is an
orchestrator for AI agents (Node.js server + React UI), not a business generator. The
"real estate company" therefore has to be configured inside it: goal, org chart, skills,
governance, budgets.
- Artifact: this log; chat explanation to Dylan

## P-004 · 2026-10-29 · human · decision
Approve cloning Paperclip locally to `~/paperclip` and installing/running it.
- Why: low-risk, local-only; Dylan said "YES".

## P-005 · 2026-10-29 · human · goal
Document the entire build for an article and a YouTube video; present at the end how well
the business goal was met. Everything is to be built and run toward the real estate
business goal.

## P-006 · 2026-10-29 · human · decision
Publish the work as a public GitHub repo named "paperclip for real estate" so other people
can actually use it.
- Design choice (claude): the repo is a *company template pack* layered on top of upstream
  Paperclip (org chart, skills, governance, setup script, provenance), not a copy of
  Paperclip's source. Users install upstream Paperclip and apply the pack.
- Why: easier to keep current with upstream, and avoids redistributing a 200 MB codebase.

## P-007 · 2026-10-29 · human · decision
All customizations and decisions must be provenance-tracked so the video, article and code
can all be derived from the record. This file plus per-artifact `Provenance:` headers are
the mechanism.

## P-008 · 2026-10-29 · claude · setup
Started `git clone` of upstream into `~/paperclip` (large repo, exceeded the 120 s tool
timeout and moved to background). Created the empty template repo at
`~/paperclip-for-realestate` with this log.

## P-009 · 2026-10-29 · claude · decision
Machine has Node 23.11; Paperclip requires >=24.11. Rather than change the system Node,
run Paperclip through `npx -p node@24` so a temporary Node 24 is used for that process only.
- Why: no system-wide change, easily reversible.
- Status: server start in progress at time of writing (see later entries for outcome).

## P-010 · 2026-10-29 · claude · decision
Package the company in the Agent Companies format (`agentcompanies/v1`) with a
`.paperclip.yaml` sidecar, instead of a custom script that calls Paperclip's API.
- Why: Paperclip's `company import <owner>/<repo>` can install it straight from GitHub, so
  anyone can use it with one command. Markdown stays readable and portable to other runtimes.
- Source: `docs/companies/companies-spec.md` in paperclipai/paperclip (read 2026-10-29).
- Artifacts: `COMPANY.md`, `.paperclip.yaml`

## P-011 · 2026-10-29 · claude · customization
Org design: one Chief of Staff (the Paperclip "ceo" role) with five direct reports: Lead
Concierge, Market Analyst, Content Editor, Transaction Coordinator, Compliance Officer.
The licensed human is the owner above the org chart.
- Why: maps to the four jobs a solo agent or small team actually has (leads, content, deals,
  risk) and keeps compliance as its own checkpoint rather than an afterthought.
- Artifacts: `agents/*/AGENTS.md`

## P-012 · 2026-10-29 · claude · decision
Governance: agents prepare, the human decides. A shared `approval-gate` skill defines what
always needs approval (anything client-facing or public, anything touching price, contract or
fees, legal/tax/lending statements, over-budget spend, consent changes). The Compliance
Officer reviews all outbound drafts first and its result is attached to the approval request.
- Why: licensing law and Fair Housing exposure sit with the human; an AI-only brokerage is not
  a legal option. Also the main trust requirement for other agents adopting this.
- Artifacts: `skills/approval-gate/SKILL.md`, `skills/fair-housing-copy/SKILL.md`,
  `agents/compliance-officer/AGENTS.md`, `COMPANY.md` operating rules

## P-013 · 2026-10-29 · claude · decision
Market-agnostic: no city is hard-coded. Four required inputs (`MARKET`, `STATE`,
`BROKERAGE_NAME`, `AGENT_NAME`) declared in `.paperclip.yaml`.
- Why: Dylan wants other people to use it; examples reference Asheville/NC only as sample values.
- Artifacts: `COMPANY.md`, `.paperclip.yaml`

## P-014 · 2026-10-29 · claude · customization
Skills and starter work: five skills (`lead-qualification`, `market-report`,
`transaction-timeline`, `fair-housing-copy`, `approval-gate`); four projects (Setup, Lead
Pipeline, Local Authority, Operations) with a one-time first-run task and five recurring
routines (lead triage hourly in business hours, weekly snapshot and newsletter on
Wednesdays, Monday owner brief, daily deadline digest).
- Why: the recurring routines turn the goals in `COMPANY.md` into scheduled work.
- Scoring weights in `lead-qualification` (30/25/20/15/10) are an initial judgment call, not
  validated against real conversion data. To be tuned.
- Artifacts: `skills/*`, `projects/*`

## P-015 · 2026-10-29 · claude · decision
Runtime defaults: all agents use the `claude_local` adapter; Opus for the Chief of Staff and
Compliance Officer (judgment-heavy), Sonnet for the rest. Monthly budgets: Chief $30, Lead
Concierge $30, Analyst $20, Editor $20, Coordinator $20, Compliance $15 (total $135).
Schedules default to America/New_York.
- Why: budgets are a guess to be calibrated from real usage in the first weeks.
- Field names (`adapter.type`, `budgetMonthlyCents`, `inputs.env`, `routines.<task>.triggers`)
  verified against `server/src/services/company-portability.ts` in the Paperclip source.
- Artifacts: `.paperclip.yaml`

## P-016 · 2026-10-29 · claude · result
First attempt to start Paperclip failed: `ENOSPC: no space left on device` while npm was
unpacking dependencies. The disk was 100% full (114 MB free of 228 GB). It left a corrupt 928 MB
partial install in `~/.npm/_npx/cb4559dfd0885d66`.
- Decision: delete only that directory (created by this run, unusable) and retry. Did not touch
  any pre-existing user files (e.g. ~/Downloads is 6.6 GB; ~/.npm cache is otherwise untouched).
- Outcome: free space rose to 4.2 GB; retried the start (outcome logged below).
- Lesson for the article: "run it from code" has real environmental prerequisites. Added a
  disk-space note to the README.

## P-017 · 2026-10-29 · claude · result
Retry succeeded. `paperclipai onboard --yes` (quickstart: embedded Postgres, local disk,
loopback only) started Paperclip at http://127.0.0.1:3100 using a temporary Node 24 (P-009).
The first `paperclipai run` attempt had only failed because onboarding had not been done.

## P-018 · 2026-10-29 · claude · result
Dry-run import of this package passed on a real Paperclip instance: 6 agents, 4 projects,
6 tasks (5 recognized as recurring routines), 5 skills. Applied with `company import --yes`.
Verified through the API afterwards: models, roles, budgets ($135 total) and all five cron
schedules matched `.paperclip.yaml`; one backlog issue (PAP-1 First-run setup) was created.
- Finding: imported agents start **paused**. Routines are active but cannot run until an
  agent is resumed, so a fresh import spends nothing and contacts no one. Documented in README.
- Finding: non-interactive imports require `--yes`. Documented in README.
- Not yet tested: import via the GitHub shorthand (needs the repo to be public; see P-019).
- Not done on purpose: resuming agents. That spends the owner's Claude usage and needs real
  `MARKET`/`STATE`/`BROKERAGE_NAME`/`AGENT_NAME` values, which only the owner can supply.

## P-019 · 2026-10-29 · claude · result
Published https://github.com/dylanjlennon/paperclip-for-realestate (public, MIT) after a
secrets/personal-path scan came back clean. Then tested the stranger's install path:
`paperclipai company import dylanjlennon/paperclip-for-realestate --dry-run` resolved the
GitHub repo and produced the identical plan (6 agents, 4 projects, 6 tasks, 5 skills).
- Status at this checkpoint: package built, installed locally, installable from GitHub.
  Not yet done: real agent runs on live data (needs owner inputs and approval to spend).

## P-020 · 2026-10-29 · human · decision
Run the agents on the owner's Claude Code subscription, not an API key. Verified before the
first run: no ANTHROPIC_API_KEY / ANTHROPIC_AUTH_TOKEN / CLAUDE_CODE_OAUTH_TOKEN in the shell,
the Paperclip server process or its config, and `claude auth status` reports `authMethod: claude.ai`.

## P-021 · 2026-10-29 · claude · result
First agent run failed: `API Error: 400 Claude Code 2.1.257 does not support this model;
version 2.1.280 or newer is required`. Cause: Paperclip's default ACP engine bundles its own
Claude Code (Agent SDK 0.3.257 -> 2.1.257), separate from the owner's installed `claude`
(2.1.312). `claude update` reported "already up to date", so updating the CLI could not help.
- Fix: set `engine: "cli"` and `command: "claude"` on the agent, so Paperclip runs the installed
  CLI with the owner's subscription login. Applied locally to the Chief of Staff.
- Side effect observed: Paperclip scheduled automatic retries of the failed run. Paused the
  agent to stop the loop before changing config. The failed requests were rejected with a 400,
  so no model usage was consumed.
- Lesson for the article: "just update Claude Code" was the obvious fix and was wrong; the
  error message pointed at a version that lived in a different place than the one being updated.

## P-022 · 2026-10-29 · claude · result
First successful run: Chief of Staff on PAP-1 (First-run setup), status `succeeded`, exit 0,
reported cost $0.86 (API-equivalent figure; billed against the subscription, not an API key).
The agent posted the owner interview as a question card on PAP-1 for the human to answer
(market inputs, data sources, lead sources, brokerage disclosure rules, call times).
- It could not move PAP-1 out of `blocked` or post a status comment. Not yet diagnosed; likely a
  permission scope for the agent. To investigate.
- Governance check: the agent stopped and asked the human instead of guessing. This is the
  behavior `approval-gate` is designed to produce.
- Other five agents remain paused on purpose to limit subscription usage.

## P-023 · 2026-10-29 · claude · customization
Changed the `lead-triage` routine from every 15 minutes to hourly (`0 8-20 * * *`) in the
template. Proposed to the owner before the run and not objected to.
- Why: on a subscription, usage limits are the real ceiling; 15-minute polling is wasteful
  before real leads exist. Owners with high lead volume can tighten it.
- Artifacts: `.paperclip.yaml`
