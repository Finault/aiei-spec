# AIEI Mapping: EU AI Act

**Version 1.0.0** · **March 2026**

This document maps AIEI envelope fields to EU AI Act requirements. The EU AI Act enters enforcement in August 2026.

---

## Article 12: Record-Keeping (Automatic Logging)

Article 12 requires that high-risk AI systems include logging capabilities that enable the recording of events over the system's lifetime.

### Requirements and AIEI Mapping

| Article 12 Requirement | AIEI Field(s) | Coverage |
|------------------------|----------------|----------|
| Record events automatically over system lifetime | All fields persisted on every transaction | Full |
| Enable tracing of AI system operation | `who.session_id`, `who.parent_id`, `what.chain_position` | Full |
| Identify situations of risk | `worth.underwater`, `rules.margin_floor`, `rules.actions_taken` | Full |
| Logging proportionate to intended purpose | `who.feature`, `what.request_type`, `rules.compliance_class` | Full |
| Retain logs for appropriate period | Seal chain is permanent; Close Packs archived with chain integrity | Full |

### How It Works

When an AI system processes requests through a Finault gateway, every transaction is automatically logged with a complete AIEI envelope. The `who` field identifies the system, user, and purpose. The `what` field records the technical details. The `proof` field creates an immutable, verifiable record.

No additional instrumentation is required. Compliance is a byproduct of normal operation.

---

## Article 9: Risk Management

Article 9 requires a risk management system for high-risk AI systems, implemented throughout the AI system's lifecycle.

### Requirements and AIEI Mapping

| Article 9 Requirement | AIEI Field(s) | Coverage |
|------------------------|----------------|----------|
| Identify and analyze known risks | `rules.compliance_class`, `worth.underwater` | Partial — economic risks covered |
| Evaluate risks based on available data | `what.cost_usd`, `worth.margin_after`, trajectory analysis | Full for economic risk |
| Adopt risk management measures | `rules.actions_taken`, `rules.model_was_downgraded` | Full — actions documented |
| Testing to identify appropriate risk management | Finault Score trend over time | Partial |

### How It Works

The `rules.compliance_class` field tags each transaction with its regulatory classification (standard, high-risk, prohibited, etc.). The `rules.actions_taken` field documents what risk mitigation was applied — model downgrades, budget throttling, human escalation. The Close Pack monthly summary provides a periodic risk review artifact.

---

## Article 13: Transparency

Article 13 requires that high-risk AI systems are designed to be sufficiently transparent for users to interpret output.

### Requirements and AIEI Mapping

| Article 13 Requirement | AIEI Field(s) | Coverage |
|------------------------|----------------|----------|
| Enable interpretation of system output | Receipt page (human-readable envelope rendering) | Full |
| Appropriate level of accuracy, robustness | `what.model`, `what.tokens_in/out`, quality metrics | Partial |
| Human oversight measures | `rules.human_review_required`, `rules.actions_taken` | Full |

### How It Works

The receipt URL (`finault.ai/r/{id}`) provides a human-readable view of every AI transaction's economic identity. Any stakeholder — user, auditor, regulator — can inspect the receipt and understand who initiated the transaction, what it consumed, what rules governed it, and verify its proof chain.

---

## Export Format

Finault generates EU AI Act compliance exports containing:

1. **Transaction log**: All AIEI envelopes for the period, sorted chronologically
2. **Risk classification summary**: Count of transactions by compliance_class
3. **Actions taken summary**: Count and detail of enforcement actions
4. **Chain integrity verification**: Proof that the log is complete and unaltered
5. **Close Pack seal**: Cryptographic proof of the entire export

The export is available as JSON (machine-readable) and PDF (human-readable).

---

*This mapping is provided as guidance. Finault is not a law firm and this document does not constitute legal advice. Organizations should consult with legal counsel to determine their specific obligations under the EU AI Act.*
