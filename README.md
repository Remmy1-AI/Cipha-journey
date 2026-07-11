# CiPHA Markets

**A non-custodial, glass-box AI trading desk.**

Users connect their own broker accounts (MT5 for forex/metals/crypto CFDs, Deriv for binaries). A multi-model council deliberates in full public view — transcript, polls, abstains, validator results. Models **propose** trades; **code** (guardrails + validator) decides if an order is allowed. Nothing executes unless rules pass and the user authorizes.

CiPHA never holds user funds. It is infrastructure between **AI deliberation** and **the user's broker**.

---

## Live

| Resource | Link |
|----------|------|
| **Website** | [cipha.app](https://cipha.app) |
| **Trading desk** | [ciphamarkets.vercel.app](https://ciphamarkets.vercel.app) |
| **Partner forward test** | [signals.quantnexuscapital.com](https://signals.quantnexuscapital.com) |
| **CiPHA 1.0** (deliberation product) | [cipha.vercel.app](https://cipha.vercel.app) |

---

## What makes CiPHA different

Most AI trading products are black boxes: a signal fires, you follow it, you never see the reasoning. CiPHA is the opposite.

- **Glass-box council** — 10+ AI personas debate each setup; every message is stored and auditable
- **Code gatekeeper** — guardrails and a TypeScript validator approve or reject before any broker call
- **Non-custodial** — users keep their own MT5 / Deriv accounts; CiPHA routes orders, never holds capital
- **Abstain is a feature** — spread too wide, poll failed, book says hold → no order, no partner signal
- **Proof-first** — session history, outcome archives, partner delivery audit trail

This is not a Telegram signal bot. It is a **vertical trading operating system**.

---

## Product surface

### 1. The Desk
AI trading terminal: wake the council, debate setups, authorize execution, manage guardrails, view history.

### 2. Retail Signals (Ghana GTM)
Public funnel with MoMo and USDT checkout — distribution layer separate from the desk.

### 3. Partner & affiliate portal
Referral links, payouts dashboard, B2B signal API for institutional forward tests.

### 4. Autonomous lanes
Scheduled council wakes, watcher crons, and fast execution lanes — always bounded by user guardrails.

---

## Architecture (high level)

```
User (web / mobile)
       │
       ▼
  Glass-box terminal  ──►  Multi-model council (10+ personas)
       │                         │
       │                         ▼
       │                   Consensus poll
       │                         │
       ▼                         ▼
  Guardrails + validator  ◄──  Trade plan or abstain
       │
       ▼
  User's broker (MT5 via MetaApi / Deriv OAuth)
```

Optional B2B fan-out sends validated signals to partner endpoints — with the same abstain discipline.

See [ARCHITECTURE.md](./ARCHITECTURE.md) for more detail.

---

## Journey

Built from Ghana (KNUST) since May 2026:

- **May 2026** — Execution spine: council → validator → broker pipeline live
- **May–Jun 2026** — Auth, mobile shell, Deriv OAuth, rules coach, multi-tenant bridge → MetaApi migration
- **Jun–Jul 2026** — Retail autonomy, desktop shell, council reliability (two-hop architecture), Agent Whiskey lane
- **Jul 2026** — B2B partner forward test with QuantNexusCapital (crypto council → external signal book)

Full timeline: [JOURNEY.md](./JOURNEY.md)

---

## Scale (order of magnitude)

| Area | Approx. |
|------|---------|
| Backend APIs & crons | 37 deployed |
| Database migrations | 59 |
| Web pages | 37 |
| Strategy playbooks | 8 |
| Docs (internal) | 50+ |

Mid-size startup product — not a weekend script.

---

## Roadmap themes

- **Self-improving playbooks** — meta-strategist layer that learns from session outcomes while staying glass-box
- **Partner proof epochs** — compounding signal book, forward-test audit trail before capital conversations scale
- **Distribution** — Ghana signals funnel, affiliates, institutional API lane

---

## Repository note

**This repository is public documentation only** — product overview, journey, and architecture for partners and investors.

The full implementation (source code, database schema, deployment config) lives in a **private repository** and is shared on request during live product walkthroughs.

---

## Founder

**Sylvester Dapaah** — Founder & CEO, CiPHA Markets  
Kumasi, Ghana

- Email: dapaahsylvester5@gmail.com
- GitHub: [github.com/Remmy1-AI](https://github.com/Remmy1-AI)

---

## License

Documentation © 2026 CiPHA Markets. All rights reserved. No code is published in this repository.
