# AIEI Mapping: Colorado SB 21-205 (AI Act)

**Version 1.0.0** · **March 2026**

This document maps AIEI envelope fields to Colorado's Artificial Intelligence Act (SB 21-205) requirements.

---

## Key Requirements

### Impact Assessments

Colorado SB205 requires deployers of high-risk AI systems to complete impact assessments that include documentation of the system's purpose, data inputs, outputs, and known risks.

| SB205 Requirement | AIEI Field(s) | Coverage |
|-------------------|----------------|----------|
| Document AI system purpose | `who.feature`, `who.service_id` | Full |
| Document data inputs | `what.tokens_in`, `what.model`, `what.request_type` | Full |
| Document outputs and decisions | `rules.actions_taken`, `rules.model_was_downgraded` | Full |
| Assess risks of algorithmic discrimination | `rules.compliance_class` tagging | Partial — requires external bias testing |
| Retain documentation for 3 years | Seal chain + Close Pack archive (configurable retention) | Full |

### Record Retention (3 Years)

| SB205 Requirement | AIEI Field(s) | Coverage |
|-------------------|----------------|----------|
| Retain records of AI system behavior | Complete AIEI envelopes persisted | Full |
| Records must be available for inspection | Receipt URLs permanent; exports available | Full |
| 3-year minimum retention | Close Pack archive with chain integrity | Full |

### Consumer Notification

| SB205 Requirement | AIEI Field(s) | Coverage |
|-------------------|----------------|----------|
| Notify consumers of consequential AI decisions | `rules.actions_taken`, `rules.model_was_downgraded` | Documented for internal use; consumer notification is application-layer |

---

## Export Format

Finault generates Colorado SB205 compliance exports containing:

1. **Impact assessment data**: AI system inventory, purposes, data flows
2. **Transaction history**: AIEI envelopes covering the assessment period
3. **Decision documentation**: All automated decisions with rationale (rules.actions_taken)
4. **Retention proof**: Chain integrity verification demonstrating unbroken 3-year record
5. **Sealed export**: Cryptographic proof of export completeness

---

*This mapping is provided as guidance. Organizations should consult with legal counsel to determine their specific obligations under Colorado SB 21-205.*
