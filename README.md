# TerraCommons

Community-rooted land rights infrastructure — a ValiChord use case built on Holochain's commit-reveal protocol.

---

## What TerraCommons Is

1.1 billion adults worldwide feel insecure about their property rights. Every previous technical solution — digital registries, blockchain pilots — has failed in high-corruption, low-connectivity environments. They failed not because the technology was wrong but because they misunderstood the problem.

Land rights are not primarily a data storage problem. They are a trust problem.

TerraCommons is built around one core idea: community members — neighbours, elders, local land administrators — independently confirm each other's land claims, and those confirmations are stored in a way that cannot be corrupted, erased, or overridden by officials who did not participate.

### The Hard/Soft Distinction

**What technology can guarantee:**
- Every validation is cryptographically signed — forgery is impossible
- Every evidence document is fingerprinted — tampering is detectable
- Every action is timestamped and permanent — the audit trail is complete
- Land transfers are atomic — selling the same land twice becomes a detectable, attributable fork rather than a quiet alteration

**What requires human judgment:**
- Where a boundary runs
- Whether an occupation claim is accurate
- Who is right when claims conflict

TerraCommons provides infrastructure for community knowledge to become cryptographically verifiable and institutionally legible. It does not replace community knowledge with technology.

---

## Architecture

Four Holochain DNAs with distinct membranes:

| Layer | DNA | Access | Purpose |
|---|---|---|---|
| 1 | Household | Private, per-household | Original evidence — never leaves without explicit household action |
| 2 | Community Registration | Credentialed membrane | Commit-reveal validation, countersigned transfers |
| 3 | Governance | Open read, credentialed write | Validator credentials, warrants, mediation records |
| 4 | Public Record | Open read via HTTP Gateway | Certified claims readable by World Bank, NGOs, journalists, courts |

**Commit-reveal validation** — validators seal their assessments before any are revealed. No validator can adjust their verdict after seeing others'. Collusion is detectable, not merely possible.

**Countersigned transfers** — a land transfer atomically updates both parties' source chains or updates neither. Selling the same land twice requires forking the seller's cryptographic record — a fork immediately visible to the network.

---

## Relationship to ValiChord

TerraCommons and [ValiChord](https://github.com/ValiChord/ValiChord) are siblings: separate Holochain deployments implementing the same commit-reveal architecture for different domains. ValiChord verifies scientific reproducibility; TerraCommons verifies community land rights. They share no network — each community deployment has its own DNA hash and network seed.

TerraCommons draws directly on ValiChord's empirical findings — DHT sync latency data, infrastructure requirements, and the two-round validation protocol.

The [`valichord-evals`](https://github.com/ValiChord/valichord-evals) library is a potential source of shared cryptographic primitives — canonical encoding, Merkle tree construction, content hashing — but its current Bundle schema is designed for AI evaluation runs and does not map cleanly onto land validation records. If TerraCommons uses it, it would need either a `TerraCommonsAdapter` that maps certified claim records into a land-rights-specific bundle schema, or the library would need to be generalised to support multiple domain types. The most concrete use case is generating tamper-evident, institutionally legible exports of certified claim records and community registry snapshots for submission to NGO partners and international institutions — not the full challenge/response machinery, which is designed for probabilistic sampling of large benchmark datasets and does not fit the small validator counts of a land validation session.

---

## Status

**Design stage. No working code exists.**

The architecture has been validated as architecturally sound by Arthur Brock (Holochain co-founder) and Paul D'Aoust (Holochain Foundation). The components — Holochain's commit-reveal, countersigning, membrane proofs, and DHT — are production-implemented features of the framework. The TerraCommons-specific assembly of those components has not been built or tested.

The Technical Reference identifies 15 Known Design Gaps requiring engineering resolution before the DNA can be finalised for deployment.

**What is needed:** a lead Holochain engineer to work through the Known Design Gaps and build toward a 500-household pilot.

---

## Documents

| Document | What it covers |
|---|---|
| [Vision & Architecture](docs/vision_and_architecture.md) | What TerraCommons is, why every previous solution failed, the four-layer architecture, pilot design, honest limitations |
| [Technical Reference](docs/technical_reference.md) | Holochain architecture sketches, commit-reveal protocol, countersigned transfers, GPS boundary design, 15 Known Design Gaps |
| [Governance Framework](docs/governance_framework.md) | Community Land Committee, the social fraud problem, validator accountability, NGO exit design, 11 Non-Negotiables |

---

## Pilot

**Location:** Turkana County, Kenya  
**Scale:** 500 households  
**Duration:** 18–24 months  
**Budget:** $500K–750K  
**Primary partner:** Landesa or equivalent land rights NGO with Kenya presence

Two parallel tracks from day one:
- **Track A** — settled parcel registration (the current architecture)
- **Track B** — pastoral governance ledger co-design with Turkana community members and pastoral rights specialists

A pilot that completes Track A but not Track B has failed one of its primary objectives regardless of parcel certification rates.

---

## Licence

Apache 2.0. © 2026 Ceri John. See [LICENSE](LICENSE).
