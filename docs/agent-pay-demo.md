# MAGNE Agent Pay Demo

> AI Agent Payment & Receipt Demo on M Hash L2 — Developer Demonstration

**⚠️ Disclaimer: This is a testnet-stage developer demo. Not a production payment system. Subject to technical validation.**

---

## Overview

The MAGNE Agent Pay Demo showcases an **x402-compatible AI task payment and receipt flow** running on M Hash L2 Testnet.

**Repository:** [jerrysohigh-create/magne-agent-pay-demo](https://github.com/jerrysohigh-create/magne-agent-pay-demo)

This demo demonstrates how AI agents can receive payments, verify transactions, and generate on-chain receipts — without modifying M Hash L2 core code or OP Stack deployment logic.

---

## Demo Flow

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│  Step 1      │     │  Step 2      │     │  Step 3      │     │  Step 4      │
│  AI Agent    │────▶│  HTTP 402    │────▶│  Payment     │────▶│  AI Task     │
│  Task        │     │  Required    │     │  Settlement  │     │  Receipt     │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
     │                    │                    │                    │
  Create task        402 with           MetaMask /            On-chain
  request            payment info       EVM wallet            receipt
```

### Step 1: AI Agent Task (HTTP 402 Payment Required)

The backend returns `HTTP 402` with x402-compatible payment metadata:

```json
{
  "error": "Payment Required",
  "protocol": "x402-compatible",
  "network": "mhash-l2-testnet",
  "chainId": 20250827,
  "amount": "0.01",
  "currency": "mMHA",
  "recipient": "0x...",
  "paymentId": "pay_...",
  "taskId": "task_...",
  "facilitator": "0x...",
  "description": "AI Agent Task Payment - text-generation"
}
```

### Step 2: Wallet Payment Authorization

User approves payment via MetaMask or any EVM-compatible wallet. Transaction settles on M Hash L2 (~400ms target block time).

### Step 3: Facilitator Verification

Backend verifies the payment by querying the transaction on-chain:
- Chain ID matches
- Recipient matches
- Amount matches
- Transaction status: success

### Step 4: M Hash L2 AI Task Receipt

Backend calls `AITaskReceipt.createReceipt()` on-chain, emitting `AITaskReceiptCreated` event with full receipt data.

---

## Smart Contracts

Deployed on M Hash L2 Testnet (Chain ID: `20250827`):

| Contract | Purpose |
|----------|---------|
| `MockMHA` | ERC20-like mock token (symbol: mMHA) for testnet demo |
| `AITaskReceipt` | Records AI task receipts on-chain |

These are standard EVM contracts. No modifications to M Hash L2 core or OP Stack logic.

---

## Chain ID Note

> ⚠️ **Current Kurtosis config may use `2151908`**, while the public M Hash L2 testnet chainId should follow the official network configuration (currently `20250827`).
>
> When integrating, always verify the chainId matches the target network. Demo is configured for `20250827`.

---

## Key Design Principles

1. **No Core Modifications** — Demo operates as a standard EVM contract layer on top of M Hash L2
2. **x402-Compatible** — Uses HTTP 402 with structured payment metadata for wallet integration
3. **Facilitator Pattern** — Backend handles verification and receipt generation off-chain
4. **Event-Based** — `AITaskReceiptCreated` events provide transparent, on-chain audit trail

---

## API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/agent/task` | POST | Create AI task → returns 402 payment required |
| `/paid-api/wallet-risk` | GET | Check payment status for a task |
| `/facilitator/verify` | POST | Verify payment transaction on-chain |
| `/facilitator/receipt` | POST | Generate AI task receipt on-chain |
| `/facilitator/status` | GET | Check facilitator service status |

---

## Risk Warnings

⚠️ **Testnet Only** — This demo runs exclusively on M Hash L2 Testnet. No mainnet deployment.

⚠️ **Not Production Payment System** — This is a developer demonstration. Not audited. Not suitable for production use.

⚠️ **Subject to Technical Validation** — All specifications, fees, and chain IDs are subject to change and technical validation.

⚠️ **No Financial Advice** — This demo does not constitute financial or investment advice. MHA token is not being promoted for purchase.

---

## Related Repositories

| Repo | Purpose |
|------|---------|
| [jerrysohigh-create/magne-agent-pay-demo](https://github.com/jerrysohigh-create/magne-agent-pay-demo) | This demo — x402-compatible payment flow |
| [magne-ai/M-Hash-L2](https://github.com/magne-ai/M-Hash-L2) | M Hash L2 OP Stack configuration |
| [magne-ai/MAGNE-L1](https://github.com/magne-ai/MAGNE-L1) | MAGNE L1 blockchain |
| [magne-ai/blockscout](https://github.com/magne-ai/blockscout) | Block explorer (M Hash L2) |
| [magne-ai/blockscout-frontend](https://github.com/magne-ai/blockscout-frontend) | Block explorer frontend |

---

## Network Configuration

| Parameter | Value |
|-----------|-------|
| Network Name | M Hash L2 Testnet |
| Chain ID | `20250827` (verify with official docs) |
| RPC URL | `https://testnet-rpc.mhash.ai` |
| Block Explorer | `https://testnet-explorer.mhash.ai` |
| Target Block Time | ~400ms |
| Target Gas Fee | <$0.0025 |

---

*Last updated: Testnet Demo V0.1*
