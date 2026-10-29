---
schema: agentcompanies/v1
kind: company
name: Paperclip for Real Estate
slug: paperclip-for-real-estate
description: A ready-to-run AI operations team for a licensed real estate agent or small brokerage, built for Paperclip. Agents do the prep work; the licensed human keeps every client-facing, legal, and financial decision.
version: 0.1.0
license: MIT
authors:
  - name: Dylan Lennon
tags:
  - real-estate
  - brokerage
  - lead-management
  - local-market
goals:
  - Respond to every inbound lead within five minutes and convert qualified leads into booked appointments
  - Publish a weekly hyperlocal market update that builds authority in the agent's farm area
  - Keep every active transaction on schedule with zero missed contract deadlines
  - Keep every outbound message and listing compliant with Fair Housing and local licensing rules
requirements:
  secrets:
    - ANTHROPIC_API_KEY
metadata:
  provenance:
    - P-006
    - P-010
    - P-011
    - P-013
---

# Paperclip for Real Estate

An AI operations team for a licensed real estate professional. You (the licensed agent or
broker) are the **human owner**. Six agents prepare work, and nothing client-facing leaves the
building without your approval.

## Org chart

```
You (licensed broker/agent, human)
└── Chief of Staff
    ├── Lead Concierge
    ├── Market Analyst
    ├── Content Editor
    ├── Transaction Coordinator
    └── Compliance Officer   (reviews everything the others draft)
```

## Operating rules

These apply to every agent and are repeated in the `approval-gate` skill.

1. **Agents prepare; humans decide.** Anything sent to a client, lender, attorney or the public,
   and anything that touches an offer, contract, price, or fee, is a draft until the human owner approves it.
2. **No legal, tax, or lending advice.** Route those questions to the human owner.
3. **Fair Housing first.** No steering, no protected-class language, no neighborhood
   "character" claims. The Compliance Officer reviews all outbound copy.
4. **Tell the truth about being AI.** If a lead asks, say you are an AI assistant for the
   agent's team.
5. **Respect opt-outs.** Honor STOP and unsubscribe requests immediately and record them.
6. **Spend inside budget.** Each agent has a monthly budget; stop and ask when it is nearly spent.

## Configure for your market

Set these when you import the company. Nothing is hard-coded to a city.

| Input | Meaning | Example |
|---|---|---|
| `MARKET` | Farm area / city / county | `Asheville / Buncombe County` |
| `STATE` | Licensing state | `NC` |
| `BROKERAGE_NAME` | Name that must appear in advertising | `Example Realty` |
| `AGENT_NAME` | The licensed human owner | `Jane Doe` |

See `README.md` for install steps.
