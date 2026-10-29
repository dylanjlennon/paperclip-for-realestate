# Paperclip for Real Estate

A ready-to-run AI operations team for a licensed real estate agent or small brokerage, packaged
for [Paperclip](https://github.com/paperclipai/paperclip) using the open
[Agent Companies](https://github.com/paperclipai/paperclip/blob/master/docs/companies/companies-spec.md) format.

**You stay the licensed professional. The agents do the prep work.** Every message to a client,
every public post, and every contract-related action is a draft until you approve it.

## What you get

| Agent | Job |
|---|---|
| **Chief of Staff** | Plans the week, writes your Monday brief, keeps your approval queue short |
| **Lead Concierge** | Scores inbound leads, drafts first replies within minutes and follow-ups on days 1/3/7/14 |
| **Market Analyst** | Weekly sourced market snapshot for your farm area |
| **Content Editor** | Weekly hyperlocal newsletter, social posts, listing descriptions |
| **Transaction Coordinator** | Contract deadline timeline, daily digest, status-update drafts |
| **Compliance Officer** | Reviews every outbound draft for Fair Housing, advertising and consent issues |

Plus 5 skills (`lead-qualification`, `market-report`, `transaction-timeline`,
`fair-housing-copy`, `approval-gate`), 4 starter projects, and 5 scheduled routines.
Monthly budgets default to $135 total across agents and are easy to change.

## Install

You need Node.js 24.11+, about 1 GB of free disk for the install, and a running Paperclip instance
(`npx paperclipai@latest onboard --yes`; see the Paperclip docs).

```bash
# Preview first (no writes)
npx paperclipai company import dylanjlennon/paperclip-for-realestate \
  --target new --new-company-name "My Real Estate Team" --dry-run

# Then apply (--yes is required in a non-interactive shell)
npx paperclipai company import dylanjlennon/paperclip-for-realestate \
  --target new --new-company-name "My Real Estate Team" --yes
```

**Agents are imported paused.** Nothing runs, spends money, or contacts anyone until you resume
an agent in the Paperclip UI. Start with the Chief of Staff and the **First-run setup** task.

When asked for inputs, set `MARKET`, `STATE`, `BROKERAGE_NAME` and `AGENT_NAME`. Then run the
**First-run setup** task and answer the Chief of Staff's questions.

## Status

v0.1.0. Verified: the package passes Paperclip's importer and every agent, budget, model and
schedule lands as written (see provenance P-018). Not yet verified: real agent runs against live
lead and market data. Treat the prompts and the lead-scoring weights as a starting point to tune.

## Make it yours

- Edit any `agents/*/AGENTS.md` or `skills/*/SKILL.md`. They are plain markdown.
- Change budgets, models and schedules in `.paperclip.yaml`. Schedules default to `America/New_York`.
- Connect your data (MLS/IDX export, county records, GIS) and lead sources in Paperclip.

## Provenance

Every decision and customization behind this package is recorded in
[`provenance/LOG.md`](provenance/LOG.md), and each file carries `metadata.provenance` IDs that
point back to it. Fork it, and add your own entries.

## Important

- **Not legal advice.** The compliance skills are drafting aids. You and your managing broker
  remain responsible for Fair Housing, advertising, licensing, TCPA and CAN-SPAM compliance in your state.
- **Licensing.** Only a licensed broker can operate a brokerage. This package is an operations layer, not a licensed entity.
- **Human approval is the point.** Don't remove the `approval-gate` for client-facing actions.
- Paperclip is a separate MIT-licensed project and is not affiliated with this repo.

## License

MIT
