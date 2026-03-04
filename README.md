# Alcor

### The Invisible Data Settlement Layer.

![Status](https://img.shields.io/badge/STATUS-EXPERIMENTAL-red?style=for-the-badge)
![Encryption](https://img.shields.io/badge/ENCRYPTION-ASYMMETRIC%20%2F%20ZKML-blue?style=for-the-badge)
![Chain](https://img.shields.io/badge/CHAIN-SOLANA-purple?style=for-the-badge)
![Architect](https://img.shields.io/badge/BUILDER-ASTROPHEL-orange?style=for-the-badge)

> **⚠️ WARNING: SIGNAL DETECTED**
> This is a raw artifact from the R&D lab of Astrophel. The code is aggressive, sovereign, and encrypted. It is the "Hidden Star" of the data economy.

---

## ⭐️ The Mission (Alcor)

In the constellation Ursa Major, **Alcor** is the faint star hidden behind Mizar. Ancient armies used it as an eyesight test. If you could see Alcor, you were elite.

The global AI economy has an "Alcor" too: **African Language Data.**

Hausa. Swahili. Yoruba. Twi. Billions of voices training zero models.

The AI industry is starving for diverse language data — yet millions of African language speakers are completely excluded from both contributing to and benefiting from AI development. Existing datasets are overwhelmingly English-biased, making AI models perform poorly for African users.

**ALCOR** brings this data to light without burning the source.

It is a decentralized marketplace where local users get paid **instantly** to contribute verified voice data in their native languages — verified on-chain, encrypted end-to-end, and sold directly to AI companies.

---

## 🌌 The Physics (Tech Stack)

Alcor operates on a sovereign, Rust-native stack:

| Layer | Technology | Function |
| :--- | :--- | :--- |
| **The Interface** | `Leptos` (Rust/WASM) | PWA frontend — fully Rust native |
| **The Gravity** | `Axum` (Rust) | Backend API layer |
| **The Contract** | `Solana` (Rust) | Instant on-chain payments + CID storage |
| **The Void** | `IPFS` / `Storacha` | Decentralized storage for encrypted voice payloads |
| **The Observer** | `EZKL` (zkML) | Lightweight ZK model — liveness check + language verification |
| **The Shield** | Asymmetric Encryption | AI company public key encrypts — only they can decrypt |

---

## 🔬 How It Works

```
Local User speaks → Voice captured
        ↓
zkML verifies: Human? ✅ Correct Language? ✅ Accurate Content? ✅
        ↓
Voice encrypted asymmetrically with AI company's public key
        ↓
Encrypted payload uploaded to IPFS → CID hash stored on Solana
        ↓
Smart contract releases payment instantly to contributor
        ↓
Alcor delivers clean API / JSON to AI company in USD
```

**Two sides. One protocol.**
- **Public layer** — Fully decentralized. Contributors, verification, payments.
- **Private layer** — Alcor bridges to AI companies. USD settlements. No crypto required on their end.

---

## 🔐 Encryption Model

Voice data is encrypted **asymmetrically** using the purchasing AI company's public key before it ever leaves the contributor's device. Only the AI company holding the corresponding private key can decrypt the data. Not Alcor. Not the chain. No one else.

This is trustless by design.

---

## 🧠 zkML Verification

Using **EZKL**, Alcor runs a lightweight ZK classifier that proves:
1. The voice is from a **real human** (liveness detection)
2. The voice is in the **correct language** (language classification)
3. The content is **topically accurate** (keyword verification against node requirements)

All three proofs are generated on-device. No heavy computation. No centralized oracle.

---

## 🚀 Simulation (Kernel)

*Currently in pre-alpha kernel mode.*

```bash
# Clone the Star
git clone https://github.com/astrophelverse/alcor-protocol.git

# Enter the Gravity Well
cd alcor

# Run the Rust backend
cargo run

# Deploy Solana program
anchor build && anchor deploy
```

---

## 📁 Architecture

```
alcor/
├── contracts/          # Solana programs (Rust/Anchor)
│   └── alcor_market.rs
├── backend/            # Axum API server
│   └── src/
├── frontend/           # Leptos PWA
│   └── src/
├── zkml/               # EZKL voice classifier
│   └── model/
└── scripts/            # TypeScript glue + deployment
```

---

## 🌍 Why This Matters

- African language AI data is a **multi-billion dollar gap**
- Google, Meta, Microsoft are actively seeking this data
- Contributors earn in a trustless, instant, on-chain system
- No middlemen. No delayed payments. No data theft.

---

*Built by Astrophel. Veritas in Umbra — truth in the shadows.*
