# TerraCommons — Technical Reference
## Illustrative Architecture Sketches for Engineering Discussion

**Author:** Ceri John
**Date:** March 2026

**© 2026 Ceri John. Licensed under the Apache License, Version 2.0.**

**Contact:** topeuph@gmail.com

---

## Important Note on Status

**These are design intent documents, not implementation code.**

The Rust structures and functions in this document describe the *shape* of TerraCommons' architecture — data models, system flows, and component interactions. They are illustrative sketches developed through intensive architectural design in collaboration with Claude AI (Anthropic). They have not been compiled, tested, or audited. They are the starting point for a first proper engineering conversation, not the output of one.

**What "the technology is proven" means and does not mean:** The Vision & Architecture document states that "the technology is proven." This requires precise interpretation. What is proven: Holochain's core primitives — agent-centric source chains, DHT, countersigning, membrane proofs, and capability grants — are production-implemented features of the Holochain framework. They function as described in published Holochain documentation. The cryptographic properties of commit-reveal protocols are mathematically established. These are the building blocks TerraCommons assembles.

What is not proven: the TerraCommons-specific assembly of those primitives into a land rights validation system. No TerraCommons code has been written. No TerraCommons validation session has been run. The interaction between TerraCommons' specific multi-DNA architecture, its governance protocols, and Holochain's DHT behaviour at the scales required for a 500-household pilot has not been tested. The claim that this assembly will work as intended is an architectural judgment based on understanding of the primitives, not an empirical finding. A competent Holochain engineer reviewing this document may identify integration challenges, performance constraints, or design decisions that require revision before the architecture can be built.

Institutional partners should read "the technology is proven" as: the foundations are real and the architectural approach is grounded. They should not read it as: this has been built and works.

**What this document is for:** An engineer reading this should understand what TerraCommons needs to do, what data it handles, how the core protocols work, and where the open design questions are. It should allow technical discussion to begin at the right level.

**What this document is not:** Production code, a specification that can be implemented without modification, or evidence of technical progress beyond architectural design.

**Sections flagged `[OPEN DESIGN QUESTION]`** identify areas where the architectural approach is clear but specific design decisions require engineering input, empirical data from the pilot, or community input on the ground in Kenya. These are not weaknesses to hide — they are the real questions a first engineering conversation needs to address.

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────┐
│ DNA 4: Public Record                                    │
│ Open read via HTTP Gateway. Certified claims,           │
│ transfer records, dispute flags, aggregate stats.       │
│ Readable by World Bank, NGOs, journalists, lawyers.     │
└─────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────┐
│ DNA 3: Governance                                       │
│ Validator credentials, warrants, mediation records,     │
│ community governance decisions. Open read, credentialed │
│ write.                                                  │
└─────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────┐
│ DNA 2: Community Registration                           │
│ Shared DHT, credentialed membrane. Claim submission,    │
│ commit-reveal validation protocol, transfer             │
│ countersigning, dispute flagging.                       │
└─────────────────────────────────────────────────────────┘
┌═════════════════════════════════════════════════════════╗
║ DNA 1: Household                                        ║
║ Private, per-household. Original documents, evidence,   ║
║ private notes. Nothing leaves without explicit action.  ║
╚═════════════════════════════════════════════════════════╝
```

### Data Flow: Claim Registration

```
Household prepares claim privately (DNA 1)
         ↓
SHA-256 fingerprints generated for all evidence documents
         ↓
Structured claim package submitted to Community Registration (DNA 2)
[GPS boundary, evidence hashes, metadata — not raw documents]
         ↓
Required validators identified by DNA validation rules
         ↓
Commit-reveal protocol executes (all in DNA 2)
         ↓
Outcome written to Governance layer (DNA 3) — audit record
         ↓
Certified claim published to Public Record (DNA 4)
```

### Data Flow: Transfer

```
Seller initiates transfer in Community Registration (DNA 2)
         ↓
Countersigning session opens — both parties' chains frozen
         ↓
Transfer entry built collaboratively, both parties sign
         ↓
Session completer releases completed set
         ↓
Both parties commit transfer to their own source chains
         ↓
Transfer record published to Public Record (DNA 4)
```

---

## Type Conventions

Throughout this document the following type aliases are assumed:

- `Hash` = `[u8; 32]` (SHA-256 for evidence fingerprints; Holochain uses BLAKE2b internally for its own addressing)
- `DateTime` = UTC timestamp
- `AgentId` = Holochain `AgentPubKey` — the unique cryptographic identity per participant
- `Signature` = cryptographic signature from an agent's keypair (Ed25519)
- `GpsPolygon` = ordered sequence of (lat, lon) coordinate pairs forming a closed boundary
- `ExternalHash` = 32-byte identifier for data outside the DHT (SHA-256 of evidence documents)
- `ActionHash` / `EntryHash` = Holochain's own 39-byte addresses (BLAKE2b). **References between entries on the DHT use these, never `Hash`/`ExternalHash`** — you can only `must_get_*` an `ActionHash`, and a SHA-256 evidence fingerprint cannot be resolved to a record. Fields such as `claim_id`, `commitment_entry_hash`, and `issuer_credential_hash` are therefore `ActionHash` values despite the generic naming below.
- `Nonce` = random 32-byte value generated locally, kept private until reveal phase

These are illustrative. Final type definitions depend on Holochain SDK version and engineering decisions.

---

## Multi-DNA Architecture and Membranes

TerraCommons is structured as four distinct Holochain applications (DNAs) rather than a single monolithic application. Each DNA creates its own encrypted peer-to-peer network with its own **membrane** — the access boundary controlling who can join that network and what data exists within it.

This is not a policy layer on top of a shared database. The DNAs are genuinely separate organisms. Even where two communities deploy identical TerraCommons code, a **network seed** baked into each instance at deployment makes their networks separate — Turkana County's land registry and Samburu County's land registry will never synchronise unless explicitly bridged by an agent participating in both.

**Why four DNAs rather than one with access controls?**

Internal access controls inside a single DNA are policies. They can be bypassed by a node running a modified version of the coordinator zomes. A membrane is architecture. The Household DNA's private data cannot enter the Community Registration DNA's DHT because no code path connects them — the only way data crosses the boundary is an explicit action by the household agent, submitting a structured claim package. The sensitive original documents never move. This makes GDPR compliance an architectural property, not a compliance posture.

---

## DNA 1: Household DNA

**Membrane:** Private. Only the household agent can join this network. In practice, this may be a single device or a small set of devices belonging to the same household with shared keys.

**Purpose:** Store original evidence privately. Generate cryptographic fingerprints. Prepare structured claim packages for submission to the Community Registration network. Record privately held metadata the household is not yet ready to share.

**Zome structure:**
- `household_integrity`: defines HouseholdDocument, ClaimDraft, EvidenceBundle entry types; validates locally
- `household_coordinator`: CRUD for private documents; claim package preparation; submission to DNA 2 via bridge call

### HouseholdDocument

```rust
/// Raw evidence document stored privately
pub struct HouseholdDocument {
    /// What kind of evidence this is
    pub document_type: HouseholdDocumentType,
    
    /// Content — stored locally only, never leaves this DNA
    pub content: Vec<u8>,
    
    /// SHA-256 fingerprint of content
    /// This fingerprint CAN travel to the shared network; the content cannot
    pub sha256_fingerprint: ExternalHash,
    
    /// When this document was added to the household record
    pub recorded_at: DateTime,
    
    /// Optional description for the household's own reference
    pub description: Option<String>,
}

pub enum HouseholdDocumentType {
    /// GPS boundary coordinates
    BoundaryCoordinates,
    
    /// Photograph of land or dwelling
    Photograph,
    
    /// Written witness statement
    WitnessStatement { witness_name: String },
    
    /// Oral history recording
    OralHistoryRecording,
    
    /// Inherited document (previous title, agreement, letter)
    InheritedDocument,
    
    /// Government-issued document (where one exists)
    OfficialDocument { issuing_authority: String },
    
    /// Community-issued document (elder's letter, committee record)
    CommunityDocument { issuing_body: String },
    
    /// Other
    Other { description: String },
}
```

### ClaimDraft

The claim draft is the private working space before submission. The household assembles evidence, records GPS boundaries, and writes their claim narrative here before deciding to submit anything to the shared network.

```rust
pub struct ClaimDraft {
    pub draft_id: Hash,
    
    /// GPS polygon defining the claimed boundary
    /// [OPEN DESIGN QUESTION — see GPS section below]
    pub boundary: GpsPolygon,
    
    /// Evidence documents included in this claim
    /// Stored as fingerprints only — actual documents stay in HouseholdDocument entries
    pub evidence_fingerprints: Vec<EvidenceItem>,
    
    /// Occupation history narrative (private draft)
    pub occupation_narrative: String,
    
    /// Type of land rights being claimed
    pub claim_type: LandClaimType,
    
    /// Draft status
    pub status: DraftStatus,
    
    pub created_at: DateTime,
    pub last_modified: DateTime,
}

pub enum LandClaimType {
    /// Individual household parcel
    IndividualParcel,
    
    /// Communal land shared by multiple households
    /// [OPEN DESIGN QUESTION — communal types need extended design for pastoralist use]
    CommunalParcel { community_id: AgentId },
    
    /// Seasonal use rights (grazing, water access)
    SeasonalUseRight {
        season: Season,
        use_type: UseType,
    },
    
    /// Inheritance claim on a previously registered parcel
    InheritanceClaim { predecessor_claim_id: Hash },
}

pub enum DraftStatus {
    InProgress,
    ReadyToSubmit,
    Submitted { submission_id: Hash },
}
```

### EvidenceItem

```rust
/// Reference to evidence — fingerprint only, not the document itself
pub struct EvidenceItem {
    /// SHA-256 fingerprint of the evidence document
    pub fingerprint: ExternalHash,
    
    /// Type of evidence
    pub evidence_type: EvidenceType,
    
    /// Strength assessment (household's own view)
    pub strength: EvidenceStrength,
    
    /// When this evidence was recorded
    pub dated_at: Option<DateTime>,
}

pub enum EvidenceType {
    /// GPS coordinates (the boundary itself)
    BoundaryData,
    
    /// Visual documentation
    Photograph,
    
    /// Testimony from a named person
    WitnessStatement,
    
    /// Occupancy duration evidence
    OccupationRecord {
        years_occupied: u32,
        generations: u32,
    },
    
    /// Documentary evidence
    Document,
}

pub enum EvidenceStrength {
    Strong,    // Multiple corroborating sources, long duration
    Moderate,  // Some corroboration, reasonable duration  
    Weak,      // Single source, recent, or contested
    Unknown,
}
```

### ClaimPackage (the only thing that leaves DNA 1)

When the household decides to submit, the coordinator zome prepares a ClaimPackage — structured metadata and fingerprints only. The actual documents remain in DNA 1.

```rust
/// The structured submission that travels to Community Registration DNA
/// Contains NO original documents — only fingerprints and structured metadata
pub struct ClaimPackage {
    /// Unique identifier for this submission
    pub package_id: Hash,
    
    /// The household's agent identity
    pub claimant: AgentId,
    
    /// GPS boundary (coordinates are metadata, not personal data)
    pub boundary: GpsPolygon,
    
    /// Evidence fingerprints — validators can request to verify documents
    /// against these fingerprints; they cannot access the documents themselves
    pub evidence_fingerprints: Vec<EvidenceItem>,
    
    /// Occupation duration claim
    pub occupation_years: u32,
    pub occupation_generations: u32,
    
    /// Type of rights being claimed
    pub claim_type: LandClaimType,
    
    /// SHA-256 of the complete ClaimDraft — proves the claim hasn't changed since preparation
    pub draft_fingerprint: ExternalHash,
    
    /// Submission timestamp
    pub submitted_at: DateTime,
    
    /// Household's signature over the package
    pub claimant_signature: Signature,
}
```

> **Engineering note:** The bridge call from DNA 1 to DNA 2 that submits the ClaimPackage requires the household agent to be joined to both networks. In Holochain, a hApp can include multiple DNAs, and the conductor routes calls between them. The household agent holds keys for their DNA 1 instance and presents separate credentials for their DNA 2 membership. This is standard Holochain multi-DNA design.

> **Required UX step — claimant coordinate confirmation:** Where GPS coordinates are recorded by a community facilitator on behalf of a household (the standard model for low-digital-literacy participants), the claimant must confirm the recorded coordinates on their own device before the ClaimPackage can be submitted. This is not an optional accessibility feature — it is a required verification step. A facilitator who records coordinates 20 metres from where the claimant indicated, whether accidentally or deliberately, generates an overlap with a neighbouring claim between parties who actually agree. Without the confirmation step, this error is committed to the DHT and triggers false conflict arbitration. The confirmation UI must display the boundary on a visible map and require explicit claimant approval — not a passive timeout. The confirmation action must be recorded in the ClaimDraft's audit trail.

> **Limitation of the confirmation step for low-literacy claimants:** The confirmation step reduces facilitator error risk but does not eliminate it for claimants who cannot read a map. A claimant who cannot interpret the displayed boundary may confirm whatever the facilitator presents, making the confirmation a gesture rather than a verification. For low-literacy households, the confirmation step alone is insufficient. A partial additional mitigation: require a second independent witness — a different community member who was present during the GPS walk and can separately attest that the recorded boundary matches the walked boundary. This witness attestation is recorded in the ClaimDraft alongside the claimant confirmation. It does not eliminate the risk but creates a second point of accountability and makes coordinated facilitator fraud require two co-conspirators rather than one. The witness credential model for this role is an open design question requiring specification before deployment.

> **The witness collusion limit — an honest acknowledgement:** The second witness requirement assumes that "independent" is achievable in a clan-based community. It may not be. In a tight kinship network, the coordination cost of finding a compliant witness is low — a powerful facilitator has ready access to family members, allies, and social dependants who will confirm whatever they are asked to confirm. Two co-conspirators are structurally harder to coordinate than one, but only marginally so where social obligation routinely overrides formal independence requirements. The witness requirement reduces the risk; it does not restructure the underlying power relationship between a digitally competent facilitator and a zero-literacy household. The honest primary detection mechanism for facilitator fraud is not the witness requirement — it is the qualitative corruption indicators tracked over time: dispute rate post-certification, arbitration request patterns, and concordance analysis of facilitator-assisted versus self-submitted claims. These become meaningful only after months of accumulated data. In the early pilot phase, active Land Committee oversight — including the Land Committee chair periodically attending facilitation sessions unannounced — is the more reliable check. The witness requirement is a necessary but insufficient safeguard, and should be described as such in community communications.

> **GPS accuracy and minimum parcel size:** Consumer GPS accuracy is typically 3–10 metres under good conditions. For small homestead parcels — a 15x15 metre plot, for instance — a 5-metre circle of error on all four corners produces a recorded boundary that is operationally meaningless and will overlap with every adjacent parcel by default. At small parcel scales, the overlap detection system produces constant false positives from GPS noise rather than genuine boundary disputes. A minimum parcel size must be established before deployment below which the GPS polygon model is not used. For sub-threshold parcels, an alternative documentation approach is required — perimeter walk with direction and timestamp recordings, social boundary testimony with multiple witnesses, or community sketch maps confirmed by adjacent households — and this requires a distinct entry type in the DNA. See also Open Design Question 9 in the Vision & Architecture document.

---

## DNA 2: Community Registration DNA

**Membrane:** Credentialed. Joining requires a valid credential signed by the community's authorised issuing authority. The issuer's public key is stored in DNA properties and is therefore part of the DNA hash — changing who can issue credentials means deploying a new network.

**Purpose:** The shared community layer. Receives claim packages, runs the commit-reveal validation protocol, records transfers via countersigning, flags disputes, and maintains the live state of all community land claims.

**Zome structure:**
- `registration_integrity`: defines all entry and link types; validation rules; membrane proof logic
- `registration_coordinator`: claim submission; validator notification; commit-reveal orchestration; countersigning initiation; dispute flagging; bridge calls to DNA 3 and DNA 4

### Membrane Proof Design

The two-stage membrane proof controls who can join the Community Registration network.

```rust
/// Stage 1: genesis_self_check — runs before network join, no DHT access
/// Checks format only: is this credential structurally well-formed? It cannot verify the
/// issuer's signature (no verify_signature in HDI) nor that the issuer exists on the network.
pub fn genesis_self_check(data: GenesisSelfCheckData) -> ExternResult<ValidateCallbackResult> {
    let membrane_proof = data.membrane_proof
        .ok_or(wasm_error!("No membrane proof provided"))?;
    
    let credential: JoiningCredential = decode(membrane_proof)?;
    
    // Check structural validity
    if credential.community_id.is_empty() {
        return Ok(ValidateCallbackResult::Invalid("Missing community_id".into()));
    }
    
    // NOTE: the issuer-signature check CANNOT run here. `verify_signature` is an HDK
    // host function not exposed to HDI/integrity code, and genesis_self_check must be
    // deterministic. Per ValiChord's working pattern, the full Ed25519 check (issuer key
    // from DNA properties, signature over the joining agent's key) runs in the
    // coordinator's `init()` callback; if it fails, init() returns Fail and the cell can
    // never write protocol data. Only format checks belong here.
    
    Ok(ValidateCallbackResult::Valid)
}

/// Stage 2: validate_agent_joining — runs when an existing peer validates a new joiner
/// Has full DHT access. Checks the issuing authority's own credential is on the network.
pub fn validate_agent_joining(
    agent_pub_key: AgentPubKey,
    membrane_proof: &Option<MembraneProof>,
) -> ExternResult<ValidateCallbackResult> {
    let proof = membrane_proof.as_ref()
        .ok_or(wasm_error!("No membrane proof"))?;
    
    let credential: JoiningCredential = decode(proof)?;
    
    // Verify the issuing authority credential exists on the governance DHT
    // [OPEN DESIGN QUESTION: cross-DNA read requires careful bridge design]
    let issuer_record = must_get_valid_record(credential.issuer_credential_hash)?;
    
    // Verify the issuer's credential is currently active (not revoked)
    let issuer_status = get_issuer_status(credential.issuer_id)?;
    if issuer_status == IssuerStatus::Revoked {
        return Ok(ValidateCallbackResult::Invalid("Issuer credential revoked".into()));
    }
    
    // NOTE: the "issuer signed this agent's key" Ed25519 check is NOT done here —
    // verify_signature is unavailable in HDI. It runs in the coordinator's init()
    // callback (which fails the cell if invalid). This callback may still read the DHT
    // (the must_get_* above) to confirm the issuer's credential exists and is not revoked.
    
    Ok(ValidateCallbackResult::Valid)
}
```

> **Engineering note — dispatch and signatures (applies to every `validate_*` sketch below):** Real Holochain integrity zomes expose a single `validate(op: Op)` entry point that dispatches via `op.flattened::<EntryTypes, LinkTypes>()`. The per-entry `validate_*` functions shown here are illustrative *branches* of that dispatch, not standalone callbacks. Two consequences run through every sketch: (1) `verify_signature` is an HDK host function **not available in HDI/integrity code**, so signature checks either rely on `action.author()` (the Action is already signature-verified by the system) or move to the coordinator / `init()` callback — exactly as ValiChord does (its full Ed25519 membrane-proof check runs in `init()`, with integrity doing format-only validation); and (2) references between entries use Holochain `ActionHash` values (see Type Conventions) — which is what `must_get_*` requires — not the SHA-256 `Hash` used for evidence fingerprints.

```rust
/// DNA properties — baked into the DNA hash at deployment
/// Changing any of these fields creates a new DNA and a new network
pub struct DnaProperties {
    /// Public key of whoever is authorised to issue joining credentials
    /// This might be a village elder, an NGO fieldworker, a land committee
    pub authorized_issuer_key: AgentPubKey,
    
    /// Number of validators required before a claim can be certified
    pub required_validators: u32,    // e.g. 5
    
    /// Agreement percentage required for certification (0-100)
    pub certification_threshold: u32,  // e.g. 70
    
    /// Agreement percentage below which arbitration is triggered
    pub arbitration_threshold: u32,    // e.g. 60
    
    /// How long a commit-reveal session stays open before timing out
    pub session_timeout_hours: u32,    // [OPEN DESIGN QUESTION — needs empirical calibration]
    
    /// Community identifier
    pub community_id: String,
    
    /// Network seed — makes this community's network distinct from all others
    /// even when running identical code
    pub network_seed: String,
}
```

> **Engineering note:** DNA properties are a standard Holochain mechanism. They are serialised and included in the DNA hash computation. This means the community's validation rules are tamper-evident — any change to the required validator count, threshold percentages, or authorised issuer creates a new DNA and a new network. This is the correct behaviour: a community that wants to change its rules should consciously create a new network, not silently alter the existing one. Migration of existing records between network versions is a governance question, not a technical one.

### LandClaim (entry in Community Registration DNA)

```rust
/// A submitted land claim — created when a household submits a ClaimPackage
pub struct LandClaim {
    /// Unique identifier for this claim
    pub claim_id: Hash,
    
    /// The household making the claim
    pub claimant: AgentId,
    
    /// GPS boundary — reproduced from the ClaimPackage for DHT indexing
    pub boundary: GpsPolygon,
    
    /// Hash of the original ClaimPackage — proves the claim hasn't been altered
    pub package_fingerprint: ExternalHash,
    
    /// Evidence items (fingerprints only, same as ClaimPackage)
    pub evidence_fingerprints: Vec<EvidenceItem>,
    
    /// Occupation claim
    pub occupation_years: u32,
    pub occupation_generations: u32,
    
    /// Type of rights being claimed
    pub claim_type: LandClaimType,
    
    /// Current status
    pub status: ClaimStatus,
    
    pub submitted_at: DateTime,
    
    /// Claimant's signature (carried from ClaimPackage)
    pub claimant_signature: Signature,
}

pub enum ClaimStatus {
    /// Submitted, awaiting validator assignment
    Pending,
    
    /// Validators assigned, awaiting commitments
    AwaitingCommitments {
        assigned_validators: Vec<AgentId>,
        deadline: DateTime,
    },
    
    /// All commitments received, awaiting reveals
    AwaitingReveals {
        commitment_count: u32,
        deadline: DateTime,
    },
    
    /// Certified — sufficient agreement reached
    Certified {
        certified_at: DateTime,
        agreement_percentage: u32,
        harmony_record_id: Hash,
    },
    
    /// Referred to arbitration — insufficient agreement
    InArbitration {
        referred_at: DateTime,
        agreement_percentage: u32,
        mediator_id: Option<AgentId>,
    },
    
    /// Conflicting with another claim — flagged for governance
    Conflicted {
        conflicting_claim_id: Hash,
        flagged_at: DateTime,
    },
    
    /// Rejected — failed basic validation
    Rejected { reason: String },
}
```

---

## The Commit-Reveal Protocol

This is the heart of TerraCommons. It is what makes community validation structurally resistant to collusion rather than merely policy-resistant. Every detail matters.

### Protocol Overview

```
Claim enters validation queue
         ↓
Required validators identified by DNA validation rules
         ↓
COMMIT PHASE
Each validator independently:
  1. Reviews the claim package
  2. Forms their assessment
  3. Generates a private nonce
  4. Hashes (assessment + nonce) → commitment
  5. Writes CommitmentEntry to DHT
  [Cannot change assessment after this point]
         ↓
Coordinator polls DHT:
  Are all required commitments present?
  → NO: wait (up to session_timeout_hours)
  → TIMEOUT: session fails, claim stays in AwaitingCommitments
  → YES: transition to reveal phase
         ↓
REVEAL PHASE
Each validator:
  1. Submits their original assessment + nonce
  2. System verifies: hash(assessment + nonce) == recorded commitment
  → MISMATCH: manipulation detected → warrant issued
  → MATCH: reveal accepted
         ↓
All reveals collected → agreement analysis
         ↓
≥ certification_threshold: claim certified
< certification_threshold but ≥ arbitration_threshold: document disagreement, seek additional validators
< arbitration_threshold: refer to community arbitration
```

### CommitmentEntry

```rust
/// Written to the shared DHT during the commit phase
/// Contains the commitment hash only — assessment is hidden until reveal
pub struct CommitmentEntry {
    /// Which claim this commitment is for
    pub claim_id: Hash,
    
    /// The validator making this commitment
    pub validator: AgentId,
    
    /// Blinded commitment: SHA-256(assessment_bytes ++ nonce)
    /// The assessment itself is NOT included here
    pub commitment_hash: Hash,
    
    /// When this commitment was made
    pub committed_at: DateTime,
    
    /// Validator's signature over the commitment hash
    /// Proves the validator cannot later deny having made this commitment
    pub validator_signature: Signature,
}
```

```rust
/// Validation of a CommitmentEntry — runs on every DHT authority for this entry
pub fn validate_commitment_entry(entry: CommitmentEntry) -> ExternResult<ValidateCallbackResult> {
    // Membership is enforced by the membrane (no valid credential, no network access), so
    // it need not be re-checked per entry — and an AgentPubKey is not a record address that
    // could be passed to must_get_valid_record anyway.
    
    // Verify this validator was actually assigned to this claim
    // `claim_id` is a Holochain ActionHash (see Type Conventions), not a SHA-256 fingerprint.
    let claim = must_get_valid_record(entry.claim_id.clone())?;
    let land_claim: LandClaim = claim.entry().to_app_option()?.ok_or(wasm_error!("Not a LandClaim"))?;
    
    let assigned = match &land_claim.status {
        ClaimStatus::AwaitingCommitments { assigned_validators, .. } => {
            assigned_validators.contains(&entry.validator)
        },
        _ => false,
    };
    
    if !assigned {
        return Ok(ValidateCallbackResult::Invalid("Validator not assigned to this claim".into()));
    }
    
    // Identity without verify_signature (unavailable in HDI): this is dispatched from
    // validate(op), so check `entry.validator == action.author()` — the Action is already
    // signature-verified by the system. Signature checks over payloads go in the coordinator.
    
    // Check session has not timed out
    // (If properties are needed, use the generated helper `DnaProperties::try_from_dna_properties()?`.)
    // [timeout check implementation depends on sys_time availability in validation context]
    // [OPEN DESIGN QUESTION: validation must be deterministic — sys_time cannot be used here]
    // [timeout enforcement belongs in coordinator logic, not validation]
    
    Ok(ValidateCallbackResult::Valid)
}
```

> **Engineering note:** Validation callbacks in Holochain integrity zomes must be deterministic — they cannot call `sys_time()` because the result would differ between validators running the callback at different times. Session timeout enforcement therefore belongs in the coordinator zome, not the integrity zome. The coordinator polls the DHT, checks timestamps on CommitmentEntry records against the current time, and decides whether to transition the session. This is the same pattern used in ValiChord.

### RevealEntry

```rust
/// Written to the shared DHT during the reveal phase
/// Contains the actual assessment — now visible to all participants
pub struct RevealEntry {
    /// Which claim this reveal is for
    pub claim_id: Hash,
    
    /// The validator making this reveal
    pub validator: AgentId,
    
    /// The actual assessment — now disclosed
    pub assessment: ValidatorAssessment,
    
    /// The nonce used in the commitment hash
    /// Anyone can now verify: SHA-256(assessment_bytes ++ nonce) == commitment_hash
    pub nonce: Nonce,
    
    /// Hash of the corresponding CommitmentEntry
    /// Creates an explicit link between commit and reveal
    pub commitment_entry_hash: Hash,
    
    /// When this reveal was made
    pub revealed_at: DateTime,
    
    /// Validator's signature over (assessment ++ nonce)
    pub validator_signature: Signature,
}
```

```rust
/// Validation of a RevealEntry — the critical integrity check
pub fn validate_reveal_entry(reveal: RevealEntry) -> ExternResult<ValidateCallbackResult> {
    // Retrieve the corresponding commitment
    // `commitment_entry_hash` is an ActionHash (see Type Conventions).
    let commitment_record = must_get_valid_record(reveal.commitment_entry_hash.clone())?;
    let commitment: CommitmentEntry = commitment_record.entry().to_app_option()?
        .ok_or(wasm_error!("Not a CommitmentEntry"))?;
    
    // The key integrity check:
    // Does hashing (this assessment + this nonce) produce the recorded commitment?
    let assessment_bytes = encode(&reveal.assessment)?;
    let nonce_bytes = reveal.nonce.as_bytes();
    let recomputed_hash = sha256([&assessment_bytes[..], nonce_bytes].concat());
    
    if recomputed_hash != commitment.commitment_hash {
        // Manipulation detected — the validator changed their assessment after committing
        // The coordinator will issue a warrant on the basis of this validation failure
        return Ok(ValidateCallbackResult::Invalid(
            "Reveal does not match commitment — assessment was changed after committing".into()
        ));
    }
    
    // Verify the claim_id matches
    if reveal.claim_id != commitment.claim_id {
        return Ok(ValidateCallbackResult::Invalid("claim_id mismatch".into()));
    }
    
    // Verify the validator matches
    if reveal.validator != commitment.validator {
        return Ok(ValidateCallbackResult::Invalid("validator mismatch".into()));
    }
    
    // Identity without verify_signature (HDI restriction): dispatched from validate(op),
    // check `reveal.validator == action.author()`. The Action is already signature-verified.
    
    Ok(ValidateCallbackResult::Valid)
}
```

### ValidatorAssessment

```rust
/// The validator's actual assessment of a land claim
pub struct ValidatorAssessment {
    /// Overall verdict
    pub verdict: AssessmentVerdict,
    
    /// Boundary agreement
    pub boundary_assessment: BoundaryAssessment,
    
    /// Occupation claim assessment
    pub occupation_assessment: OccupationAssessment,
    
    /// Confidence in this assessment
    pub confidence: Confidence,
    
    /// Free text notes (optional)
    pub notes: Option<String>,
    
    /// Whether this validator recommends arbitration regardless of threshold
    pub flag_for_arbitration: bool,
    pub arbitration_reason: Option<String>,
}

pub enum AssessmentVerdict {
    /// Agree with the claim as submitted
    Agree,
    
    /// Agree with modifications (boundary adjustment, qualification)
    AgreeWithModification { details: String },
    
    /// Disagree — specific reason required
    Disagree { reason: DisagreementReason },
    
    /// Insufficient information to assess
    Abstain { reason: String },
}

pub enum DisagreementReason {
    /// Boundary as claimed does not match what validator knows
    BoundaryDispute { known_boundary: Option<GpsPolygon> },
    
    /// Occupation claim is inaccurate
    OccupationDispute { known_duration: Option<u32> },
    
    /// Another party has a competing claim to this land
    CompetingClaim { known_claimant: Option<String> },
    
    /// Evidence is insufficient or appears fabricated
    InsufficientEvidence,
    
    /// Other
    Other { details: String },
}

pub struct BoundaryAssessment {
    /// Does the GPS polygon match what this validator knows about the boundary?
    pub boundary_accurate: bool,
    
    /// If not accurate, what adjustment does the validator suggest?
    /// [OPEN DESIGN QUESTION: how do we represent boundary adjustment suggestions?
    /// A replacement polygon? A list of disputed vertices? This needs UX input.]
    pub suggested_adjustment: Option<GpsPolygon>,
    
    /// How well does the validator know this boundary?
    pub familiarity: BoundaryFamiliarity,
}

pub enum BoundaryFamiliarity {
    /// Adjacent — shares this boundary directly
    Adjacent,
    
    /// Nearby — within the community, knows this area
    Nearby,
    
    /// General knowledge — community member but not directly adjacent
    General,
}
```

### Agreement Analysis

After all reveals are collected, the coordinator analyses the results and determines the claim outcome.

```rust
pub struct AgreementAnalysis {
    pub claim_id: Hash,
    
    /// Total validators who revealed
    pub total_reveals: u32,
    
    /// Validators who agreed (fully or with minor modification)
    pub agreement_count: u32,
    
    /// Agreement percentage
    pub agreement_percentage: u32,
    
    /// Detailed breakdown of verdicts
    pub verdict_breakdown: VerdictBreakdown,
    
    /// Whether any validator flagged for arbitration
    pub arbitration_flagged: bool,
    
    /// Outcome determination
    pub outcome: ClaimOutcome,
    
    pub analysed_at: DateTime,
}

pub enum ClaimOutcome {
    /// Agreement threshold met
    Certified,
    
    /// Below certification but above arbitration threshold
    /// System seeks additional validators before deciding
    InsufficientAgreement { additional_validators_needed: u32 },
    
    /// Below arbitration threshold — refer to community governance
    ReferredToArbitration,
    
    /// Conflict detected — competing claim exists
    Conflicted { competing_claim_id: Hash },
}
```

### A note from ValiChord development: the two-round option

**ValiChord finding:** ValiChord — the sibling system built and tested on this same commit-reveal core — identified a refinement worth offering here as an opt-in. In the single-round protocol above, a validator seals their *verdict* on the claim at the same moment they seal their *knowledge* of the boundary. A two-round variant separates the two:

- **Round 1 (raw knowledge, sealed):** each validator independently seals the boundary as they personally know it — their own GPS line, their own account of occupation and duration — with no verdict on the claimant's submission.
- **Round 2 (verdict, sealed):** once all Round 1 knowledge is revealed, each validator seals a verdict on the *claimant's* boundary, still blind to other validators' verdicts.

Both rounds keep the same blind structure. The payoff is twofold. First, Round 1 captures each neighbour's *independent* version of the boundary as sealed evidence — far richer material for arbitration than a bare agree/disagree. Second, publishing both layers side by side creates an immediate, individual-claim-level accountability check: a validator whose own sealed knowledge contradicts their own verdict is visibly exposed in a single record, without waiting for longitudinal pattern analysis. This is the most direct partial answer the architecture offers to the social-fraud problem (see Governance Framework). ValiChord implements it as two new phase states and two new entry types (`ValidatorRawFindings`, `VerdictAnchor`) with **no change to the governance or record layer** — the same staging would apply here. Recommended as opt-in for complex or contested boundaries, not as the default for straightforward adjacent-parcel claims, since it adds a second commit-reveal cycle and its coordination cost.

---

## Transfer Protocol: Countersigning

This section is precise because countersigning is the structural solution to double-selling — the most common fraud in corrupt land registries. It is a standard Holochain mechanism.

### TransferEntry

```rust
/// A countersigned land transfer — both parties must sign identical content
/// The transfer atomically updates both parties' source chains,
/// or updates neither.
pub struct TransferEntry {
    /// Unique identifier for this transfer
    pub transfer_id: Hash,
    
    /// The existing certified claim being transferred
    pub claim_id: Hash,
    
    /// Current holder (seller/transferor)
    pub transferor: AgentId,
    
    /// Incoming holder (buyer/recipient)
    pub transferee: AgentId,
    
    /// Type of transfer
    pub transfer_type: TransferType,
    
    /// Agreed terms (sale price, conditions, date)
    /// For non-sale transfers, may be empty or contain inheritance terms
    pub terms: TransferTerms,
    
    /// GPS boundary of the land being transferred
    /// Reproduced here to make the transfer self-describing
    pub boundary: GpsPolygon,
    
    /// Hash of the claim record at the moment of transfer
    /// Proves both parties agreed to transfer the same specific certified claim
    pub claim_state_at_transfer: Hash,
    
    /// Timestamp agreed by both parties during the countersigning session
    pub transfer_at: DateTime,
    
    /// Transferor's signature
    pub transferor_signature: Signature,
    
    /// Transferee's signature
    /// Both signatures over identical content is what makes this countersigned
    pub transferee_signature: Signature,
}

pub enum TransferType {
    Sale { price: Option<String> },
    Gift,
    Inheritance { relationship: String },
    CommunityReassignment { authorised_by: AgentId },
}

pub struct TransferTerms {
    pub effective_date: DateTime,
    pub conditions: Vec<String>,
    pub witnessed_by: Vec<AgentId>,
}
```

### Why Countersigning Prevents Double-Selling

When Alice attempts to transfer her land to Bob, the countersigning session opens on Alice's chain. Alice's chain is frozen — she cannot create any other actions until the session completes or times out. If Alice simultaneously tries to open a countersigning session with Carol for the same land, she would need to create two competing "next" entries on her chain from the same position. The DHT detects this immediately as a source chain fork — two entries both claiming to be `seq_n+1` after the same `seq_n`. Both entries are valid individually but contradictory together. The DHT marks Alice's agent as warranted, both transfer sessions are suspended, and the case is referred to the Land Committee for formal arbitration under the documented dispute resolution process.

This is an important distinction. "The community is alerted" is not a remediation mechanism — social pressure is precisely the enforcement model TerraCommons identifies as unreliable and corruptible in its own problem statement. Post-fork remediation must route through the governance layer's documented arbitration process, with named arbitrators, recorded evidence, and a written finding. If Alice has already received payment from both Bob and Carol before the fork is detected, TerraCommons cannot automatically unwind the fraud. What it provides is a cryptographic record of the fork that is admissible in arbitration and, where legal standing exists, in court. Prevention of double-payment relies on community practice — sellers and buyers confirming network status before transferring money — rather than on anything the architecture can guarantee.

**Counterweight:** This detect-not-prevent limitation should be read against the realistic alternative, not against perfection. A centralised registry prevents double-selling outright with a uniqueness constraint — but only while its operator is honest, and TerraCommons exists precisely for contexts where the operator is the likely fraudster. Countersigning delivers atomic two-party transfer with no trusted referee: no administrator who can be bribed to wave the second sale through, and no central record that can be quietly altered to hide it. The fork becomes permanent, attributable evidence rather than a deniable clerical error. Against a bribable registrar — the actual competitor in these environments — that is a structural improvement, even though it stops short of making fraud economically impossible.

```rust
/// Validation of a TransferEntry — runs on DHT authorities
pub fn validate_transfer_entry(transfer: TransferEntry) -> ExternResult<ValidateCallbackResult> {
    // Verify the claim being transferred is actually certified
    // `claim_id` is the ActionHash of the certified claim (see Type Conventions).
    let claim_record = must_get_valid_record(transfer.claim_id.clone())?;
    let claim: LandClaim = claim_record.entry().to_app_option()?
        .ok_or(wasm_error!("Not a LandClaim"))?;
    
    match &claim.status {
        ClaimStatus::Certified { .. } => {},
        _ => return Ok(ValidateCallbackResult::Invalid("Cannot transfer uncertified claim".into())),
    }
    
    // Verify transferor is the current registered holder
    if claim.claimant != transfer.transferor {
        return Ok(ValidateCallbackResult::Invalid("Transferor is not the registered holder".into()));
    }
    
    // NOTE: both parties signing identical content is guaranteed by the Holochain
    // countersigning session that produced this entry — not re-checked with
    // verify_signature here (which is unavailable in HDI in any case).
    
    // (transferor's signature — guaranteed by the countersigning session; see note above.)
    
    // (transferee's signature — guaranteed by the countersigning session; see note above.)
    
    // Verify the claim state matches the record (catches a claim that changed since the
    // session opened). Note: hash_entry returns an EntryHash, so claim_state_at_transfer
    // must itself be an EntryHash (Holochain addressing), not the SHA-256 `Hash`.
    let claim_hash = hash_entry(&claim)?;
    if claim_hash != transfer.claim_state_at_transfer {
        return Ok(ValidateCallbackResult::Invalid("Claim state changed since transfer session opened".into()));
    }
    
    Ok(ValidateCallbackResult::Valid)
}
```

> **Engineering note:** Holochain's countersigning preflight mechanism handles the chain-freezing. Both agents must signal readiness before either commits. The session data (containing the agreed TransferEntry content) is built collaboratively and both agents sign the session before it closes. If either agent goes offline or refuses to sign, the session times out and both chains unfreeze without committing. This is the correct atomic behaviour.

---

## DNA 3: Governance DNA

**Membrane:** Open read. Credentialed write — only agents with governance roles can write entries.

**Purpose:** Validator credentials and their status. Warrant records. Mediation outcomes. Community governance decisions. The audit trail of everything that happened in DNA 2 and why.

### ValidatorCredential

```rust
/// Issued by the community's authorised credential issuer
/// Required for joining DNA 2 (Community Registration)
pub struct ValidatorCredential {
    pub credential_id: Hash,
    
    /// The agent receiving this credential
    pub agent: AgentId,
    
    /// Who issued this credential
    pub issued_by: AgentId,
    
    /// What role this credential grants
    pub role: ValidatorRole,
    
    /// How long this credential is valid
    pub valid_from: DateTime,
    pub valid_until: Option<DateTime>,
    
    /// Issuer's signature
    pub issuer_signature: Signature,
    
    /// Current status
    pub status: CredentialStatus,
}

pub enum ValidatorRole {
    /// Household validator — can validate neighbouring claims
    HouseholdValidator,
    
    /// Community elder — elevated weight in assessments
    /// [OPEN DESIGN QUESTION: how is elder weighting implemented?
    /// DNA properties field? Per-entry metadata? Needs design.]
    CommunityElder,
    
    /// Land committee member — can participate in arbitration
    LandCommitteeMember,
    
    /// NGO fieldworker — can issue credentials to community members
    NgoFieldworker { organisation: String },
    
    /// Mediator — can record mediation outcomes
    Mediator,
}

pub enum CredentialStatus {
    Active,
    Suspended { reason: String, since: DateTime },
    Revoked { reason: String, since: DateTime },
    Expired,
}
```

### WarrantEntry

```rust
/// Cryptographic evidence of validator misconduct
/// Self-proving: carries the evidence of the violation, verifiable by any peer
pub struct WarrantEntry {
    pub warrant_id: Hash,
    
    /// The agent being warranted
    pub warranted_agent: AgentId,
    
    /// What this warrant is for
    pub warrant_type: WarrantType,
    
    /// Cryptographic evidence — the proof of violation
    pub evidence: WarrantEvidence,
    
    /// Who issued this warrant
    pub issued_by: AgentId,
    
    pub issued_at: DateTime,
    
    /// Issuer's signature
    pub issuer_signature: Signature,
}

pub enum WarrantType {
    /// Validator changed assessment after committing (commit-reveal mismatch)
    CommitRevealManipulation,
    
    /// Validator submitted a reveal with no corresponding commitment
    UngroundedReveal,
    
    /// Statistical evidence of systematic collusion across multiple validations
    CollisionPattern { validation_ids: Vec<Hash> },
    
    /// Source chain fork detected — attempted double-transfer or record manipulation
    SourceChainFork { fork_evidence: ForkEvidence },
}

pub struct WarrantEvidence {
    /// The specific entries proving the violation
    pub entry_hashes: Vec<Hash>,
    
    /// Plain language description of what the evidence shows
    pub description: String,
    
    /// Any additional cryptographic proof
    pub proof_bytes: Option<Vec<u8>>,
}
```

### MediationRecord

```rust
/// Records the outcome of a community arbitration
pub struct MediationRecord {
    pub mediation_id: Hash,
    
    /// The claim(s) that were in dispute
    pub disputed_claim_ids: Vec<Hash>,
    
    /// Who participated in the mediation
    pub mediators: Vec<AgentId>,
    pub parties: Vec<AgentId>,
    
    /// Outcome
    pub outcome: MediationOutcome,
    
    /// Full narrative record of the mediation process
    pub narrative: String,
    
    /// When mediation was completed
    pub completed_at: DateTime,
    
    /// All mediators' signatures over the outcome
    pub mediator_signatures: Vec<(AgentId, Signature)>,
}

pub enum MediationOutcome {
    /// One claim upheld, the other rejected
    ClaimUpheld { upheld_claim_id: Hash, rejected_claim_id: Hash },
    
    /// Claims divided (partial boundaries to each party)
    BoundarySplit { new_boundaries: Vec<(AgentId, GpsPolygon)> },
    
    /// Agreement reached between parties
    NegotiatedAgreement { terms: String },
    
    /// Referred to external authority (regional court, NGO legal team)
    EscalatedExternally { authority: String, referral_date: DateTime },
    
    /// Mediation failed — no agreement possible
    Failed { reason: String },
}
```

---

## DNA 4: Public Record DNA

**Membrane:** Open read via HTTP Gateway. Write access controlled by governance roles and the outcome of DNA 2 processes.

**Purpose:** The internationally legible public record. Contains certified claims, completed transfers, active dispute flags, and aggregate statistics. Readable by anyone via standard HTTP.

### CertifiedClaim (public record)

```rust
/// The public-facing record of a certified land claim
/// Contains summary information only — not the full validator assessment details
pub struct CertifiedClaim {
    pub public_claim_id: Hash,
    
    /// Registered holder
    pub holder: AgentId,
    
    /// GPS boundary (publicly visible)
    pub boundary: GpsPolygon,
    
    /// Type of rights
    pub claim_type: LandClaimType,
    
    /// Certification summary
    pub certified_at: DateTime,
    pub validator_count: u32,
    pub agreement_percentage: u32,
    
    /// Hash linking to the full harmony record in DNA 3
    /// Allows institutional partners to request the full record if needed
    pub harmony_record_id: Hash,
    
    /// Current status
    pub public_status: PublicClaimStatus,
}

pub enum PublicClaimStatus {
    Certified,
    Transferred { to: AgentId, at: DateTime },
    Disputed { since: DateTime },
    UnderArbitration,
    Superseded { by_claim_id: Hash },
}
```

### HTTP Gateway Queries

The Holochain HTTP Gateway (released 2025) allows external systems to query DNA 4 via standard HTTP requests without running Holochain software.

```
// Retrieve a specific certified claim
GET /dna/public-record/zome/public_queries/fn/get_certified_claim
Body: { "claim_id": "<hash>" }

// List all certified claims in a GPS bounding box
GET /dna/public-record/zome/public_queries/fn/claims_in_area
Body: { "min_lat": -1.5, "max_lat": -0.5, "min_lon": 34.5, "max_lon": 35.5 }

// Get transfer history for a claim
GET /dna/public-record/zome/public_queries/fn/transfer_history
Body: { "claim_id": "<hash>" }

// Get aggregate community statistics
GET /dna/public-record/zome/public_queries/fn/community_stats
Body: { "community_id": "turkana-pilot-001" }
```

> **Engineering note:** The HTTP Gateway requires a Holochain conductor with the DNA deployed to be running and accessible. For the pilot, this is likely a single always-on node operated by the NGO partner or a designated community server. For resilience, multiple always-on nodes can serve the gateway. This is an operational dependency that must be planned for — if the gateway node goes offline, institutional read access is interrupted, even though community records remain intact on participant devices.

---

## GPS Boundary Representation

**[OPEN DESIGN QUESTION]**

This section describes the problem clearly and proposes a starting approach, but the final representation format requires field input from Kenya before being fixed in the DNA.

### The Problem

GPS coordinates provide precise boundary definition. But:

1. GPS accuracy in rural Kenya with consumer-grade phones is typically ±5–10 metres. Two adjacent claims may have overlapping polygons not because of a genuine dispute but because of measurement imprecision.

2. The DNA hash includes the entry type definitions. If the GPS representation format is wrong, changing it means a new DNA and a new network.

3. Pastoralist use rights are not polygons — they are routes, seasonal corridors, and water access points. The polygon assumption is wrong for a significant portion of Turkana's land use patterns.

**The arbitration tsunami problem — overlap tolerance threshold:**

Three independent red-team audits of TerraCommons identified the same operational risk: if every polygon overlap triggers a conflict flag, a 500-household community will generate hundreds of automatic conflicts on day one — the majority caused by GPS measurement imprecision rather than genuine disputes. The arbitration layer would be paralysed before a single real dispute is heard.

The proposed mitigation is an overlap tolerance threshold: overlaps below a defined area (proposed starting point: any overlap where the overlapping region is less than 25 square metres) are not automatically flagged as conflicts. They are recorded as measurement uncertainty notes attached to both claims and reviewed during the community's annual boundary audit process rather than triggering immediate arbitration. Claims where the overlapping region exceeds the tolerance threshold are flagged for arbitration as before.

This threshold is a DNA property — it must be set at deployment and changed only through migration. The 25 square metre figure is a starting proposal; the correct value depends on the typical plot sizes in the target community and requires field input before deployment. An overlap tolerance of 25 square metres in a community where average plots are 200 square metres has a very different meaning than in a community where plots are 2,000 square metres.

**The suppression appeal problem:** Automatic below-threshold suppression treats all small overlaps as measurement artefacts. Some are genuine disputes. A three-metre strip of land — perhaps 12 square metres — might contain a water channel, a path, or a crop that matters significantly to both households. Suppressing this overlap as measurement uncertainty dismisses a real dispute as a technical one. The tolerance threshold must therefore be paired with an explicit appeal mechanism: any household may flag a below-threshold overlap as a genuine dispute, triggering the arbitration process regardless of the area involved. This flag must be exercisable by either party, must be recorded publicly, and must be processed within the standard arbitration timeline. Suppression is a default, not a barrier. The appeal mechanism must be explained to claimants in accessible language at the point of claim registration.

`overlap_tolerance_sq_metres: f32` should be added to `DnaProperties` as a required field with no default — forcing the deploying community to make a conscious, documented choice.

### Proposed Starting Approach

```rust
/// GPS boundary — deliberately flexible to accommodate pilot learnings
pub enum BoundaryRepresentation {
    /// Standard polygon for settled household parcels
    Polygon {
        vertices: Vec<GpsCoordinate>,
        /// Accuracy estimate in metres (from device GPS)
        accuracy_metres: f32,
    },
    
    /// Corridor for movement routes (pastoralist)
    Corridor {
        centreline: Vec<GpsCoordinate>,
        width_metres: f32,
    },
    
    /// Point with radius (water access, resource point)
    PointWithRadius {
        centre: GpsCoordinate,
        radius_metres: f32,
    },
    
    /// Reference to an external survey (where professional survey exists)
    ExternalSurveyReference {
        survey_id: String,
        issuing_authority: String,
        survey_date: DateTime,
    },
    
    /// Narrative description (fallback where GPS is unavailable or meaningless)
    /// [Captures community knowledge in place of coordinates]
    NarrativeDescription {
        description: String,
        reference_landmarks: Vec<String>,
    },
}

pub struct GpsCoordinate {
    pub latitude: f64,
    pub longitude: f64,
    pub altitude_metres: Option<f32>,
    pub accuracy_metres: Option<f32>,
}
```

> **Engineering note:** The NarrativeDescription variant is not a technical fallback — for many pastoralist communities, "the land between the acacia tree where the path meets the dry riverbed and the rock that looks like a sleeping cow" is a more precise and socially meaningful boundary description than a GPS polygon. The system must accommodate both. How narrative descriptions are indexed for spatial queries (the claims_in_area HTTP call above) is an open engineering question.

---

## Holochain Architecture Notes

### Why `must_get_*` in validation

All data retrieval in integrity zome validation callbacks uses `must_get_valid_record`, `must_get_entry`, and `must_get_action` rather than `get` or `get_links`. The `get` family of functions can return `None` if the DHT hasn't fully propagated a record yet — returning a non-deterministic result that could cause the same validation to pass on one node and fail on another. The `must_get_*` functions block until the record is available (or return an error if it cannot be retrieved), making validation deterministic across all nodes. This is non-negotiable in Holochain integrity zomes.

### Coordinator vs. Integrity Zomes

The commit-reveal phase transitions — polling the DHT to check whether all commitments have arrived, deciding when to open the reveal phase, timing out stalled sessions — happen in the coordinator zome, not the integrity zome. Coordinator logic can call `sys_time()`, can call `get_links()`, can have side effects. Integrity logic cannot. The phase transition logic in this document is therefore coordinator pseudocode, even though it references data structures defined in the integrity zome.

### post_commit for Notifications

After a CommitmentEntry or RevealEntry is successfully committed, the `post_commit` callback in the coordinator zome can send remote signals to notify relevant parties. This is appropriate for UX responsiveness — a validator learning immediately that all commitments have arrived and the reveal phase is open. However, protocol state transitions must NOT rely on signals. Signals are fire-and-forget and are lost if the recipient is offline. The protocol state must always be reconstructable from DHT polling alone, with signals serving only as notification shortcuts for online participants.

### Session Timeout Parameter

The `session_timeout_hours` DNA property requires empirical calibration. In Turkana County, connectivity is intermittent. A validator may be genuinely offline for 48–72 hours. Setting the timeout too short means legitimate validations stall constantly. Setting it too long means a single unresponsive validator can block a claim indefinitely.

Proposed pilot approach: start with a 7-day timeout. Monitor the distribution of actual session completion times. Adjust for DNA v2 based on evidence.

**The timeout is an irresolvable tension, not a calibration problem.** It should be stated explicitly: no single timeout value satisfies both requirements simultaneously. A timeout short enough to prevent strategic non-participation as veto will be short enough to fail legitimate validators in low-connectivity conditions. A timeout long enough to accommodate genuine connectivity gaps will be long enough to be exploited as a delay weapon. The governance framework's stall-as-veto protocol — Land Committee review, documented substitution, 30-day challenge window — is the governance layer that manages this tension. The technical timeout is a backstop, not a solution.

**Session hold attack on transfers:** A related but distinct attack applies to countersigning transfer sessions. A malicious party can open a transfer session with a counterparty, wait for the counterparty to commit their signature (freezing their chain), then deliberately go offline before the session can be completed — leaving the counterparty's chain frozen while the attacker arranges a competing session elsewhere. Mitigation: a maximum chain-hold time after which the frozen chain is released if the session completer cannot confirm that both parties are still active within the session window. This requires careful engineering to avoid creating a different race condition. It is a pre-deployment design requirement for transfer sessions specifically.

---

## Known Design Gaps

These are not peripheral questions. Each one needs resolution before the DNA properties can be fixed for the pilot deployment.

**1. Communal and pastoralist land types.** The `LandClaimType::SeasonalUseRight` and `CommunalParcel` variants are placeholders. The actual entry structure for communal rights — who holds them, how they're transferred, how multiple households assert shared rights — requires community design work with Turkana residents before the DNA can be finalised.

A deeper challenge underlies this: pastoralist land use systems function precisely because they remain flexible and negotiated. Seasonal corridors, grazing rights, and movement routes shift with rainfall patterns and inter-community negotiation. Attempting to encode these as static data structures — even flexible ones like `SeasonalUseRight` — risks freezing arrangements that depend on adaptive social process to function. A polygon committed to the DHT in March reflects land use as it was in March. It says nothing about what was negotiated by August.

The honest response to this challenge is a design reorientation for pastoral rights specifically: TerraCommons for pastoral land use should record the *governance process* — who negotiated access with whom, when, with what witnesses, under what conditions — rather than attempting to represent the right itself as a fixed spatial record. This is closer to a mediation ledger than a land register. What is cryptographically committed is the fact of a negotiated agreement and its parties, not a claim that a given polygon belongs to a given household indefinitely. This reorientation requires pastoral rights specialists and Turkana community members to co-design, and is explicitly a pre-deployment requirement, not a post-deployment enhancement.

**2. Elder weighting in assessments.** The Vision document mentions long-term residents being weighted more heavily. This document has not implemented that weighting. The options are: (a) DNA properties include a weighting map by credential type; (b) the agreement analysis algorithm weights by credential type; (c) weighting is handled in governance by requiring elder agreement as a separate threshold. Each has different tamper-evidence properties. This needs an architectural decision.

**3. Cross-community disputes.** Two communities running separate network seeds have no native mechanism for cross-network dispute resolution. This is an acknowledged gap in the current design. Resolution at the pilot stage is likely a governance mechanism (the NGO partner mediates between community networks) rather than a technical one.

**4. Credential issuer governance.** The DNA properties hold a single `authorized_issuer_key`. Real communities may need multiple authorised issuers (multiple elders, different NGO fieldworkers) or may need to rotate the issuer key when a fieldworker leaves. Multi-sig issuing authority or a governance-controlled issuer rotation mechanism is needed. This is a pre-deployment design question.

**4a. Migration-as-erasure attack.** When a network migration is required — because an issuer key must be rotated, thresholds changed, or entry types updated — whoever controls the migration process has structural power over which historical records survive into the new network. A powerful actor who cannot invalidate an inconvenient claim through normal governance channels might be able to do so through a mandatory migration: by influencing who authorises the migration, what records are ported across, and which are classified as "disputed" or "unresolvable" and therefore excluded. This attack is not hypothetical — it is the digital equivalent of what corrupt officials do during paper registry transitions. The migration protocol must be designed with adversarial migration control as an explicit threat model. Minimum requirements: migration must be authorised by a threshold of existing credentialed community members (not just the issuer), every existing certified claim must be ported unless explicitly flagged with cryptographic evidence of the flag's basis, and the migration process must be observable by external parties via the HTTP gateway. This is a pre-deployment governance design requirement, not a future enhancement.

**5. Warrant scope boundary.** The warrant mechanism catches cryptographic protocol violations — commit-reveal mismatches, source chain forks, ungrounded reveals. It does not and cannot catch a validator who follows the protocol correctly while providing false local knowledge. A validator who genuinely claims a boundary is in a different location than it is, produces a valid commitment and a valid reveal. No warrant fires. This is the most common form of corruption in community land validation — not technical fraud, but social fraud — and the warrant system is silent on it. The governance framework, not the technical architecture, must address this. The documents must be explicit that the warrant system addresses protocol integrity only. Reputation systems and governance accountability mechanisms carry the weight for knowledge fraud. Communities and institutional partners must understand this boundary before deployment.

**6. The offline transfer paradox.** Countersigning requires both parties to be online simultaneously in an active session. In rural Kenya with intermittent connectivity, this synchronous requirement is a significant friction point. The risk is not simply that transfers are delayed — it is that users work around the friction by reverting to informal oral or paper agreements, completing the transaction socially and then recording it in TerraCommons after the fact. If this becomes common practice, the digital record is not the authoritative record of land ownership — it is a retroactive documentation of events that happened outside the system, with all the authenticity questions that entails. The TerraCommons record becomes unreliable not because it was corrupted but because it was bypassed.

This is a pre-deployment design requirement for the Turkana context, not a future enhancement. The Turkana pilot cannot deploy with synchronous-only transfer sessions as the sole mechanism — the connectivity conditions will drive bypass behaviour from day one.

The proposed design direction is an asynchronous transfer initiation model: the seller creates and commits a `TransferIntent` entry (a public declaration of intent to transfer to a named buyer, cryptographically signed and time-stamped), which opens a defined acceptance window (e.g. 72 hours) during which the buyer must countersign to complete. This separates initiation and acceptance in time without requiring simultaneous online presence. The countersigning session remains the final commitment mechanism — both parties must still cryptographically commit — but the coordination pressure is reduced from "both online at the same moment" to "both online within the same window."

The security implications require careful engineering review before adoption: a `TransferIntent` entry that is public creates a window during which a competing actor could attempt to interfere, and the interaction between an open TransferIntent and other claim actions on the seller's chain needs precise specification. This design must be completed and reviewed before pilot deployment, not deferred to a later phase.

**7. Evidence document access during validation.** Validators receive evidence fingerprints in the ClaimPackage. But can they request to see the actual documents? The current design leaves household documents in DNA 1, accessible only to the household. A partial solution: the household can grant a specific validator temporary read access via a Holochain capability grant. The mechanics of this need specifying.

**8. Illiterate participant support.** The submission and validation flows assume digital literacy. The facilitator model (a trusted community member assists the household) is a governance solution, but it needs to be reflected in the credential model — a facilitator acting on behalf of a household should have that delegation recorded in the household's own record.

**9. No mechanism for systematic polygon updates when better survey data becomes available.** The current design commits GPS polygons to the DHT at registration. If more accurate spatial data subsequently becomes available — a professional cadastral survey, satellite imagery at higher resolution, or a national mapping programme — there is no mechanism for systematically correcting existing polygon records without triggering a conflict cascade. Every correction to a registered boundary is, from the protocol's perspective, a new boundary claim that overlaps with the existing one. A 500-household community receiving a professional survey would generate up to 500 simultaneous boundary conflicts, overwhelming the arbitration system even if every correction is uncontested.

This is a pre-deployment design requirement. The TR needs a `BoundaryRevision` entry type — a specific path for updating polygon coordinates where both the original claimant and a designated surveying authority (whose credential type is defined in the DNA) co-sign the revision, bypassing the standard conflict detection pathway. Revisions committed via this pathway are recorded as surveyor-confirmed corrections, not as competing claims. The community assembly must authorise the surveying authority credential type before any bulk revision process begins. This design must be specified before the DNA is finalised — adding it post-deployment requires a migration.

**10. DHT sparsity and record orphaning.** The DHT distributes responsibility for storing and serving entries across active network participants. Each entry is held by the DHT neighbourhood — the peers whose agent addresses are closest to the entry's hash. If participation drops significantly — households stop running the app, devices are lost, the NGO exits and the successor organisation is slow to onboard new nodes — DHT neighbourhoods may become undersized. In an extremely sparse network, entries whose neighbourhood has fallen below the minimum redundancy threshold may become temporarily or permanently unreachable, even though they have never been deleted or altered.

This is distinct from the frozen network problem (no new members can join if the issuer key is lost). The DHT sparsity problem means existing certified records could become inaccessible without any actor intending to delete them. The risk is highest in the period immediately after NGO exit, when the transition to community or regional institutional operation may reduce active node count before the successor organisation has stabilised its infrastructure.

Mitigations: the always-on gateway node (required in the exit planning protocol) provides a permanent archival node that participates in the DHT regardless of other participation levels; the exit plan must specify a minimum node count below which the Land Committee must escalate to the successor organisation for technical intervention; and the governance framework's quarterly audit should include a DHT health check — a verification that all certified claims remain reachable — as a standing agenda item. The specific minimum redundancy threshold is an engineering decision requiring specification before deployment.

**ValiChord finding:** Load-testing of ValiChord, the sibling system on the same DHT architecture, makes this risk concrete rather than theoretical. On capable infrastructure, DHT sync latency was a median of ~185 ms but a p90 of ~8.7 s under load; more pointedly, three conductors could not reliably *peer at all* on a stock 2-core cloud runner, and stable multi-node operation required dedicated bootstrap and relay servers rather than default public infrastructure. The implication for a low-connectivity rural deployment is direct: the always-on gateway node cannot be treated as optional resilience. A dedicated, well-connected bootstrap/relay node (or small set of them) is a hard prerequisite for the network to form and hold neighbourhoods at all — and the minimum-node and DHT-health-check thresholds named above should be calibrated against this experience, not against optimistic assumptions drawn from phone-penetration figures.

**11. Cross-DNA membrane proof validation failure.** `[OPEN DESIGN QUESTION]` The four-DNA architecture requires membrane proofs from DNA 1 (Household) to be validated by DNA 2 (Community Registration), and credential status from DNA 3 (Governance) to be checked during DNA 2 validation. The existing code sketch flags this at the relevant point: `[OPEN DESIGN QUESTION: cross-DNA read requires careful bridge design]`. The specific failure mode requiring attention is credential revocation timing: a node whose issuing credential is valid at the moment a cross-DNA interaction begins but is revoked before the interaction completes may produce an entry that is cryptographically committed but whose authority chain is now invalid. In a land rights context this edge case matters — a claim certified by a validator whose credential was revoked mid-session is of uncertain validity, and the protocol needs a defined answer for how to handle it. This requires precise engineering specification before the multi-DNA architecture is built. The interaction between cross-DNA state and validation determinism is the most technically complex part of the architecture and should be the first engineering question addressed when a lead engineer is engaged.

**12. Algorithmic completer DHT Denial-of-Service vulnerability.** The Technical Reference recommends algorithmic DHT completion as the session completer for transfers, on the grounds that an algorithm cannot be bribed. This is correct for the bribery threat model. It is inadequate for the network topology threat model. In Turkana's intermittent-connectivity environment, the DHT neighbourhood responsible for a specific transfer entry's hash may at any given time consist of only two or three active devices. If those devices are offline, belong to the same household, or are controlled by a single actor, the transfer session cannot complete — not because anyone has been bribed, but because the network neighbourhood is too sparse to function. An attacker who understands DHT addressing does not need to corrupt the completer; they need only ensure the relevant neighbourhood is undersized. This is a Denial-of-Service vector requiring no cryptographic attack.

Mitigation direction: algorithmic completion should only be used when the DHT neighbourhood for the relevant entry hash exceeds a defined minimum active-node threshold (an engineering decision, but likely no fewer than 5 independently operated nodes). Below that threshold, the session must fall back to the human Land Committee escalation path. The threshold check must be performed at session initiation, not assumed. This is a pre-deployment design requirement — the session protocol cannot be finalised without specifying the threshold and the fallback behaviour.

**ValiChord finding:** ValiChord's load testing supports this requirement directly. Three conductors failed to peer on a stock cloud runner without dedicated bootstrap/relay infrastructure — confirming that a transfer entry's DHT neighbourhood being too sparse to complete is a routine operating condition in thin networks, not a rare attack. The minimum active-node threshold for algorithmic completion should therefore be set conservatively and checked at session initiation as specified, with the human Land Committee fallback treated as a frequently-used path in early deployment, not an exceptional one.

**13. Link types and queryable collections.** `[OPEN DESIGN QUESTION]` The current architecture defines entry types for all TerraCommons data — claims, transfers, credentials, warrants, certified records — but does not define the link type structures required to make that data queryable. This is a significant architectural gap identified during review with Arthur Brock (Holochain co-founder).

In Holochain there is no global dataset and no centralised query layer. Data retrieval works through two mechanisms: `get`, which requires the hash of the specific entry; and `get_links`, which traverses a graph of links from an anchor to a collection of related entries. Without explicit link definitions, it is not possible to answer queries like "show me all certified claims in this community," "show me all validations by this validator," or "show me all active credentials for this DNA." These are precisely the queries the Public Record DNA (DNA 4) must serve to the HTTP Gateway and the queries the Governance DNA (DNA 3) must serve for validator registry lookups and warrant history.

The lead engineer must define link structures for every collection the system needs to query. For each entry type, the design must specify: what anchors exist (e.g. a community anchor, a validator-identity anchor, a claim-status anchor), what link types connect anchors to entries, and what link tags enable filtering within a collection. The two primary patterns are anchors (a well-known entry from which links radiate to all members of a collection) and paths (hierarchical, colon-segmented structures that distribute load across the DHT to prevent hotspots at scale). For the 500-household pilot, anchor patterns are sufficient. For production-scale deployment, path-based structures with colon segmentation should be considered to prevent DHT hotspots as validator and claim populations grow. This design must be completed before any zome code is written — it determines the fundamental data retrieval architecture of the system.

**14. Structural validation rules — entry permissions.** `[OPEN DESIGN QUESTION]` The architecture describes scientific validation (the human judgment validators apply to claims) in detail, but does not specify the structural validation rules that Holochain's integrity zomes enforce automatically on all entries regardless of their content. These are distinct: structural rules govern the behaviour of the software, not the quality of the science.

Examples of structural validation rules required for TerraCommons: only the household agent may delete their own DNA 1 entries; a validator may not retrospectively alter their own commitment entry after it has been committed; a Land Committee member may not issue a credential to a close family member without a second Land Committee co-signature; a claim entry may not be certified unless the required number of validator reveal entries are present and their hashes match the corresponding commitment entries. Without these rules encoded in integrity zome validation callbacks, the system relies entirely on governance processes and social norms to enforce them — which is insufficient for a tamper-evident land rights system.

The lead engineer must enumerate the permission model for every entry type: who may create it, who may update it, who may delete it, and what preconditions must be satisfied for each action to be valid. This model must be specified in the integrity zome so that violations are rejected at the protocol level, not reported after the fact. The structural validation rules and the link type definitions (Known Design Gap 13) should be designed together — they are both components of the integrity zome and must be mutually consistent.

**15. Device loss and DNA 1 evidence recovery.** DNA 1 is architecturally private — household documents, GPS recordings, witness statements, and scanned materials live on the household's device and nowhere else. This is the privacy guarantee working as intended. It also means that if the device is lost, stolen, destroyed by water, or damaged, the household's private evidence chain is gone.

The certified claim on the shared DHT survives. The evidence that supports it in any dispute, arbitration, or legal proceeding does not. In a rural Kenyan context, device loss is not an edge case — it is a statistical certainty at pilot scale across 500 households over 18–24 months. The poorest households will have the cheapest and most fragile devices and are therefore most exposed to exactly the harm TerraCommons is designed to protect against.

There is no architectural recovery pathway. DNA 1 cannot be reconstructed from the shared network — if it could, the privacy guarantee would be broken. The mitigation is therefore a pre-registration protocol and governance requirement, not a technical fix: every household must be guided, before their first submission, to back up their DNA 1 agent key to a second medium appropriate to their context (a second device, a printed QR code stored safely, a community device store maintained by the NGO, or a sealed written record held by a trusted family member). The backup method must be documented and confirmed by the facilitator as part of the claim submission process.

This does not fully solve the problem — a backup can also be lost, and key recovery in a low-literacy context is genuinely difficult. But the minimum requirement is that households are told, in accessible language, that their device is the sole location of their private evidence, and that they are helped to create at least one backup before their data matters. Pre-registration backup assistance must be a defined facilitator responsibility, not an optional enhancement.

---

## What This Document Does and Doesn't Claim

**This document claims:**

- The four-DNA membrane architecture is sound for the TerraCommons use case
- Countersigning is the correct protocol for land transfers and detects double-selling at the architectural level — making it immediately visible and permanently attributable, not cryptographically impossible
- Commit-reveal is the correct protocol for community validation and makes collusion detectable
- The DNA properties mechanism correctly makes validation thresholds tamper-evident
- The entry types and validation logic sketched here are consistent with Holochain's current framework

**This document does not claim:**

- That any of these Rust structs will compile without modification
- That the open design questions have been answered
- That the GPS boundary representation is final
- That the communal land rights design is sufficient for pastoralist communities
- That the pilot can deploy this DNA without field research and community design work first

The value of this document is in communicating the shape of the system precisely enough that an engineer with Holochain experience can engage with the real design questions — not in providing a specification that can be implemented without conversation.

---

**Companion Documents:**
- *TerraCommons Vision & Architecture* — What the system is and why it matters
- *TerraCommons Governance Framework* — Community governance design and institutional capture resistance
- *TerraCommons Pilot Plan* — Detailed 18-month Kenya implementation

**Contact:** Ceri John — topeuph@gmail.com

**© 2026 Ceri John. Licensed under the Apache License, Version 2.0.**
