# CiPHA Markets — architecture (public overview)

High-level system design for partners and investors. No implementation details, credentials, or deployment config.

---

## Design principles

| Principle | Meaning |
|-----------|---------|
| **Non-custodial** | Users connect their own MT5 or Deriv accounts. CiPHA routes orders; it never holds capital. |
| **Glass-box** | Every council message, poll, abstain, and validator result is stored and auditable. |
| **Models propose, code decides** | LLMs suggest trade plans. TypeScript guardrails + validator approve or reject. |
| **Abstain is valid** | No order and no partner signal when rules fail, spread is wide, or consensus is weak. |
| **Agentic infrastructure** | Crons, watchers, and snapshots run 24/7. LLMs wake on schedule and events — not infinite chart-staring. |

---

## System diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                         User (web / mobile)                      │
│   Terminal · Trade · History · Settings · Connect broker        │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│                      Application layer                           │
│   Auth · entitlements · paywall · partner portal · admin        │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Council & intelligence                        │
│                                                                  │
│   Context load ──► 10+ personas (playbook + model each)         │
│        │              StyleScout → Structure → Macro →           │
│        │              Bull → Bear → StyleLens → Scalp →          │
│        │              Critic → ProperRisk → Integrator           │
│        │                         │                               │
│        │                         ▼                               │
│        │                   Consensus poll                        │
│        │                         │                               │
│        ▼                         ▼                               │
│   Memory systems          Trade plan or ABSTAIN                  │
│   (learnings, book, journal, pulse, transcript)                  │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│                      Safety & validation                         │
│   Guardrails (lots, SL, spread, symbols, daily loss, sessions)  │
│   Validator (TypeScript) — approve or reject before broker       │
└────────────────────────────┬────────────────────────────────────┘
                             │
              ┌──────────────┴──────────────┐
              ▼                             ▼
┌──────────────────────────┐   ┌──────────────────────────┐
│   User broker execution   │   │   Partner signal fan-out  │
│   MT5 (MetaApi)           │   │   (B2B forward test)      │
│   Deriv (OAuth)           │   │   Same abstain discipline   │
└──────────────────────────┘   └──────────────────────────┘
```

---

## The four product surfaces

### 1. The Desk (`/app`)
Core AI trading terminal for paying desk users.

| Screen | Purpose |
|--------|---------|
| Terminal | Chat with council — wake, debate, trade plans |
| Trade | Live balance, positions, broker status |
| History | Past council sessions + outcomes |
| Settings | Guardrails, signal book, trails, consensus |
| Connect | MT5 (MetaApi) or Deriv OAuth |

### 2. Retail Signals (`/signals`)
Ghana go-to-market funnel: landing, pricing, MoMo/USDT checkout, FAQ.

### 3. Partner portal (`/signals/partners`)
Affiliate links, referrals, payouts — distribution in West Africa.

### 4. Admin (`/r1`)
Founder ops: payments, users, partner deliveries, proof ledger.

---

## Council flow (forex / metals / crypto CFDs)

1. **Load context** — prices, levels, user rules, open trades, past learnings, pulse state
2. **Run personas** — each uses a different model + strategy playbook
3. **Consensus poll** — buy / sell / abstain; must beat user threshold
4. **Integrator** — outputs structured trade plan or abstain
5. **Validator** — code checks lots, spread, SL, daily loss, symbol allowlist
6. **Execute or fan-out** — user authorizes broker order, or partner receives signal

Council can run in **two hops** (debate + finalize) for reliability on long deliberations.

---

## Memory & context

| System | What it remembers |
|--------|-------------------|
| Session learnings | Prior abstain reasons, spread notes, setup context |
| Position book | Open legs — hold vs new setup vs conflict |
| Trade journal | Closed sessions, PnL rollups |
| Pulse state | Desk mood, streaks, event acknowledgements |
| Council transcript | Full thread — the glass box |

Multi-timeframe stack: **D1/H4/H1 bias → H1/M15 setup → M5 trigger → risk**. Not random scalps against higher timeframes.

---

## Execution paths

### Path A — User desk (MT5)
Council → validator → MetaApi → user's MT5 account.

### Path B — User desk (Deriv)
Reduced council (5 agents) → validator → Deriv OAuth → binaries execute.

### Path C — Partner B2B
Council-only lane (no founder execute) → validator → partner API with compounding signal book for lot sizing.

All paths share the same abstain discipline.

---

## Autonomous lanes

| Lane | Role |
|------|------|
| **Scheduled autonomy** | Cron wakes council on user-defined schedule; execution capped |
| **Watcher** | Price alarms resume saved council plans |
| **Whiskey** | Fast position-management lane — flat-only discipline |
| **Partner cron** | Crypto council on schedule → partner forward test |

Autonomy never bypasses guardrails.

---

## Payments & entitlements

- **Desk:** run packs, premium tiers, phone OTP login
- **Signals:** MoMo (Moolre) + USDT (NOWPayments)
- **Partners:** referral tracking, payout dashboard

---

## What is not in this repo

This public repository contains documentation only. The private implementation includes:

- Full TypeScript backend (edge functions, shared modules)
- Database schema and migrations
- Frontend source (React)
- Deployment configuration
- Internal runbooks

Shared on request during live product walkthroughs.

---

## Related links

- [README](./README.md) — product overview
- [JOURNEY.md](./JOURNEY.md) — build timeline
- [cipha.app](https://cipha.app) — website
- [ciphamarkets.vercel.app](https://ciphamarkets.vercel.app) — live desk
