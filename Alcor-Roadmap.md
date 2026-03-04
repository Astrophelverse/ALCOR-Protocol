# ALCOR — Full Build Roadmap
### Decentralized Voice Data Marketplace for African Languages
*Built by Astrophel. Veritas in Umbra.*

---

## 🗺️ Overview

| Phase | Name | Duration | Goal |
| :--- | :--- | :--- | :--- |
| **0** | Foundation | Week 1 | Repo setup, tooling, environment |
| **1** | The Contract | Week 2-3 | Solana payment smart contract |
| **2** | The Vault | Week 4-5 | Encryption pipeline + IPFS storage |
| **3** | The Observer | Week 6-7 | zkML voice verification |
| **4** | The Bridge | Week 8-9 | Axum backend API |
| **5** | The Face | Week 10-11 | Leptos PWA frontend |
| **6** | The Star Goes Live | Week 12 | MVP launch + first dataset |

---

## ⚙️ Phase 0 — Foundation (Week 1)

> Goal: Everything set up, nothing broken, repo is clean and public.

### Tasks:
- [ ] Initialize monorepo structure
- [ ] Set up Rust toolchain (`rustup`, `cargo`)
- [ ] Install Anchor framework for Solana
- [ ] Install Leptos + Trunk for PWA
- [ ] Set up Axum backend project
- [ ] Configure Solana CLI + Devnet wallet
- [ ] Set up IPFS/Storacha account
- [ ] Set up EZKL for zkML
- [ ] Push skeleton repo to GitHub (public)
- [ ] Write initial README (done ✅)

### Folder Structure:
```
alcor/
├── contracts/          # Solana programs (Anchor/Rust)
├── backend/            # Axum API server (Rust)
├── frontend/           # Leptos PWA (Rust/WASM)
├── zkml/               # EZKL voice classifier (Python)
├── scripts/            # TypeScript glue + deployment
├── tests/              # Foundry + integration tests
└── docs/               # Architecture docs
```

### Commit message:
`feat: initialize alcor monorepo — the star awakens`

---

## 🔗 Phase 1 — The Contract (Week 2-3)

> Goal: Solana smart contract that holds bounties, verifies submissions and pays contributors instantly.

### Stack:
- **Rust + Anchor** for Solana program
- **Solana Devnet** for testing
- **Foundry-style tests** via Anchor's test framework

### What to build:
- [ ] `AlcorMarket` program — main contract
- [ ] `post_bounty` instruction — AI company posts a data bounty with USDC/SOL
- [ ] `submit_voice` instruction — contributor submits IPFS CID hash
- [ ] `verify_and_pay` instruction — verifies zkML proof and releases payment instantly
- [ ] `close_bounty` instruction — AI company closes bounty and reclaims unused funds
- [ ] Account structures:
  - `BountyAccount` — stores bounty details, language, reward per sample
  - `SubmissionAccount` — stores contributor wallet, CID hash, verification status
- [ ] Write full test suite (happy path + edge cases)
- [ ] Deploy to Devnet
- [ ] Push to GitHub

### Key logic:
```
AI Company deposits funds → BountyAccount created
Contributor submits CID + zkML proof → SubmissionAccount created
Contract verifies proof on-chain → Payment released instantly
```

### Commit messages:
- `feat(contracts): add AlcorMarket program skeleton`
- `feat(contracts): implement post_bounty instruction`
- `feat(contracts): implement verify_and_pay with instant settlement`
- `test(contracts): full test suite passing on devnet`

---

## 🔐 Phase 2 — The Vault (Week 4-5)

> Goal: Voice data encrypted before it ever leaves the device. Only AI company can decrypt.

### Stack:
- **Rust** for encryption logic
- **RSA / ECIES** asymmetric encryption
- **IPFS + Storacha** for decentralized storage
- **Python** for encryption key management scripts

### What to build:
- [ ] Asymmetric encryption module in Rust
  - AI company uploads their public key to Alcor
  - Voice payload encrypted with their public key on-device
  - Only their private key can decrypt — not Alcor, not the chain
- [ ] IPFS upload pipeline
  - Encrypted voice payload → uploaded to IPFS via Storacha
  - Returns CID hash
- [ ] CID storage on Solana
  - CID hash submitted to `SubmissionAccount` on-chain
  - Costs fractions of a cent per submission
- [ ] Key registry — smart contract stores AI company public keys on-chain
- [ ] Test full encrypt → upload → CID → store flow
- [ ] Push to GitHub

### Flow:
```
Voice recorded → Encrypted (AI company public key)
→ Uploaded to IPFS → CID returned
→ CID stored on Solana → Submission complete
→ AI company uses private key to decrypt later
```

### Commit messages:
- `feat(vault): asymmetric encryption module`
- `feat(vault): IPFS upload pipeline via Storacha`
- `feat(vault): CID hash storage on Solana`
- `test(vault): full encryption roundtrip verified`

---

## 🧠 Phase 3 — The Observer (Week 6-7)

> Goal: Lightweight zkML classifier that proves voice is human, correct language, and accurate content.

### Stack:
- **EZKL** for ZK proof generation
- **Python** for model training + export
- **ONNX** for model format
- **Rust** for proof verification integration

### What to build:
- [ ] Train small voice classifier in Python
  - Input: voice audio
  - Output 1: is_human (liveness detection)
  - Output 2: language_label (Hausa / Swahili / Yoruba etc.)
  - Output 3: content_accurate (keyword match for bounty node)
- [ ] Export model to ONNX format
- [ ] Convert ONNX model to EZKL circuit
- [ ] Generate ZK proof on-device
  - Proof says: "This voice passed all 3 checks" without revealing the voice
- [ ] Verify proof in Solana smart contract (Phase 1 integration)
- [ ] Test on real voice samples (collect 10-20 manually)
- [ ] Push to GitHub

### Why EZKL works here:
- Small classifier = tiny circuit = fast proof generation
- Not running a full LLM through ZK — just a small binary classifier
- Proof generation happens on contributor device, not server

### Commit messages:
- `feat(zkml): voice classifier model training`
- `feat(zkml): ONNX export + EZKL circuit generation`
- `feat(zkml): on-device proof generation pipeline`
- `feat(contracts): integrate zkML proof verification`

---

## 🌉 Phase 4 — The Bridge (Week 8-9)

> Goal: Axum backend that bridges the decentralized public layer with AI companies who want clean APIs.

### Stack:
- **Axum** (Rust) for REST API
- **TypeScript** scripts for AI company integration
- **JSON** export for data delivery

### What to build:
- [ ] Axum server setup
- [ ] Private API endpoints (authenticated):
  - `POST /bounty` — AI company creates bounty
  - `GET /bounty/:id/submissions` — list verified submissions
  - `GET /submission/:id/data` — retrieve encrypted payload from IPFS
  - `POST /bounty/:id/deliver` — package verified data as JSON/API for delivery
- [ ] Public API endpoints:
  - `GET /bounties` — list active bounties
  - `POST /submit` — contributor submits voice + CID
- [ ] USD payment integration for AI companies (Stripe or wire)
- [ ] Data delivery pipeline:
  - Alcor pulls encrypted CIDs from chain
  - Fetches payloads from IPFS
  - Packages as clean JSON
  - Delivers to AI company
- [ ] Rate limiting + API key auth for AI companies
- [ ] Push to GitHub

### The two-layer model:
```
PUBLIC (Decentralized):
Contributors → zkML → IPFS → Solana → Instant crypto payment

PRIVATE (Centralized):
AI Companies → USD payment → Alcor API → Clean JSON data delivery
```

### Commit messages:
- `feat(backend): Axum server skeleton`
- `feat(backend): public bounty + submission endpoints`
- `feat(backend): private AI company delivery pipeline`
- `feat(backend): USD payment integration`

---

## 🌐 Phase 5 — The Face (Week 10-11)

> Goal: Rust-native PWA where local African users can connect wallet, record voice, and get paid.

### Stack:
- **Leptos** (Rust/WASM) for frontend
- **Trunk** for bundling
- **Solana Wallet Adapter** for wallet connection
- **Web Audio API** for voice recording

### What to build:
- [ ] Leptos PWA setup with Trunk
- [ ] PWA manifest + service worker (works offline)
- [ ] Pages:
  - `/` — Landing page, active bounties list
  - `/bounty/:id` — Bounty detail, record voice here
  - `/record` — Voice recorder with live feedback
  - `/wallet` — Connect wallet, view earnings, history
  - `/verify` — zkML verification status
- [ ] Voice recording component
  - Record audio in browser
  - Show waveform feedback
  - Trigger zkML verification
  - Encrypt + upload to IPFS
  - Submit CID to Solana contract
  - Show payment confirmation
- [ ] Wallet connection (Phantom, Solflare)
- [ ] Mobile responsive (most African users on mobile)
- [ ] Push to GitHub

### Commit messages:
- `feat(frontend): Leptos PWA skeleton + Trunk config`
- `feat(frontend): bounty listing + detail pages`
- `feat(frontend): voice recorder component`
- `feat(frontend): wallet connection + payment confirmation`
- `feat(frontend): mobile responsive PWA`

---

## 🚀 Phase 6 — The Star Goes Live (Week 12)

> Goal: Full pipeline live. First real dataset collected. Grant milestone delivered.

### Tasks:
- [ ] Deploy Solana contract to Mainnet
- [ ] Deploy Axum backend to server (Railway / Fly.io)
- [ ] Deploy Leptos PWA to Vercel / Cloudflare Pages
- [ ] Collect first 500 real voice samples (Hausa, Swahili, Yoruba)
- [ ] Verify full pipeline end to end with real users
- [ ] Package first dataset for AI company delivery
- [ ] Record demo video
- [ ] Write grant milestone report
- [ ] Apply to Solana Foundation main grant with working MVP
- [ ] Apply to Filecoin/IPFS grant
- [ ] Apply to Optimism RetroPGF

---

## 🏆 Grant Timeline

| Grant | Apply When | Expected |
| :--- | :--- | :--- |
| **Superteam Instagrant** | Applied ✅ | 72hr decision |
| **Solana Foundation** | After Phase 3 (Week 7) | 2-4 weeks |
| **Filecoin Ecosystem Fund** | After Phase 2 (Week 5) | 2-4 weeks |
| **Optimism RetroPGF** | After MVP (Week 12) | Retroactive |

---

## 📊 KPIs

| Metric | Target | How Verified |
| :--- | :--- | :--- |
| Verified voice samples | 500 | On-chain submission count |
| Languages covered | 3 minimum | Bounty node labels |
| Automatic on-chain payments | 100+ | Solana transaction history |
| Full bounty delivered | 1 | AI company receipt |
| zkML proof failures | 0 | Contract rejection logs |

---

## 🛠️ Full Tech Stack Reference

| Layer | Technology | Language |
| :--- | :--- | :--- |
| Frontend | Leptos + Trunk | Rust/WASM |
| Backend | Axum | Rust |
| Smart Contract | Anchor + Solana | Rust |
| Storage | IPFS + Storacha | — |
| ZK Verification | EZKL | Python + Rust |
| Encryption | RSA/ECIES asymmetric | Rust |
| Glue Scripts | TypeScript | TypeScript |
| AI Model | ONNX classifier | Python |
| Testing | Anchor tests + Foundry | Rust |

---

## 📅 Week by Week

| Week | Focus | GitHub milestone |
| :--- | :--- | :--- |
| 1 | Repo setup, tooling | Skeleton pushed |
| 2 | Solana contract pt.1 | `post_bounty` working |
| 3 | Solana contract pt.2 | `verify_and_pay` working |
| 4 | Encryption module | Encrypt/decrypt roundtrip |
| 5 | IPFS pipeline | CID storage on Solana |
| 6 | zkML model training | Classifier working |
| 7 | zkML ZK proofs | Proof generation live |
| 8 | Axum backend pt.1 | Public endpoints live |
| 9 | Axum backend pt.2 | Private delivery pipeline |
| 10 | Leptos frontend pt.1 | PWA shell + bounty list |
| 11 | Leptos frontend pt.2 | Voice recorder + payments |
| 12 | Launch + dataset | 500 samples collected |

---

*Built by Astrophel. Brick by brick. Star by star. 🌟*
*Veritas in Umbra — truth in the shadows.*

