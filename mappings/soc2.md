# AIEI Mapping: SOC 2 AI Controls

**Version 1.0.0** · **March 2026**

This document maps AIEI envelope fields to SOC 2 Trust Service Criteria relevant to AI system governance.

---

## Relevant Trust Service Criteria

### CC6: Logical and Physical Access Controls

| SOC 2 Criteria | AIEI Field(s) | Evidence |
|----------------|----------------|----------|
| Access to AI systems is authorized | `who.org_id`, `who.user_id`, `who.environment` | Every transaction records the authenticated entity |
| Changes to AI system configuration | `rules.model_was_downgraded`, `rules.original_model` | Model routing decisions documented |
| Access logs retained | Seal chain with `proof.chain_depth` | Immutable, chronological record |

### CC7: System Operations

| SOC 2 Criteria | AIEI Field(s) | Evidence |
|----------------|----------------|----------|
| System processing monitored | `what.provider`, `what.model`, `what.latency_ms` | Every transaction monitored |
| Anomalies detected and addressed | `rules.actions_taken`, `worth.underwater` | Automatic detection and enforcement |
| Budget controls enforced | `rules.budget_remaining_usd`, `rules.budget_utilization` | Real-time budget enforcement documented |

### CC8: Change Management

| SOC 2 Criteria | AIEI Field(s) | Evidence |
|----------------|----------------|----------|
| Changes to AI models documented | `what.model`, `rules.model_was_downgraded`, `rules.original_model` | Every model routing decision recorded |
| Impact of changes assessed | `worth.margin_before` vs `worth.margin_after` | Economic impact computed per transaction |

### CC9: Risk Mitigation

| SOC 2 Criteria | AIEI Field(s) | Evidence |
|----------------|----------------|----------|
| Risks identified and mitigated | `rules.compliance_class`, `rules.margin_floor` | Risk classification and thresholds documented |
| Controls operating effectively | Close Pack monthly summary, Finault Score trend | Periodic evidence of control effectiveness |
| Third-party risk managed | `what.provider`, per-provider cost and reliability tracking | Vendor dependency documented |

---

## Evidence Package

For SOC 2 audits, Finault generates an evidence package containing:

1. **Transaction log** with complete AIEI envelopes for the audit period
2. **Budget enforcement log** showing all budget checks, alerts, and blocks
3. **Model governance log** showing all routing decisions and overrides
4. **Chain integrity proof** verifying log completeness
5. **Close Pack series** with month-over-month sealed summaries
6. **Finault Score trend** demonstrating governance effectiveness over time

All evidence is cryptographically sealed. Auditors can independently verify chain integrity without Finault access.

---

*This mapping is provided as guidance for SOC 2 readiness. Organizations should work with their auditors to determine specific evidence requirements.*
