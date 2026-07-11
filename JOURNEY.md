# CiPHA Markets — journey

**Public timeline** · Last updated July 2026  
**Tone:** honest build log for partners and investors — no internal infra details.

---

## Before Markets: CiPHA 1.0

The founder was copy-pasting between ChatGPT, Claude, Gemini, and others — same context, different rooms, human synthesis in the middle.

**CiPHA 1.0** fixed that: one room, multiple models, @mentions, polls, human-first orchestration. Landing at [cipha.vercel.app](https://cipha.vercel.app). Premium deliberation product; credits model; no broker execution.

The **Markets epiphany:** the same multi-agent debate pattern that works for hard questions should work for risk capital — but with a code gatekeeper, real PnL, and orders that only hit **your** broker.

Key principles locked early:

- **Non-custodial** — we never hold funds
- **Validator before broker** — models propose; code approves
- **Agentic (our definition)** — infrastructure watches 24/7; LLMs wake on events and rules
- **Separate product** from CiPHA 1.0 — same design language, different data and pricing

---

## May 2026 — prove the spine first

We did not start with polished UI. We started with:

- Docs and environment scaffolding
- A clear split from CiPHA 1.0
- Planning for council roster, broker research, build phases

**Why:** if council → validator → queue → broker does not work, nothing else matters.

### First real money path (forex / MT5)

| Milestone | What shipped |
|-----------|--------------|
| Bridge scaffold | Python worker, MT5 execution, snapshot push |
| First MT5 order | Demo ticket confirmed; lot normalization |
| Edge pipeline | Regime gate → validate proposal → place order |
| Wake-board | Headless council wake (R0) deployed |
| Live wake trade | Signal → ticket confirmed end-to-end |
| Terminal UI v0 | Vite/React desk, session sidebar, "Call board" |
| Core pipeline | Guardrails DB, validator v2, R1 council, market gate |

**Council path (forex):** Structure → Macro → Bull/Bear debate → Poll → Integrator → Validator → execution queue → broker.

**R0 vs R1:** R0 = solo model, fast. R1 = full council — the product thesis. Execute only when user asks and validator passes.

---

## Late May 2026 — auth, mobile, broker connect

- Supabase auth, per-user row-level security
- Encrypted MT5 credential storage, Connect page
- Mobile bottom nav: Terminal | Trade | History
- Persona avatars, council typing indicator, abstain chips
- Multi-tenant signal routing (Phase 1)

**Why mobile-first shell:** the desk is used on phone; chart-first MT5 layout was not the target experience.

---

## Late May – early June 2026 — scale and rules

- Multi-user bridge orchestrator (auto MT5 slots per user)
- Pip/lot math fixes (gold ≠ forex pip math)
- Rules Coach — chat to set guardrails instead of editing JSON
- Preflight re-wake — if price moves between council and queue, one fresh pass before abort
- Share cards + history floor

**Pain points documented:** IPC auth, Algo Trading flags, stale ticks — all runbook'd and iteratively fixed.

---

## June 2026 — Deriv, conversational desk, council waits

- Deriv OAuth 2.0 PKCE — binaries/synthetics without MT5 for those symbols
- Conversational desk — user talks to desk like CiPHA 1.0; pair picker cards; authorize-before-council flow
- Council **wait** — saves plan, resumes on level hit (not a full re-run of 10 LLMs)
- Multi-user VPS bridge maturity; orchestrator self-heal

---

## Late June – July 2026 — MetaApi, retail, reliability

The legacy VPS bridge era largely ends. **MetaApi** becomes the primary MT5 path; council reliability and retail tiers mature.

| Milestone | What shipped |
|-----------|--------------|
| MetaApi | Cloud terminals 24/7; limit-order fixes |
| Council split | Two edge hops (debate + finalize) to avoid timeout |
| Retail autonomy | Scheduled wakes + execution cap |
| Premium desk | Login-only after MoMo pay; retail tier UI |
| Agent Whiskey | Fast autonomous lane — position manager, flat-only discipline |
| Desktop shell | CiPHA 1.0 sidebar layout on wide screens |
| Council UX | Typing indicators, warming, cancel-on-decline, multi-TF stack |

---

## July 2026 — B2B partner forward test

Cold outreach → demo call → live API integration → forward test under QuantNexusCapital legal entity.

### Why this partner

Institutional quant background — not buying Telegram signals; buying **glass-box council output** with open/close lifecycle, abstain discipline, and **master_balance + lotsize ratio** for post-signal scaling.

### Integration milestones

| When | What |
|------|------|
| Early Jul | Demo call; BTC proof screenshots |
| Mid Jul | Partner API spec — open/close same trade ID, confidence + threshold |
| Mid Jul | Legal route → `signals.quantnexuscapital.com` |
| Mid Jul | Partner fan-out; position book memory; algorithmic partner closes |
| Mid Jul | Compounding signal book — book × risk% ÷ SL; book compounds on close |
| Mid Jul | Partner sizing decoupled from desk guardrail floors |

### How the partner lane runs

- **Crypto cron:** scheduled BTC/ETH council wakes
- **Council-only:** signals to partner endpoint — no founder MT5 execute on this lane
- **Fan-out blocks:** abstain, hold, poll fail, position-book conflict
- **Payload:** trade ID, asset, direction, lot size, price, master balance, confidence, style, close reason
- **Reference book:** compounding master balance for B2B ratio math

Live forward test: [signals.quantnexuscapital.com](https://signals.quantnexuscapital.com)

---

## What works today (July 2026)

- Phone OTP desk login, paywall, entitlements, premium activation
- Forex: full council → validator → MT5 via MetaApi
- Binaries: 5-agent council → Deriv OAuth execute
- Authorize card → full council in background
- Council wait → saves plan → resumes on level hit
- History, journal, share cards, Rules Coach, desktop shell
- Admin proof ledger + partner delivery audit log
- Signals / Partners / MoMo / USDT checkout
- Autonomy + watcher cron + crypto partner cron
- Partner signal book — compounding master balance for B2B sizing

---

## What we learned the hard way

1. **OAuth client id ≠ WebSocket app id** — always verify broker API config explicitly
2. **Council wait is a decision, not a reason to re-run 10 LLMs** — alarm hit should resume the saved plan
3. **Wait direction must use price vs level** — pullback vs breakout semantics matter
4. **Integrator JSON will truncate** — always need poll/stake fallback on binaries
5. **Snapshot workers must loop** — stale ticks break council preflight
6. **Browser tab is not a job queue** — background council runs + cron for long hops
7. **Multi-user bridge is ops** — slots, heartbeats, broker flags — not just code
8. **Partner signals need the same abstain discipline as desk** — no fan-out on hold or poll fail

---

## Mental model: two products, one founder

```
CiPHA 1.0                    CiPHA Markets
─────────────                ─────────────────
Deliberation chat     →      Trading desk (same debate pattern)
Credits / early access →     Desk runs paywall
No execution          →      MT5 forex + Deriv binaries
cipha.vercel.app      →      ciphamarkets.vercel.app
```

**Revenue lanes in Markets:**

1. **Desk** — council runs, autonomy, serious traders
2. **Signals** — MoMo/USDT funnel, affiliates
3. **Partners** — referrals, payouts, B2B API
4. **Institutional forward test** — QuantNexusCapital lane

---

## Contact

**Sylvester Dapaah** — dapaahsylvester5@gmail.com  
[github.com/Remmy1-AI](https://github.com/Remmy1-AI)
