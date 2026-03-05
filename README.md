# AI Economic Identity Envelope (AIEI)

**Version 1.0.0** · **MIT License** · **March 2026**

A standard economic identity for every AI transaction.

---

## The Problem

Every AI API call today is economically anonymous. A request to OpenAI or Anthropic carries a prompt and returns a response — nothing more. It has no awareness of:

- **Who** it serves (which customer, which user, which feature)
- **What** it costs in context (step 3 of 20 in an agent workflow? cumulative session cost?)
- **What it's worth** (is this customer profitable? what's the margin after this call?)
- **What rules apply** (budget remaining? margin floor? compliance classification?)
- **Whether it can be proven** (can you verify this transaction happened exactly as recorded?)

Every other financial transaction in the world carries economic identity. Credit card swipes carry merchant, buyer, amount, and authorization. Wire transfers carry SWIFT codes. Shipping carries bills of lading. SaaS subscriptions carry customer and plan data.

AI transactions carry none of this.

## The Standard

The AI Economic Identity Envelope (AIEI) gives every AI transaction five fields:

| Field | Contains | Purpose |
|-------|----------|---------|
| **who** | Customer, user, agent, feature, session, provenance chain | Links every AI call to the business entity it serves |
| **what** | Provider, model, tokens, cost, latency, chain position | The economic inputs — what this call consumed |
| **worth** | Revenue, plan, margin, cost-to-serve, underwater flag | Connects AI cost to customer revenue. The field nobody else has. |
| **rules** | Budget, margin floor, compliance class, enforcement actions | Governance that travels with the transaction |
| **proof** | SHA-256 hash, chain link, HMAC signature, seal timestamp | Cryptographic attestation — tamper-evident and verifiable |

## Quick Start

### Option 1: Gateway (zero code change)

```python
from openai import OpenAI

client = OpenAI(
    api_key="sk-...",
    base_url="https://gateway.finault.ai/v1",  # ← one line
    default_headers={
        "X-Finault-Key": "fn_live_...",
        "X-Finault-Customer": "cust_acme_corp",
    }
)

response = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "Analyze this contract"}]
)

# Every response now includes:
# X-Finault-Cost: 0.0231
# X-Finault-Margin: 0.71
# X-Finault-Receipt: https://finault.ai/r/01961a2f
```

### Option 2: SDK

```python
from finault import Finault

finault = Finault(api_key="fn_live_...")

response = finault.complete(
    model="gpt-4o",
    messages=[{"role": "user", "content": "Analyze this contract"}],
    customer_id="cust_acme_corp",
    feature="contract-review",
)

print(response.identity.worth.margin_after)   # 0.71
print(response.identity.proof.hash)            # "7f2a9b..."
print(response.identity.receipt_url)           # "https://finault.ai/r/..."
```

## The Envelope

A complete AIEI envelope looks like this:

```json
{
  "version": "aiei/1.0.0",
  "id": "01961a2f-7d3e-7f8a-b4e1-9c2d3f4a5b6c",
  "timestamp": "2026-03-04T14:23:01.847Z",

  "who": {
    "org_id": "org_fintech_abc",
    "customer_id": "cust_acme_corp",
    "user_id": "user_jane_doe",
    "feature": "contract-review",
    "agent_id": "agent_doc_analyzer",
    "session_id": "sess_8f2a1b3c",
    "environment": "production"
  },

  "what": {
    "provider": "anthropic",
    "model": "claude-sonnet-4-20250514",
    "tokens_in": 4280,
    "tokens_out": 1650,
    "cost_usd": 0.0231,
    "latency_ms": 1847,
    "request_type": "chat",
    "chain_position": {
      "step": 3,
      "total_steps": 5,
      "cumulative_cost_usd": 0.0892
    }
  },

  "worth": {
    "customer_revenue_monthly": 499.00,
    "customer_plan": "professional",
    "margin_current": 0.72,
    "margin_after": 0.71,
    "cost_to_serve_cumulative": 142.30,
    "underwater": false
  },

  "rules": {
    "budget_remaining_usd": 357.70,
    "margin_floor": 0.40,
    "model_was_downgraded": false,
    "compliance_class": "standard",
    "actions_taken": ["none"]
  },

  "proof": {
    "hash": "7f2a9b4e1c3d5f6a...",
    "prior_hash": "3a1b5c7d9e0f2a4b...",
    "signature": "hmac_9e8d7c6b5a4f...",
    "algorithm": "sha256+hmac-sha256",
    "chain_depth": 4729,
    "sealed_at": "2026-03-04T14:23:01.892Z"
  }
}
```

See [`schema/aiei-v1.0.0.json`](schema/aiei-v1.0.0.json) for the complete JSON Schema.

## Regulatory Mapping

The AIEI maps directly to existing and upcoming regulatory requirements:

| Regulation | AIEI Fields | How It Maps |
|------------|-------------|-------------|
| **EU AI Act Article 12** | who + what + proof | Automatic logging over system lifetime |
| **EU AI Act Article 9** | rules + compliance_class | Risk management documentation |
| **Colorado SB205** | who + what + rules | Impact assessment data, 3-year retention |
| **SOC 2 AI Controls** | rules + proof | Evidence of budget enforcement, model governance |
| **Financial Audit** | worth + proof | Sealed AI P&L at transaction level |
| **ISO 27001** | proof + rules | Information security evidence chain |

See [`mappings/`](mappings/) for detailed regulatory mapping documents.

## The Receipt

Every AIEI-enriched transaction generates a receipt — a human-readable, cryptographically sealed, shareable proof accessible via a permanent URL:

```
https://finault.ai/r/01961a2f-7d3e
```

The receipt renders the complete envelope in a format readable by founders, CFOs, and auditors. It includes a "Verify Chain" button that independently confirms the cryptographic integrity of the seal chain.

Receipts don't expire. They're permanent, verifiable, and shareable.

## The Seal Chain

Every transaction's proof field links to the previous transaction via `prior_hash`. This creates an unbroken chain of economic events — tamper-evident, independently verifiable, and impossible to recreate after the fact.

```
Transaction #1:  hash=abc123  prior_hash=null       chain_depth=1
Transaction #2:  hash=def456  prior_hash=abc123     chain_depth=2
Transaction #3:  hash=ghi789  prior_hash=def456     chain_depth=3
...
Transaction #4729: hash=7f2a9b  prior_hash=3a1b5c   chain_depth=4729
```

After 6 months, a company has tens of thousands of sealed transactions in an unbroken chain. This chain is their auditable AI financial history. It cannot be edited, backdated, or fabricated.

## Close Packs

At the end of each billing period, all AIEI envelopes seal into a **Close Pack** — a monthly summary artifact containing:

- Total spend by provider, model, customer, and feature
- Customer margin analysis (profitable vs. underwater)
- Finault Score (5-dimension economic health metric)
- Chain integrity verification
- SHA-256 seal linking to the previous Close Pack

Close Packs are the AI equivalent of monthly financial statements. They're what a CFO hands to a board and what an auditor references in a compliance review.

## Design Principles

1. **Identity is first-class, not an afterthought.** The envelope is part of the transaction, not a log entry generated later.

2. **Revenue is required for intelligence.** Cost tracking without revenue data is a microscope. Economic intelligence requires both sides.

3. **Proof is the seal, not the data.** Anyone can generate a JSON file. The cryptographic proof — hash, chain link, signature — is what makes it trustworthy.

4. **The standard is open. The implementation is Finault.** The AIEI spec is MIT-licensed. Anyone can build emitters and consumers. Finault is the reference implementation and intelligence layer.

5. **Two lines of code or less.** Integration should never require more than a base URL change or a two-line SDK import.

## Repository Structure

```
aiei-spec/
├── README.md                    ← You are here
├── LICENSE                      ← MIT License
├── schema/
│   └── aiei-v1.0.0.json        ← Complete JSON Schema
├── examples/
│   ├── envelope-healthy.json    ← Example: profitable customer
│   ├── envelope-underwater.json ← Example: underwater customer
│   ├── envelope-agent-chain.json← Example: multi-step agent workflow
│   └── close-pack.json          ← Example: monthly Close Pack
└── mappings/
    ├── eu-ai-act.md             ← EU AI Act Article 12 & 9 mapping
    ├── soc2.md                  ← SOC 2 AI controls mapping
    └── colorado-sb205.md        ← Colorado SB205 mapping
```

## Contributing

The AIEI is an open standard. We welcome contributions:

- **Field proposals**: Open an issue describing the field, its purpose, and which envelope section it belongs to.
- **Regulatory mappings**: PRs adding mappings to new regulations or standards.
- **Implementation examples**: Code showing AIEI integration in different languages and frameworks.
- **Feedback**: Issues with questions, concerns, or use cases we haven't considered.

## About

The AIEI is published by [Finault](https://finault.ai), the AI Economic Intelligence platform. Finault provides the reference implementation of the AIEI standard through its gateway, SDKs, and intelligence layer.

**They track costs. Finault tracks profitability.**

---

*AI Economic Identity Envelope (AIEI) v1.0.0 · MIT License · finault.ai/aiei*
