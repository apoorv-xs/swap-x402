# Swap.io x402 // 3D Kinetic Vault & Machine Finance Visualizer

> **Architect**: Apoorv A S ([@apoorv_xs](https://x.com/apoorv_xs) / [GitHub](https://github.com/apoorv-xs))  
> **Live Interactive WebUI**: [https://apoorv-xs.github.io/swap-x402/](https://apoorv-xs.github.io/swap-x402/)  
> **Bounty Track**: Superteam Earn x Swap.io x402 Video Challenge  

---

## 1. Architectural Overview

Autonomous AI agents cannot solve CAPTCHAs, enter 2FA codes, or carry traditional corporate credit cards. Storing long-lived static API keys in agent memory or configuration files creates a critical operational hazard.

**Swap.io resolves this via the HTTP 402 ("Payment Required") protocol on Solana L1**:
1. **Discovery & Challenge (`HTTP 402`)**: An unauthenticated agent requests aggregated swap liquidity (`POST https://hermes.swap.io/v1/swap/quote`). The Hermes gateway returns status code `402` containing an on-chain invoice with exact payment terms (USDC SPL mint, PayTo address, FeePayer sponsor, and micropayment amount).
2. **Autonomous SPL Settlement**: The agent's local signer signs a micro-transfer ($0.003–$0.0044 USDC) directly on Solana. Gas is sponsored by the FeePayer, requiring zero native SOL reserves from the agent.
3. **Vault Unlock & Aggregated Routing (`HTTP 200 OK`)**: The agent re-submits the quote request with the transaction receipt in the `X-PAYMENT` header. Upon on-chain confirmation, the Hermes router serves the best execution quote across Jupiter, DFlow, and OKX Dex.

This repository provides an interactive, client-side 3D kinetic visualization of this protocol built in WebGL/Three.js with procedural Web Audio synthesis.

---

## 2. Technical Implementation

- **Standalone 3D Monolith Engine**:
  - Engineered in vanilla WebGL via Three.js with zero build-step dependencies.
  - The custom 3D model is bundled in base64 within `swap_vault_data.js`, allowing 100% offline execution with zero CDN latency or CORS restrictions.
- **Physical PBR Horological Finishing**:
  - Restrained Swiss horological aesthetic (Solar Amber `#f59e0b` & brushed titanium, zero cyan/cyberpunk clutter).
  - High-anisotropy procedural dials generated via canvas textures with 240 micro-lathe grooves and horological typography.
  - Dual-axis trunnion gimbal assembly rotating along horizontal and orthogonal axes.
- **Procedural Foley Web Audio**:
  - Pure Web Audio API synthesis without external audio files (`.mp3`/`.wav`).
  - Synthesizes bandpass-filtered tactile relay clicks and low-frequency resonant sub-bass drops (140 Hz → 35 Hz) on settlement confirmation.
- **Strict Frame Budget**:
  - Enforced 16.6ms frame budget (60 FPS floor) with clamped device pixel ratio (DPR ≤ 2.0).

---

## 3. Protocol Verification & Testing

To test the machine-to-machine x402 handshake directly against Swap.io's gateway:

```bash
# 1. Unauthenticated request triggers HTTP 402
curl -i -X POST https://hermes.swap.io/v1/swap/quote \
  -H "Content-Type: application/json" \
  -d '{"inputMint":"EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v","outputMint":"So11111111111111111111111111111111111111112","amount":"10000000","slippageBps":50}'

# 2. Run the included Python agent client
python agent_swap_client.py
```

---

## 4. Production Links & Assets

| Asset | Link / Status | Description |
|---|---|---|
| **Live Interactive WebUI** | [apoorv-xs.github.io/swap-x402](https://apoorv-xs.github.io/swap-x402/) | Production deployment hosted on GitHub Pages |
| **Protocol Documentation** | [swap.io](https://swap.io) | Official Swap.io x402 protocol specification |
| **Source Code** | [github.com/apoorv-xs/swap-x402](https://github.com/apoorv-xs/swap-x402) | Complete client and agent handshake implementation |

---

*Authored by Apoorv A S — Creative Technologist & 3D WebUI Architect.*
