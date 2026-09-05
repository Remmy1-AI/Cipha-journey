# Cipha Markets

Cipha Markets puts AI agents on your trading account. Money stays at your broker. Models on the desk debate, code decides whether a trade is allowed.

**Live:** [cipha.app](https://cipha.app)

Not a broker. Not custodial. Not a signal group. Not [Cipha Sounds](https://en.wikipedia.org/wiki/Cipha_Sounds) the DJ.

---

## Live

| Resource | Link |
|----------|------|
| **Website** | [cipha.app](https://cipha.app) |
| **App** | [ciphamarkets.vercel.app](https://ciphamarkets.vercel.app) |
| **Cipha 1.0** (human + model rooms) | [cipha.vercel.app](https://cipha.vercel.app) |

---

## What it is

Users connect their own broker or prop account. A desk of models reads the session and debates the setup in the open. Code (guardrails + validator) decides whether a trade is allowed. Nothing executes unless the rules pass.

Cipha never holds user funds. It is infrastructure between the desk and the user's broker.

- **Glass-box desk** — models debate each setup; the transcript is stored and auditable
- **Code decides** — a TypeScript validator approves or rejects before any broker call
- **Non-custodial** — money stays at the broker; Cipha never withdraws
- **Abstain is a feature** — if the book says hold, there is no order
- **Proof-first** — session history and outcomes on the record

This is not a Telegram signal bot.

---

## Product

| Module | What it is |
|--------|------------|
| **Desk** ($79/mo) | Models debate, code decides, trades the connected broker or prop account |
| **Whiskey** ($49/mo) | Autonomous watcher on the tape |
| **Binaries** ($49/mo) | Deriv fixed-payout desk |
| **Arena** ($39/mo) | Live floor — watch desks work in real time |

Partner and affiliate portal is separate from the desk. The old retail signals funnel is not the product.

---

## Architecture (high level)

```
User (web / mobile)
       │
       ▼
  Glass-box terminal  ──►  Desk of models
       │                         │
       ▼                         ▼
  Guardrails + validator  ◄──  Plan or abstain
       │
       ▼
  User's broker (Connect: native APIs / Deriv / optional MetaApi)
```

See [ARCHITECTURE.md](./ARCHITECTURE.md) for more detail.

---

## Build

Solo-founded. Started 28 March 2026 (KNUST Cursor hackathon). Full execution spine live: desk → validator → broker. Implementation is in a private repo; this repository is public documentation only.

Full timeline: [JOURNEY.md](./JOURNEY.md)

---

## Founder

**Sylvester Dapaah** — Founder, Cipha Markets

- GitHub: [github.com/Remmy1-AI](https://github.com/Remmy1-AI)

---

## License

Documentation © 2026 Cipha Markets. All rights reserved. No code is published in this repository.
