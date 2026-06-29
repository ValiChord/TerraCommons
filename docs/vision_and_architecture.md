# TerraCommons
## Vision & Architecture for Community-Rooted Land Rights Infrastructure

**Author:** Ceri John
**Date:** March 2026

**© 2026 Ceri John. Licensed under the Apache License, Version 2.0.**

**Contact:** topeuph@gmail.com

---

## What This Document Is

This is the vision and architecture document for TerraCommons. It explains what TerraCommons is, why it matters, and how it works — in plain language, for anyone who wants to understand the project.

It is the source document from which grant applications, partnership pitches, and technical briefings are drawn. It is not itself a grant application.

A note on the name. *Terra* is Latin for land. A *chord* requires multiple notes sounding together — no single note creates harmony. Secure land rights require multiple community members independently confirming a claim — no single authority grants them. The name is deliberate.

**Companion documents:**
- *TerraCommons Technical Reference* — Illustrative architecture sketches for engineering discussion
- *TerraCommons Governance Framework* — How the system resists corruption and institutional capture
- *TerraCommons Pilot Plan* — The Kenya (Turkana County) implementation design, budget, and timeline

---

## The Problem

### The Scale of It

1.1 billion adults worldwide feel insecure about their property rights. That is 23% of the adult population across 108 countries, according to the Prindex 2024 Global Report — a significant worsening from 19% in 2020. Only 30% of the global population has legally registered land rights. The rest occupy land that cannot be used as collateral, cannot be inherited securely, and can be seized with near-impunity by corrupt officials.

The economic consequences are staggering. Hernando de Soto estimated $20 trillion in "dead capital" — land occupied and used by billions of people that generates no economic value because it cannot be officially mortgaged, transferred, or leveraged. 90% or more of rural land in many African countries is unregistered. 50% of court cases in Kenya are land disputes. One in five people globally paid bribes for land services in the most recent Transparency International survey.

The human consequences are worse. 70% of women in 53 countries own no land at all. Indigenous communities — 6% of the global population — face 25% of all land defender attacks. The Colombia land fraud scandal discovered $17 billion in fraudulent purchases between 2023 and 2025. The Transparency International OREO Index (2025) found that 70% of 42 surveyed countries fall into the high-risk zone of both high perceived corruption and low land data transparency.

The World Bank is currently investing $2.9 billion across 31 countries trying to address this. They have not solved it.

### The Human Reality

Maria is a Colombian farmer. She was displaced in 2000. When she returned in 2015, corrupt officials had sold her land three times over. She spent eight years in courts, paid $15,000 in bribes, and never recovered her land. She lives in an urban slum.

Her story is not exceptional. It is the structure of the problem repeated across 1.1 billion lives.

---

## Why Every Previous Solution Failed

### Traditional Digitisation: Automating Corruption

Rwanda's digital land registry is cited as the most successful example of digital transformation in sub-Saharan Africa. Yet 87% of rural transactions remain unregistered. Moving a corrupt paper system onto computers does not remove the corruption — it just automates it. Officials who altered paper records can alter digital records. Officials who demanded bribes to register land still demand bribes. Digitisation addresses storage and retrieval. It does not address trust, accessibility, or the incentive structures that produce corruption in the first place.

### Blockchain: The Wrong Architecture for the Right Problem

Between 2015 and 2022, at least six countries attempted blockchain land registry pilots with significant international backing. All failed or stalled.

**Honduras (2015–2016).** World Bank funded. Project abandoned. Cause: officials already using their access to the database to steal beachfront properties simply killed the project. Political resistance was fatal before technical work could begin.

**Georgia (2016–2018).** Cited as the blockchain success story. Reality: it worked because Georgia already had a clean, $60 million World Bank-funded system, only 26 years of cadastral history, and relatively low corruption. It used a closed, centralised distributed ledger (Exonum) — the database remained administrated by officials who could still alter it. Transaction costs of $50–200 in gas fees excluded poor farmers. World Bank's own conclusion: "High-quality data was critical to success." Blockchain succeeded where it wasn't needed.

**Sweden (2016–present).** After nine years of pilot work, still not deployed. The system that was already clean gained nothing.

**Ukraine, Ghana, Brazil, India.** Various attempts. Minimal impact.

The pattern is consistent and instructive: blockchain succeeds where data is already clean and governance already trustworthy. It fails precisely where the problem is worst — high corruption, absent governance, unreliable infrastructure.

The architectural reasons are not incidental. They are structural:

**Blockchain requires clean starting data.** Corrupt countries have the worst data. Building an immutable ledger on top of fraudulent historical records makes fraud permanent, not correctable.

**Immutability prevents justice.** When Maria's land was fraudulently registered three times, the correct response is to correct the record. Blockchain makes correction architecturally impossible. Immutability is not a feature in this context — it is a weapon that locks in historical fraud.

**Gas fees exclude the poor.** $50–200 per transaction means the farmers who most need the system cannot use it.

**Global consensus is politically fragile.** A system that requires every participant to agree on every change can be killed by anyone with enough influence. In corrupt environments, the people with enough influence are precisely those who benefit from the current system.

**GDPR non-compliance.** In jurisdictions that recognise data protection rights, immutable public ledgers containing personal land data cannot legally operate.

The deeper problem is this: blockchain is a global consensus architecture applied to a local knowledge problem. Whether Maria owns a specific piece of land in rural Colombia is not a question that benefits from being settled by global cryptographic consensus. It is a question that the people who live next to that land, who watched her family farm it for forty years, are uniquely positioned to answer.

---

## What TerraCommons Is

### The Core Idea

TerraCommons is a system where community members — neighbours, elders, local land administrators — independently confirm each other's land claims, and those confirmations are stored in a way that cannot be corrupted, erased, or overridden by officials who have not participated in the confirmation process.

That is the essence. Everything else — the four-layer architecture, the Holochain infrastructure, the commit-reveal protocol — serves that core function.

TerraCommons does not replace community knowledge with technology. It provides the infrastructure for community knowledge to become cryptographically verifiable, institutionally legible, and permanently recorded.

### The Land Rights Lifecycle

A family's land rights move through TerraCommons in a clear sequence:

**Registration.** A household prepares a claim: GPS boundary coordinates, historical occupation evidence, photographs, witness statements. The claim is stored privately on their own device. Cryptographic fingerprints of every document are generated — change a single bit of any document and the fingerprint changes completely. This proves, later, that the evidence was not altered after submission.

**Community validation.** The claim enters the shared community network. A defined set of neighbouring households — those sharing a boundary or within a specified radius — are asked to independently confirm the claim. Critically, validators cannot see each other's assessments before committing. Each validator privately seals their assessment first. Only after every required validator has committed does the system open a joint session revealing all assessments simultaneously. This is the commit-reveal protocol: it prevents the last validator from adjusting their answer to match the others, and it makes collusion detectable rather than merely possible.

**Boundary arbitration.** Where assessments conflict — validators disagree on where a boundary runs — the conflict is flagged immediately and preserved in full. The community governance layer (elders, a land committee, or a designated mediator) handles arbitration. Technology does not determine who is right. Technology ensures that every step in the arbitration is recorded, attributable, and permanent.

**Claim certification.** When sufficient community validation is achieved, the claim is certified. The certification record contains every validator's independent assessment, the statistical summary of agreement, any disagreement documented in full, and the overall status. Disagreement is not averaged away — it is preserved, because a 7-2 split among validators tells you something important about a claim that a simple "certified" label does not.

**Transfer.** When land changes hands — sale, inheritance, gift — the transaction is countersigned atomically by both parties. Both the transferor's record and the transferee's record receive the identical transfer entry simultaneously, or neither does. It is architecturally impossible for Alice to sell the same land to Bob and to Carol in the same transaction — attempting to do so would require forking Alice's own cryptographic record, a fork that DHT validators detect and record as a permanent, attributable warrant. The double-sell problem, which has enabled vast fraud in corrupt systems, is addressed at the protocol level rather than by trusting officials.

**Dispute resolution.** Where competing claims arise — land occupied by a family for decades, and a more recent claim backed by allegedly official documents — TerraCommons preserves both claims with their full evidence chains and flags the conflict for arbitration. The system does not determine who wins. It ensures the evidence is permanent, attributable, and accessible to whatever legitimate authority — community, NGO, international institution, eventually government — adjudicates the dispute.

**Institutional recognition.** The public record layer is readable by anyone with an internet connection — World Bank officials, NGO fieldworkers, journalists, lawyers — without requiring them to run TerraCommons software. This is a designed property of the architecture: the HTTP Gateway that exposes the public record is standard tooling, not a custom integration. Creating institutional legibility for community-validated land rights does not require institutional participation in the validation process.

### What Makes It Different

Four things distinguish TerraCommons from every previous attempt.

**1. Community knowledge as the source of truth**

Every previous system — paper, digital, blockchain — has sought to bring existing official data into a new format. TerraCommons inverts this. Official records are often absent, corrupted, or fabricated. Community knowledge — who has farmed which land, where boundaries run, which families have occupied which plots for generations — is, in many contexts, the best available evidence of land occupation and use. It is distributed across many witnesses rather than concentrated in a single corruptible record.

This does not make community knowledge immune to manipulation. In divided communities — split by ethnicity, clan, or recent conflict — community testimony can be weaponised rather than reliable. Colonial-era land alienation has distorted oral histories across sub-Saharan Africa. Coordinated false testimony is possible and documented in Kenyan land adjudication processes. TerraCommons treats community knowledge as the starting point, not the final word. The technical architecture makes community assessments cryptographically attributable and the process transparent. The governance framework — not the technology — is where the risk of community knowledge capture is managed. A community with deep internal conflict, a captured land committee, or systematic collusion among validators will produce a corrupted TerraCommons record as surely as it produces corrupted paper records. The architecture limits the damage; it cannot prevent it entirely.

TerraCommons does not need clean starting data. It treats community knowledge as the primary source of truth and provides infrastructure to formalise, record, and protect it. A village where 90% of land is unregistered is not a problem for TerraCommons — it is the starting condition.

**2. Countersigning: the structural solution to double-selling**

Previous systems have tried to prevent double-selling through timestamps ("first-registered wins") or through administrator oversight. Both fail in corrupt environments: timestamps can be falsified, and administrators can be bribed.

TerraCommons uses Holochain's countersigning mechanism. A transfer atomically updates both parties' cryptographic records simultaneously, or updates neither. The transaction is mathematically self-consistent: it references the specific state of both records at the moment of transfer. Any attempt by Alice to transfer the same land twice would require her record to fork — to have two different histories from the same point — and that fork is permanently recorded on the DHT as a cryptographically attributable warrant. The warrant cannot be erased, and any participant can retrieve it. The TerraCommons application implements a warrant-gate (blocking further protocol participation by the warranted agent) and a scheduled scan (alerting connected clients when a new warrant appears). These are application-layer mechanisms built on Holochain primitives, but their effect is the same: a detected fork routes immediately to governance arbitration and cannot be quietly buried.

**3. Commit-reveal: making collusion detectable**

A validation system where neighbours confirm each other's claims has an obvious vulnerability: neighbours can coordinate fraudulent confirmations. "You confirm my claim, I'll confirm yours."

TerraCommons addresses this through the commit-reveal protocol. Every validator privately seals a cryptographic hash of their assessment before the joint session opens. They cannot change their assessment after seeing others' assessments, because the hash of their original assessment was recorded before it was revealed. They cannot pretend they said something different from what they committed to, because the commitment is cryptographically provable. Coordinating a fraudulent confirmation requires all colluding validators to agree on their false assessments before any of them have committed — which is harder, creates evidence of coordination, and is detectable through statistical analysis of assessment patterns over time.

Collusion cannot be fully prevented. TerraCommons makes it significantly harder and, crucially, detectable.

**4. Multi-layer legitimacy: technology enables, institutions recognise, governments follow**

TerraCommons does not depend on government recognition to work. A community can use it to record and protect their land rights regardless of whether any official acknowledges the records. But the strategic aim is government recognition, and the architecture is designed with that trajectory in mind.

Community validates through TerraCommons (bottom-up, corruption-resistant). International institutions recognise the methodology as sound — World Bank, Prindex, Landesa confirm that the validation process follows international best practices and produces reliable evidence. Government recognition may follow through the accumulated weight of evidence, shifts in political will, or legal reforms — but it may not.

This third layer is not guaranteed, and the documents should not pretend otherwise. The Endorois case before the African Court on Human and Peoples' Rights established that community land rights can be recognised in international law — and demonstrated that recognition and enforcement are entirely different problems. A Kenyan government official with a paper title deed will not defer to a TerraCommons record because an international institution endorsed the methodology.

**Two legitimacy modes.** Where the government is not an adversary, TerraCommons runs in *recognition mode* — open record, engagement with the World Bank and national land frameworks, aiming at formal recognition. Where the government *is* an active adversary, it runs in *legal-empowerment mode* — restricted gateway, partnering with legal-empowerment and human-rights bodies to strengthen litigation and advocacy rather than seeking recognition. Every institutional conversation must state honestly which mode a deployment is in. The full treatment — and why the two cannot be optimised at once — is the Governance Framework's **Non-Negotiable 8**.

TerraCommons' honest value proposition, even without legal recognition, is a tamper-evident, internationally legible record of a community's claims and evidence — more durable than oral history, more credible than informal paper, more accessible to advocates and courts. It does not resolve disputes; it changes the evidentiary landscape in which they are fought. That is not merely a consolation prize: documented, timestamped, multi-witness evidence is how a great deal of land restitution and human-rights litigation is actually won. Better evidence changes who wins — but it must be neither oversold nor undersold.

Three honest warnings must reach communities *before* they register, and the Governance Framework commits to each formally: a "Certified" stamp can make a *fraudulent* claim **more** durable than an oral one, because it wears the appearance of consensus (**Non-Negotiable 4**); a certification that proves legally worthless in the local jurisdiction can leave a community worse off than never formalising (**Non-Negotiable 4**); and no record is a shield against a government's statutory **compulsory-acquisition** powers — it can strengthen a compensation or challenge claim, but gives no veto (**Non-Negotiable 11**).

This strategy is not optimistic about governments. It is designed to work even if the government layer never arrives — while positioning community land rights for eventual recognition when circumstances permit.

---

## The Hard/Soft Distinction

This framework, developed in consultation with Paul D'Aoust (Documentation and Developer Community Lead, Holochain Foundation), is the most important conceptual tool in TerraCommons' design.

### Hard: What Technology Can Guarantee

**Provenance.** Every validation is cryptographically signed by the validator. It is impossible to forge a signature or attribute a validation to someone who did not make it. Every claim, every assessment, every transfer is permanently linked to the specific person who authored it.

**Data integrity.** Every document submitted as evidence is fingerprinted using SHA-256. The fingerprint changes completely if any bit of the document changes. Anyone can verify, at any time, that the evidence has not been modified since it was submitted.

**Audit trail.** Every action — claim submission, validator commitment, reveal, transfer, governance decision — is timestamped, signed, and stored in an append-only cryptographic record. The complete history of any land claim is reconstructable at any time by any participant.

**Transfer atomicity.** A land transfer either updates both parties' records or neither's. There is no intermediate state in which the seller's record shows a transfer but the buyer's does not, or vice versa.

**Validator accountability.** If a validator signs off on something that demonstrably fails the validation rules — signs a commitment that contradicts the evidence they were given, or signs a reveal that does not match their original commitment — any peer can issue a warrant against them. The warrant is self-proving, cryptographically verifiable, and permanently recorded against the validator's identity. The network can gate that validator's future participation.

### Soft: What Requires Human Judgment

**Boundary claims.** Where exactly a boundary runs is inherently a matter of human knowledge and social agreement. TerraCommons provides the infrastructure for transparent, accountable community decision-making about boundaries. It does not determine where boundaries are.

**Historical ownership.** Whether a family has occupied land for twenty years, or forty, or whether their occupation pre-dates a competing claim — these are questions of fact that require testimony, documentary evidence, and judgment. TerraCommons ensures that the evidence is recorded, the testimony is attributable, and the judgment process is transparent. It does not make the judgment.

**Competing claims.** When two parties claim the same land with apparently valid evidence, TerraCommons preserves both claims and all evidence for adjudication. It does not determine who is right.

**Fraud at source.** A validator who follows the protocol correctly while testifying falsely commits no *cryptographic* violation — the warrant system is silent on it. This "social fraud" is the hardest problem in the system and the one technology cannot solve; it is managed by governance — reputation over time, active oversight, conflict-of-interest rules — not by the architecture. The Governance Framework's "The Social Fraud Problem" section addresses it in full.

### Why This Matters

This distinction prevents TerraCommons from making promises it cannot keep. Land tenure is fundamentally a social and political problem. Technology can make social processes more transparent, more accountable, and more resistant to corruption — but it cannot replace the social processes themselves. The architecture is designed around this reality. Every design decision that follows from this section is honest about what the system is doing and what it is not doing.

---

## The Four-Layer Architecture

TerraCommons is structured as four distinct Holochain applications (DNAs) rather than a single app. Each DNA has its own *membrane* — the access boundary that controls who can join its network and what data is shared within it. The four-layer architecture is not a design preference. It is the architectural mechanism that makes data locality a guarantee rather than a policy commitment.

Think of each layer as a separate room with its own lock. The Household layer is a private room only the family can enter. The Community Registration layer is a meeting room that credentialed community members can enter. The Governance layer is a public notice board that community members can write on and anyone can read. The Public Record layer is a window anyone in the world can look through.

A further property of the architecture: even where two communities use identical TerraCommons code, a *network seed* baked into each community's DNA makes their records genuinely separate. Turkana County's land registry and Samburu County's land registry are distinct networks that will never merge — unless intentionally bridged by agents participating in both.

### Layer 1: Household DNA (Private)

This is the family's own space. It lives on their device and nowhere else. It stores original documents — GPS coordinates, photographs, oral history recordings, inherited documents, witness statements — along with metadata and notes they are not yet ready to share publicly.

This layer has no membrane check for outsiders because outsiders cannot reach it. It is private by architecture. The family retains full control of their data. Nothing leaves this layer without an explicit action by the family — either submitting a claim for community validation, or participating in a transfer.

When a claim is submitted, the family does not upload their raw documents. They submit a structured claim package: GPS boundary coordinates, a hash-chain of evidence, and structured metadata. The original documents remain on their own device. The cryptographic fingerprints go to the shared network. Anyone can verify that the evidence matches the fingerprints — but only the family holds the documents themselves.

This is GDPR compliance by architecture. The right to erasure is preserved because sensitive personal documents never enter the shared network in the first place.

### Layer 2: Community Registration DNA (Credentialed Membrane)

This is where community validation happens. It is shared among credentialed community members: registered households, designated validators, land committee members, NGO fieldworkers with assigned roles.

The membrane controls access in two stages. Before a device even attempts to join the network, it runs a format check on its joining credentials — ensuring they are structurally valid and signed by the right issuing key. After joining, when peers validate the new participant's credentials against the distributed record, they check: does the issuing authority exist on the network? Is their authority credential valid? Is their signature over this specific agent's joining key cryptographically sound?

The identity of whoever is authorised to issue joining credentials — a village elder, an NGO fieldworker, a land committee — is baked into the DNA's properties at deployment time. These properties are part of the DNA hash. Changing the authorised issuer means creating a new DNA and a new network. The community's membership rules are as tamper-evident as its land records.

This layer is where commit-reveal validation happens. When a claim enters the validation queue, the protocol:

1. Identifies the required set of validators — neighbouring households, registered elders, or some combination defined by the community's rules
2. Each validator independently prepares their assessment and commits a cryptographic hash — they cannot change their assessment after this point
3. Once all required validators have committed, the joint session opens and all assessments are revealed simultaneously
4. The system compares commitments against reveals — any mismatch is evidence of attempted manipulation
5. Assessments are aggregated. Agreements are recorded. Disagreements are preserved in full, not averaged.

The validation thresholds — how many validators are required, what agreement percentage constitutes certification, at what point a claim is flagged for arbitration — are also baked into the DNA properties. 5 required validators and a 70% agreement threshold are not runtime settings that can be quietly changed. They are part of the DNA hash. A community that wants different thresholds deploys a different network.

### Layer 3: Governance DNA (Community-Wide, Open Read)

This layer records the community's collective decision-making. It is readable by anyone but writable only by those with governance roles — land committee members, designated elders, the NGO partner's fieldworkers.

It holds: validator credentials and their status, mediation records and outcomes, community governance decisions, escalation logs, and the validation record for the community's overall methodology (for institutional recognition purposes).

The warrant system operates here. When the network detects a validator whose commit does not match their reveal — attempted manipulation of the commit-reveal protocol — a warrant is issued and recorded against their identity in this layer. The warrant is self-proving: it carries the cryptographic evidence of the contradiction. Any peer can verify it without trusting the person who issued it. The community land committee can review warrants and decide whether to revoke a validator's credentials.

### Layer 4: Public Record DNA (Open Read, HTTP Gateway)

This is what the world sees. It contains certified land claims, transfer records, dispute flags, and aggregate statistics — but not the detailed validator assessments, personal testimony, or household evidence that lives in the Community Registration layer.

It is readable by anyone with an internet connection via the HTTP Gateway. A World Bank official in Washington, a Landesa fieldworker in Nairobi, a journalist investigating land fraud, a lawyer building a restitution case — all of them can query this layer through standard HTTP requests. They do not need to run TerraCommons software. They do not need to understand Holochain. They make an HTTP request and receive structured data.

This is the institutional legibility layer. It is how TerraCommons creates the evidence base that international institutions can recognise and that eventually creates leverage for government recognition. It is not a compromise of the community's data sovereignty — the detailed evidence chain lives in the Community Registration layer under the community's control. The Public Record layer contains only what the community has chosen to make publicly visible.

---

## How Validation and Transfer Work

The two mechanisms at the heart of the Community Registration layer are the **commit-reveal validation protocol** (neighbours seal their assessments before any are revealed, so no one can adjust to match the others) and the **countersigned transfer protocol** (a sale or inheritance atomically signs both parties' records, so a double-sell becomes a detectable, attributable fork rather than a quiet alteration). Both are summarised under "What Makes It Different" above.

The full step-by-step walk-through — commit and reveal phases, agreement analysis, the session-completer decision, fork detection, and inheritance handling — lives in the *Technical Reference*, and is not repeated here.

---

## Open Design Questions

Honest acknowledgment of open problems is more credible than silence — funders, NGO partners, and community leaders will ask about these. They are listed here in brief; the full treatment, with engineering specifics, lives in the *Technical Reference*'s **Known Design Gaps** register.

1. **Fraudulent historical records.** TerraCommons creates a parallel community-validated record; it does not erase official fraud. It supplies the ammunition for a challenge through legal/political channels — it cannot fire it.
2. **Illiterate participants.** Handled through trusted facilitators acting on a household's recorded authorisation — but the facilitator becomes a new dependency and corruption-risk location that must be governed.
3. **Disputes between communities.** Separate community networks have no native cross-network arbitration; this needs regional governance (probably NGO-coordinated), not a technical mechanism.
4. **NGO withdrawal.** Records live on community devices and the DHT, not NGO servers. This is more than resilience: with no single party holding the records, there is no central store to subpoena, seize, defund, or coerce — the capturable custodian that the realistic alternatives (a hosted database, a notarised store) quietly reintroduce. An always-on node materially increases durability.
5. **Contested GPS coordinates.** Overlapping polygons trigger a conflict flag for governance arbitration; utility in boundary disputes depends on the governance layer, not the technology.
6. **Customary, communal, and pastoral rights.** The initial design assumes household parcels; communal and seasonal-use rights need additional entry types and a governance-ledger model — a Phase 2 design problem (see the pastoral reorientation in the Technical Reference).
7. **Credential issuance at scale.** Whoever issues credentials controls participation; a single issuer recreates central control. Requires a credential-governance model (multi-signature issuing, revocation, oversight) before deployment.
8. **Connectivity floor.** The commit-reveal session stalls if a required validator is offline past the session window. ValiChord's load testing indicates the binding requirement includes at least one always-on, well-connected node for peer discovery — dedicated bootstrap/relay infrastructure is a precondition, not optional resilience (detail in the Technical Reference, Gaps 10 & 12).
9. **Minimum parcel size for GPS.** Consumer GPS error (3–10 m) makes small homestead plots overlap by default, flooding overlap detection with false positives. Below a minimum parcel size, GPS polygons must be replaced by an alternative documentation approach with its own entry type — specified before deployment.

---

## The Pilot: Turkana County, Kenya

### Why Kenya, and Why Turkana

Kenya is one of the most acute land rights crises in the world. 50% of court cases are land disputes. The government is documented as tolerant of innovation pilots while simultaneously failing to resolve the underlying problems through official channels. The World Bank has active investment. Landesa has operational presence and 50+ years of land rights experience. Mobile phone penetration is approximately 80%.

Turkana County specifically: predominantly pastoralist, which presents the communal land rights challenge described above. Active World Bank engagement. Documented corruption in land administration. Genuine community need.

**A critical scope limitation for the Turkana pilot must be stated explicitly.** TerraCommons in its current architectural form is designed for individual parcel registration. The Turkana pilot therefore begins with settled household parcels only — the portion of Turkana land use that the current architecture can handle — and explicitly defers pastoral and communal rights until the governance-ledger approach has been co-designed with Turkana community members and pastoral rights specialists.

A successful settled-parcel pilot in Turkana demonstrates that TerraCommons works in a context with genuine land rights need, documented institutional failures, and low-connectivity challenges. It does not demonstrate that TerraCommons can address pastoralist tenure. That demonstration requires Phase 2, co-designed with the community and pastoral rights specialists.

### Two-Track Pilot Structure

The Turkana pilot runs as two parallel tracks from day one.

**Track A — Settled parcel registration (18 months, primary track):** The settled-parcel architecture described throughout this document. 500 households. GPS boundary claims, commit-reveal validation, countersigned transfers.

**Track B — Pastoral governance ledger (24 months, co-design track):** A parallel process, beginning at Month 1, to co-design the governance-ledger architecture for pastoralist tenure with Turkana community members, pastoralist rights specialists, and the pastoralists themselves. Track B begins co-design at Month 1. The co-design process runs alongside Track A deployment so that by Month 18, Track B has a documented architecture, a community-ratified governance design, and a tested approach.

**Track B must have its own budget line.** If Track B is not visibly progressing by Month 12, the pilot has failed one of its primary objectives regardless of Track A's parcel certification rates.

### Four Phases

A prerequisite before the pilot clock starts: pre-pilot software development. No TerraCommons code has been written, compiled, or tested. Building the application is substantial engineering work that cannot be credibly delivered within a pilot that also requires community entry, governance design, facilitator training, and deployment. The honest sequencing is: engage a lead engineer; resolve the Known Design Gaps; build and test an MVP; only then begin the pilot phases below.

**Phase 1 (Months 1–3):** Partnership and community foundation. NGO partner confirmed. Community entry. Village selection with consent. Baseline survey. Governance design with community leaders.

**Phase 2 (Months 4–9):** Application development and training. TerraCommons application built for the specific community context. Community facilitator training. Household device setup. Offline functionality testing.

**Phase 3 (Months 10–15):** Deployment and validation. Households begin registering claims. First transfers recorded. First disputes processed through governance. Monitoring for collusion patterns, connectivity problems, facilitator dependency issues.

**Phase 4 (Months 16–18):** Evaluation and scaling preparation. Rigorous assessment against pre-defined success metrics. Academic partner produces independent evaluation.

### What Success Looks Like

**Primary metrics:**
- 80%+ of pilot households have certified land claims
- 70%+ of land disputes processed fairly through the governance layer
- 90%+ participation rate among the poorest households
- 50%+ of registered claims held by women
- Zero detected protocol violations
- Track B pastoral co-design visibly progressing by Month 12

**System performance:**
- 99%+ application uptime
- Session completion rate > 85%
- Median time from claim submission to certification < 30 days

---

## The Competitive Landscape

TerraCommons is complementary to, not competitive with, several existing approaches.

**Traditional land titling programmes** work top-down from government systems. TerraCommons works bottom-up from community knowledge. They are not alternatives — a community using TerraCommons that later achieves formal government recognition would migrate their records into the official system, not abandon them.

**Cadastral mapping initiatives** establish official boundaries using survey technology. TerraCommons uses GPS coordinates as a practical approximation for communities where professional surveys are economically impossible.

**Legal empowerment organisations** (Landesa, RRI, LandMark) conduct advocacy, litigation, and policy reform. TerraCommons provides the evidence infrastructure that makes legal empowerment more effective.

The specific gap TerraCommons fills: if international institutions want to recognise community land rights, if legal advocates want to challenge fraudulent official records, if NGOs want evidence that their land rights work has produced durable outcomes — **does a reliable, tamper-evident, institutionally legible record of community land rights exist?** No existing system provides this for communities in high-corruption, low-connectivity environments. TerraCommons does.

---

## Why This Matters

1.1 billion people cannot secure their land. $20 trillion sits as dead capital. Women are systematically excluded from land ownership across 53 countries. Indigenous communities face violence to defend rights that no legal system adequately protects.

The solutions that have been tried have failed in every high-need environment. They failed not because the technology was wrong but because they misunderstood the problem. Land rights are not primarily a data storage problem. They are a trust problem. The data needs to be trustworthy, yes — but trust is produced by communities, not by ledgers.

TerraCommons is designed from the ground up for the actual problem: creating infrastructure that makes community knowledge trustworthy, verifiable, and institutionally legible, in environments where official systems have failed or been captured by those they were supposed to constrain.

The technology is proven at the level of components — Holochain's core primitives are production-implemented and function as described. The TerraCommons-specific assembly of those components has not been built or tested. The architectural approach has been validated by Arthur Brock (Holochain co-founder) and Paul D'Aoust (Holochain Foundation); this confirms the approach is sound, not that the system is complete.

The question is no longer whether this problem can be technically addressed. The question is whether the right people will come together to address it.

---

## Where We Are Now

### What Exists

**A validated concept.** The architecture described in this document has been developed through intensive engagement with Holochain's own documentation and technical community. The core mechanisms — countersigning for transfers, commit-reveal for boundary validation, membrane proofs for community credentials, DNA properties for tamper-evident thresholds — are proven components of the Holochain framework, not novel inventions. The innovation is their combination and application to this specific problem.

**Documented research.** Three substantial documents establish the evidence base: a 14,000-word research synthesis covering the global land registry crisis, blockchain failures, and Holochain's technical readiness; a 10,000-word comparative analysis; and a 12,000-word pilot implementation plan for Kenya. All sources are from 2024–2026 and independently verifiable.

**Institutional landscape mapping.** The relevant funding landscape (World Bank Land 2030, Gates Foundation, USAID, Rights and Resources Initiative), implementation partners (Landesa, Transparency International Kenya, Kenya Land Alliance), and research partners have been identified and their current activities mapped.

### What Doesn't Exist Yet

**No working software.** The architecture described here is design intent, not functional code. Nothing has been compiled, tested, or deployed.

**No confirmed partnerships.** Institutional outreach has begun but no commitments exist.

**No empirical evidence from the field.** The pilot design is based on available evidence about Kenya's land tenure situation, connectivity conditions, and community structures. Actual deployment will surface constraints not visible from desk research.

**No confirmed team.** The project needs a lead engineer with Holochain experience, an NGO partner with implementation capacity, an academic partner for independent evaluation, and community facilitators on the ground.

---

**Companion Documents:**
- [Technical Reference](technical_reference.md) — Architecture sketches for engineering discussion
- [Governance Framework](governance_framework.md) — Community governance design and institutional capture resistance

**Contact:** Ceri John — topeuph@gmail.com

**© 2026 Ceri John. Licensed under the Apache License, Version 2.0.**
