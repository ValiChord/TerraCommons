# TerraCommons Governance Framework
## How Community Land Rights Infrastructure Resists Capture, Corruption, and Collapse

**Author:** Ceri John
**Date:** March 2026

**© 2026 Ceri John. Licensed under the Apache License, Version 2.0.**


**Contact:** topeuph@gmail.com

---

## What This Document Is

This is the governance framework for TerraCommons. It describes how the system is structured to resist the social and political failure modes that have destroyed every previous community land rights initiative — not merely the technical ones.

The technical architecture is described in the companion *Technical Reference*. This document addresses what the technical architecture cannot: who holds power, how power is constrained, what happens when power is abused, and what the system does when the institutions it depends on fail or leave.

The central argument of this framework is that TerraCommons will not fail technically. It will fail, if it fails, because a land committee is captured by a powerful clan, because an NGO embeds institutional interests into the system's rules at deployment, because a mandatory network migration is used to erase inconvenient historical claims, or because validators follow the protocol correctly while lying about what they know. Every governance mechanism in this document is designed around one of those specific failure modes.

**Companion documents:**
- *TerraCommons Vision & Architecture* — What the system is and why it matters
- *TerraCommons Technical Reference* — Illustrative architecture sketches for engineering discussion
- *TerraCommons Pilot Plan* — The Kenya (Turkana County) implementation design

---

## The Community Assembly

The community assembly is the ultimate constitutional authority in TerraCommons. It ratifies deployment parameters, authorises DNA migrations, approves sustainability models, selects operational modes, and provides the final check on Land Committee decisions. It is invoked throughout this document as the backstop against every serious failure mode.

It must therefore be defined operationally, not just invoked rhetorically. A community assembly that has no quorum requirement, no convening process, and no defined membership can be manipulated by anyone willing to control who shows up. This section establishes the minimum requirements that make a community assembly legitimate.

**Membership:**

Any adult member of the community with a household registered — or eligible to be registered — in the Community Registration DNA is entitled to participate in a community assembly. Membership is not restricted to Land Committee members, credential holders, or household heads. The community defines what "adult member" means in their cultural context, documented before the first assembly is convened.

The question of seasonal and absent members must be resolved before deployment: in communities with significant seasonal movement (including pastoralist communities), some legitimate community members will be absent during any given assembly. The community must define, in writing before deployment, whether seasonal residents are members for assembly purposes and, if so, what provision exists for their participation or proxy representation. A community assembly that only ever convenes when the pastoralists are away is not representative.

**Quorum:**

A quorum of at least 40% of registered adult community members must be present for a constitutional decision to be valid. For routine governance decisions, the Land Committee operates by its own majority rules. Constitutional decisions — DNA migration, issuer key changes, validation threshold changes, sustainability model adoption, operational mode changes — require the community assembly quorum. A meeting that does not achieve quorum may deliberate and advise but may not make constitutional decisions.

The specific quorum threshold (40% is the minimum — communities may set a higher bar) is agreed in writing before deployment and recorded in the Governance DNA. It cannot be changed without itself meeting the quorum threshold.

**Convening:**

A community assembly must be called with a minimum of 14 days' notice, communicated through whatever channels reliably reach the broadest community membership — which may include village criers, mobile phone messages, notices at community gathering points, and church or mosque announcements. The notice must state the specific decisions to be made. An assembly convened to decide one question cannot make constitutional decisions on a different question not stated in the notice.

The Land Committee may call an assembly. Any group of community members representing at least 15% of registered households may also call an assembly, without Land Committee approval. The 15% threshold prevents frivolous assemblies while ensuring that a captured Land Committee cannot block a community from exercising its constitutional authority.

**Voting:**

Voting method must be appropriate to the community's literacy and technology context. Secret ballot, show of hands, or other culturally appropriate methods are all acceptable — the requirement is that the method is agreed before the vote, applied consistently, and produces a verifiable count. In low-literacy contexts where written ballots are impractical, a standing vote with independent observer counting is the minimum acceptable standard.

All votes are recorded in the Governance DNA by the Land Committee chair within 7 days of the assembly, with: date, quorum count, the specific decision put to a vote, the vote outcome (numbers for and against), and the names of any Land Committee members or independent observer present. The record is publicly readable via the HTTP Gateway.

**Legitimacy threshold:**

An assembly whose convening process was not followed — inadequate notice, quorum not verified, decision not on the stated agenda — does not produce a valid constitutional decision. Any community member may challenge the legitimacy of an assembly decision within 30 days by filing a complaint with the independent observer. The observer reviews whether the convening process was followed and, if not, declares the decision void. A voided constitutional decision requires a new assembly convened in compliance with these requirements.

---

## The Problem with Governance

### Why Land Governance Frameworks Fail

The history of land rights governance in sub-Saharan Africa is a history of frameworks that looked sound on paper and failed in practice — not because the principles were wrong but because the institutions designed to implement them were captured, underfunded, or simply abandoned when external support ended.

Kenya's land adjudication process under the Land Adjudication Act was designed to formalise customary land rights through community-based adjudication committees. In practice, adjudication committees were routinely captured by local elites, customary rights of women and minorities were systematically excluded, and the process entrenched colonial-era distortions rather than resolving them. A governance framework that assumes good-faith participants will produce just outcomes is not a governance framework. It is a wishlist.

TerraCommons is not immune to this history. The commit-reveal protocol makes validator collusion harder to execute. It does not prevent a validator from following the protocol correctly while providing false testimony about a boundary they know perfectly well. The credentialed membrane controls who can join the network. It does not prevent the person who controls credential issuance from granting access only to allies. The DNA properties make thresholds tamper-evident once deployed. They do not prevent the deployment itself from being shaped by whoever had influence over the deployer.

This framework does not pretend these risks can be eliminated. It designs governance structures that make corruption more difficult, more visible, and more costly — while being honest about what remains beyond the governance system's reach.

### Ostrom's Design Principles

Elinor Ostrom's research on successful commons governance — the work for which she received the Nobel Prize in Economic Sciences in 2009 — identified eight design principles present in commons institutions that endure over decades. They are worth stating directly, because TerraCommons is a commons governance infrastructure and these principles are not theoretical — they describe what has actually worked in practice across diverse cultural and geographic contexts.

**1. Clearly defined boundaries.** Who is entitled to use the commons must be clearly defined. For TerraCommons: who can register claims, who can validate, who can participate in governance.

**2. Rules match local conditions.** Governing rules must fit local social and ecological conditions. For TerraCommons: the DNA's validation thresholds, validator requirements, and credential structures must be determined by the community, not imposed by an external deployer.

**3. Collective choice arrangements.** Those affected by the rules can participate in modifying them. For TerraCommons: the community must have a genuine mechanism for changing governance rules, not just a theoretical one.

**4. Monitoring.** Compliance must be actively monitored, by members of the community or by people accountable to them. For TerraCommons: validator behaviour must be observable and the warrant mechanism must be practically usable.

**5. Graduated sanctions.** Violators face graduated sanctions proportional to the seriousness of the offence. For TerraCommons: a first-time validator error is treated differently from systematic fraud.

**6. Conflict resolution mechanisms.** Fast, low-cost local arenas for dispute resolution must exist. For TerraCommons: the arbitration layer must be accessible to the people who need it, not only to those with resources to navigate formal processes.

**7. Recognition of rights to organise.** External governmental authorities must recognise the community's right to create its own institutions. For TerraCommons: the system must not depend on government recognition to function, but must not position itself as a threat to legitimate governmental authority.

**8. Nested enterprises.** For larger systems, governance activities are organised in multiple nested layers. For TerraCommons: community → regional → international is the correct nesting, with each layer handling what it is best positioned to handle.

These principles shape every section that follows.

---

## The Social Fraud Problem

This is the central governance challenge and must be stated plainly before anything else.

TerraCommons' technical architecture — commit-reveal, countersigning, warrants — addresses protocol fraud: a validator who changes their assessment after committing, a seller who attempts to transfer the same land twice, a participant who creates a false record. These are detectable cryptographically and the warrant mechanism handles them.

They are not the most common form of land rights corruption. The most common form is a validator who follows the protocol correctly while testifying falsely about what they know. A validator who commits "Agree" to a fraudulent boundary claim, and reveals "Agree" at the reveal phase, having been paid to do so, has committed no cryptographic violation. The protocol is satisfied. The warrant system is silent. The claim is certified.

This is not a design flaw in TerraCommons. It is the boundary of what any technical system can address. No cryptographic mechanism can verify that a validator's testimony reflects their genuine knowledge rather than a fabricated account shaped by a payment, a threat, or a social obligation. Holochain's architecture makes testimony *attributable* and *permanent* — it cannot make it *true*.

The governance framework carries the weight that the technical architecture cannot. Everything that follows from this section is, in one way or another, an answer to the social fraud problem.

**ValiChord finding:** ValiChord, the sibling system built on the same commit-reveal core, offers one additional structural answer beyond reputation-over-time. Its *two-round protocol* (see Technical Reference) seals each validator's independent knowledge of the boundary separately from their verdict on the claim, and publishes both, side by side, in the same record. A validator who certifies a boundary their own sealed account contradicts is then exposed at the level of the individual claim — immediately and permanently — rather than only through a statistical pattern that, as this framework acknowledges, the pilot is too small to produce. This does not detect a whole-community collusion in which everyone's sealed knowledge is also false; nothing in the architecture can. But it converts the most common single-validator case — a bribed neighbour certifying against what they actually know — from invisible to self-evident. For contested or high-stakes claims it is the strongest individual-level deterrent the design offers, and it is worth adopting for exactly the claims where social fraud is most likely.

---

## Governance Structure

### The Community Land Committee

The Land Committee is the primary governance body for each TerraCommons deployment. It does not administer the technical system — it governs the social processes the technical system depends on.

**Composition:**

A Land Committee must include:
- A minimum of five members
- At least 40% women (not as a token requirement — women are disproportionately the victims of land rights failure and disproportionately hold local knowledge about household boundaries and occupation history)
- Representation from the community's major sub-groups (clans, ethnic groups, age cohorts) where applicable
- No member who holds a certified land claim currently under dispute
- No member who is a close family relation of another member (to be defined by the community according to local kinship structures)

A Land Committee must not include:
- The current credential issuer (separation of issuing and governing authority)
- Any member who has received a warrant in the previous 24 months
- Any representative of the NGO implementation partner in a voting capacity (the NGO may hold an observer seat without voting rights)

**Selection:**

Land Committee members must be selected by the community through a process the community itself defines — not appointed by the NGO partner, not selected by existing committee members, and not determined by the deploying institution. The selection process must be documented and its outcome ratified by a community assembly before deployment.

The selection process itself is the first test of whether TerraCommons is working with a community or for a community. An NGO that appoints its own Land Committee has recreated the institutional capture it is supposed to resist.

**Term limits:**

Members serve fixed terms of no more than three years. No member may serve more than two consecutive terms. Staggered rotation — replacing no more than half the committee at any election — maintains institutional continuity while preventing entrenchment. Term limits are not bureaucratic preference. They are the primary protection against the kind of long-term capture that has undermined every previous land governance institution in the region.

**Decision-making:**

Routine decisions (claim certification confirmation, validator credential issuance, mediation appointment) require a simple majority. Significant decisions (validator credential revocation, mediation outcomes affecting certified claims, governance rule changes) require a two-thirds majority. Constitutional decisions (DNA migration, changes to the authorised issuer, changes to validation thresholds) require a three-quarters supermajority of a community assembly, not just the Land Committee.

The Land Committee cannot make constitutional decisions unilaterally. This is not a limitation on its authority — it is the protection against its capture.

**Transparency:**

All Land Committee decisions are recorded in the Governance DNA and readable via the HTTP Gateway. Meeting minutes, vote records, and rationale statements are public by default. A decision whose reasoning cannot withstand public scrutiny should not be made.

---

### Credential Issuer Governance

The credential issuer — whoever holds the key that authorises community members to join the Community Registration DNA — is the most structurally powerful role in TerraCommons. This power must be distributed, monitored, and made removable.

**Multi-signature issuing authority:**

Independent reviewers have identified TerraCommons' configurability — the ability for each community to deploy its own DNA with its own thresholds — as "centralisation disguised as configurability." The critique is precise: someone deploys the DNA, and whoever does so chooses the thresholds and holds the issuing key. If that actor is an NGO with its own institutional interests, or a politically connected local figure, the system's apparent community ownership is a surface over a familiar power structure.

The structural answer is multi-signature issuing authority, not a governance policy that trusts the issuer to behave well.

No single individual should hold the issuing key alone. The recommended structure for the pilot is a 2-of-3 multi-signature scheme: any two of three designated keyholders must co-sign a joining credential for it to be valid. The three keyholders should be: one community representative designated by the Land Committee, one NGO representative, and one independent observer (a representative from a national land rights network, a university partner, or an international NGO with no operational interest in the pilot).

This structure means:
- The NGO alone cannot onboard or exclude community members
- The community alone cannot be coerced into excluding members by an external actor
- The independent observer provides a check on both

Multi-sig does not eliminate the deployment power question — someone still deploys the DNA and sets initial thresholds — but it distributes the ongoing issuing authority across parties with different institutional interests, making sustained unilateral capture structurally harder.

**Deployment governance — the unconstrained moment:**

The multi-signature structure governs ongoing credential issuance after deployment. It does not govern the initial deployment act itself — the moment when someone runs the command that creates the network, fixes the DNA hash, sets the validation thresholds, and designates the initial authorised issuers. That moment is currently unconstrained in the documents, and it is the most powerful single act in TerraCommons' lifecycle. Whoever deploys the DNA makes decisions that cannot be changed without a network migration.

This gap must be closed before deployment. The governance framework requires the following deployment governance protocol:

The DNA parameters — validation threshold, required validator count, session timeout, overlap tolerance, authorised issuer keys — must be agreed in writing by a community assembly before the deployment command is run. This agreement must be documented, signed by the Land Committee, witnessed by the independent observer, and published via whatever communications channel the community uses before deployment occurs. The NGO partner may provide technical execution of the deployment, but may not set any parameter without prior written community assembly ratification.

The deployment itself must be witnessed: at minimum the community Land Committee chair, the NGO technical lead, and the independent observer must be present or represented when the DNA is deployed. The deployment transaction hash must be published immediately to the HTTP Gateway and to any international partners.

A DNA deployed without this process has not been deployed with community consent. It has been deployed for a community. That is not TerraCommons.

**The bootstrapping problem — governing the pre-governance phase:**

The deployment governance protocol above requires a Land Committee, a community assembly, and an independent observer to exist before deployment. But in many communities TerraCommons targets, those structures do not exist before the project arrives. TerraCommons is often proposed precisely because functional community governance is absent or captured. The governance framework cannot require community ratification of deployment parameters if there is no governed community body to do the ratifying.

This is the bootstrapping problem: the governance process that constrains deployment is itself ungoverned before deployment.

The governance framework addresses this through a two-layer approach grounded in international standards and adaptable to local law:

*Universal principle:* The UN Voluntary Guidelines on the Responsible Governance of Tenure (VGGT), specifically Section 9 on indigenous and community land, requires that governance processes for community land formalisation have documented legitimacy before they begin. Where no formal community governance structure yet exists, an interim structure with documented authority must be constituted before the deployment governance protocol can operate. This interim structure is not TerraCommons governance — it is the pre-TerraCommons mechanism that legitimises the deployment process.

*Implementation guidance — Kenya:* The Community Land Act 2016, Section 10, provides a statutory mechanism for an interim committee to be appointed to initiate community land registration where no formal structure exists. TerraCommons' Turkana pilot has proposed constituting its pre-deployment body under CLA 2016 Section 10. However, this requires careful legal scrutiny before adoption: under the CLA, the statutory mandate to facilitate community land registration rests with the Cabinet Secretary and the County Government, not with an NGO. A committee constituted through an NGO-led process rather than through the statutory government-appointment pathway may lack the "Gazetted" status required by Kenyan law to give its decisions legal effect. A hostile County Government could use this to declare TerraCommons records void on the grounds that the entire process was initiated by a self-appointed rather than a government-appointed body.

The Kenyan legal review required under Non-Negotiable 9 must address this specific question: whether an NGO-constituted pre-deployment body can invoke CLA 2016 Section 10 without County Government involvement, and if not, what the alternative statutory pathway is. Three options exist: (1) involve the County Government in constituting the interim body, accepting the capture risk that creates; (2) use the VGGT as the international standard rather than the CLA, accepting that this provides moral rather than statutory authority; (3) constitute the interim body as a purely customary governance structure under existing community law, not invoking the CLA at all and relying on FPIC compliance for legitimacy rather than statutory standing. Each has different implications for the legal recognition strategy. The Kenyan lawyer must advise on which pathway is viable before the bootstrapping protocol is finalised.

*Implementation guidance — other jurisdictions:* Deployments outside Kenya should identify the equivalent statutory or customary mechanism for constituting a legitimate interim governance body under applicable national law, or should rely on the VGGT as the international minimum standard where no domestic equivalent exists. The requirement that some documented, legitimate pre-deployment governance body exists before parameters are set is non-negotiable regardless of jurisdiction.

The interim body's sole function is to govern the deployment process and transition to the full Land Committee once deployment is complete. Its composition, decision-making process, and meeting records must be documented and published before the DNA is deployed.

**Interim committee representativeness verification:** Constituting an interim committee does not automatically make it representative. An NGO arriving in a community will have existing contacts — often the most accessible, most educated, or most English-speaking community members, who may not be representative of the breadth of legitimate claimants. The governance framework requires that the interim committee's composition be verified as broadly representative before it exercises any authority over the deployment process. Verification means: the committee's membership must be publicly announced to the community through whatever communication channels reach the broadest audience, a documented period of at least 14 days must allow community members to raise objections to the composition, and any objection must be reviewed by the independent observer before the committee proceeds. This is not a veto mechanism — it is a visibility mechanism. An interim committee whose composition has survived public scrutiny has more legitimacy than one assembled quietly by the NGO and immediately operationalised.

**Key rotation protocol:**

When a keyholder leaves — an NGO fieldworker is replaced, a community representative's term ends — key rotation must be authorised by the Land Committee by a two-thirds majority, witnessed by the remaining keyholders, and recorded in the Governance DNA before the old key is retired. The new key must be activated before the old key is deactivated. At no point should the network be in a state where no valid issuing key exists.

The rotation must be completed within 30 days of the departing keyholder's exit being known. A network where key rotation is perpetually deferred is a network drifting toward a freeze.

**Multi-sig key backup and recovery:**

A 2-of-3 multi-signature scheme has a specific vulnerability: if one keyholder loses their device before their key share has been backed up, the scheme drops to 1-of-3 — below threshold. New credentials cannot be issued. The network begins to freeze before the rotation process can even be initiated.

Each keyholder must maintain a documented key backup, stored separately from their primary device, using a method appropriate to their context and technical capacity. The independent observer holds a sealed record of the backup method (not the key itself) for each keyholder, to be opened in the event of disputed loss. The Land Committee must confirm, at each quarterly audit, that all three keyholders have current, documented backups. A keyholder who cannot confirm their backup at a quarterly audit is placed on a 30-day remediation notice. If unresolved, the Land Committee may initiate a key rotation to replace them before loss occurs rather than after.

This is not bureaucratic overhead. It is the difference between a network that survives personnel changes and a read-only archive.

**Credential issuance audit:**

All credential issuance events are recorded in the Governance DNA with timestamp and issuer identity (but not the content of the joining credential). A rate anomaly — an unusually high number of credentials issued in a short period — triggers automatic notification to the Land Committee. The Land Committee may call for an audit of recent issuances. This is the monitoring mechanism for rubber-stamp credential issuance.

---

### Independent Observer Accountability

The independent observer holds structural authority in TerraCommons: they hold one key in the multi-signature issuing scheme, provide binding review in escalated disputes and registry challenges, attend retrospective reviews, and serve as a whistleblower proxy to international accountability mechanisms. This authority is only legitimate if the observer role itself is accountable, bounded, and removable. An ungoverned observer is a structural point of capture as dangerous as an ungoverned credential issuer.

**Term limits:**

The independent observer serves a fixed term of two years, renewable once by community assembly vote. No observer may serve more than four consecutive years. This is not a bureaucratic preference — it prevents the observer from becoming embedded in local social dynamics, building relationships that compromise their independence, or becoming institutionally dependent on the pilot for their own role.

**Reporting obligations:**

The independent observer must submit a written report to the community assembly at each quarterly governance meeting. The report must cover: credential issuance rates and anomalies reviewed, registry challenges received and resolved, validator sanctions reviewed, any DNA or governance concerns identified, and — critically — any situation in which the observer was asked by any party to act in a way that would compromise their independence, whether or not they complied. This last requirement is the early-warning mechanism for capture attempts.

**Removal mechanism:**

The observer may be removed before their term expires by a two-thirds majority vote of the Land Committee, ratified by a community assembly. The grounds for removal must be documented and published. Removal without documented grounds is itself a governance violation reportable to international partners. The removal mechanism exists to prevent an observer who has been compromised from remaining in post — but the ratification requirement exists to prevent the Land Committee from removing an inconvenient observer without community accountability.

**Successor selection:**

When an observer term ends or removal occurs, the successor must be selected by a joint process: the Land Committee nominates two candidates, the NGO partner nominates one, and the community assembly votes on the final selection. No successor may be employed by or financially dependent on the NGO partner, the credential issuer, or any party with a certified claim in the system. The successor must be confirmed and briefed before the outgoing observer's term ends — there must be no gap in observer coverage.

**Whistleblower protection:**

The independent observer's accountability reports are public documents on the HTTP Gateway. Any observer who believes they are being pressured to act improperly — by the NGO, the Land Committee, a credential issuer, or any other party — may send a confidential report directly to the international institutional partners (World Bank, Landesa, or equivalent) without prior Land Committee approval. This is the channel by which systemic problems that the Land Committee itself is party to can reach external accountability. It requires that international partners have agreed, before pilot deployment, to receive and act on such reports.

---

### Validator Accountability

Validators are community members who confirm or dispute land claims. They are the human layer on which TerraCommons' integrity most directly depends.

**Validator compensation — a required design decision:**

Validators must take time away from subsistence farming, pastoralism, or paid work to review claim packages, form assessments, and participate in commit-reveal sessions. In a context where every day of labour has direct food security implications, expecting this as purely voluntary civic participation is not credible at scale. If validators are not compensated, the system will produce rushed assessments, selective unavailability, and effective gatekeeping by those who can afford to participate without payment.

Validator compensation is therefore not optional — it is a core operating cost that must be resolved during Phase 1 governance design, before any claims are submitted. The options and their implications:

*No compensation:* Participation depends entirely on civic motivation. Likely to produce low-quality assessments and favour validators who are economically comfortable enough to absorb the time cost — typically the same community members who already hold disproportionate power. Not recommended for the pilot.

*Claimant-funded honorarium:* The household making a claim pays a small fee distributed among the validators. Creates a direct financial relationship between claimant and validator — the same relationship the governance framework explicitly prohibits in the eligibility criteria. A household that pays their validators has a financial stake in their assessments. This is a structural corruption risk and should not be adopted.

*NGO-funded honorarium:* Validators receive a small payment from the NGO budget per completed validation session. Creates dependency on the NGO for routine operations — precisely what the exit design is supposed to prevent. Acceptable for the pilot phase only, with a documented transition to a community-funded model before NGO exit.

*Community fee pool:* Registration fees paid by households at claim submission are pooled and distributed to validators. Aligns validator incentives with system usage rather than NGO presence. Pro-poor provisions required to ensure fees do not exclude the poorest households. This is the most sustainable model for Phase 2 but requires sufficient claim volume to generate a viable pool.

The chosen compensation model must be documented in the governance design before deployment, recorded in the Governance DNA, and reviewed as part of the sustainability model discussion at Month 12. The pilot budget must include validator compensation as a line item, not an assumption absorbed into overheads.

**Validator selection and eligibility:**

To be a validator for a given claim, a community member must:
- Hold a valid credential in the Community Registration DNA
- Have no certified claim currently under dispute
- Have no family relationship with the claimant (defined locally)
- Have no documented financial relationship with the claimant in the previous 12 months
- Be identified as a required validator by the DNA's validation rules for this claim type (adjacent neighbour, community elder, etc.)

**The required validator determination problem:**

Who is identified as a "required validator" for a given claim is itself a governance question the technical system cannot resolve alone. The DNA's validation rules specify *categories* of required validators (adjacent households, registered elders). The Land Committee's responsibility is to maintain the registry of who belongs to each category — which households are adjacent to a given parcel, which community members hold elder status.

This registry is the most critical document in TerraCommons' governance layer. It must be maintained transparently, updated through a documented process, and subject to challenge by any community member. A corrupt Land Committee that manipulates the validator registry — assigning friendly validators and excluding hostile ones from required validator pools — can corrupt the validation process without violating any cryptographic rule. The registry must therefore be:

- Publicly readable via the HTTP Gateway
- Updated only through a documented process requiring Land Committee majority vote
- Subject to community challenge for any 30-day period after an update
- Audited by the independent observer on a quarterly basis

**Validator registry challenge process:** The documents name the registry as challengeable but do not specify how. The challenge mechanism must be explicit. Any community member may challenge a registry entry — the addition, removal, or re-categorisation of a validator — by filing a written objection with the Land Committee within 30 days of the change being published. The objection must state the grounds: factual error (the person does not meet the stated category criteria), conflict of interest (the Land Committee member who proposed the change has a relationship with the added or removed validator that should have required recusal), or procedural violation (the change was not made by majority vote or was not published within the required timeframe). The Land Committee must respond in writing within 14 days. If the objecting party is unsatisfied, the challenge escalates to the independent observer for binding review within a further 14 days. The independent observer's decision on registry composition is final pending a community assembly appeal. This challenge process applies to the registry itself — not to individual validation outcomes, which have their own challenge pathway through the arbitration process.

**Stall-as-veto — the non-participation problem:**

The commit-reveal protocol requires all designated validators to participate. A required validator who simply does not submit their commitment stalls the session indefinitely. In a low-connectivity environment, a dead battery and a strategic refusal to validate are technically indistinguishable. This is the stall-as-veto attack: a validator who has been paid or coerced to block a claim does so by doing nothing, and the system cannot tell the difference between obstruction and genuine technical failure.

The governance response requires a defined protocol for non-participation:

After a session timeout period (the specific duration to be set by community consensus and baked into DNA properties), a validator who has not submitted their commitment is recorded in the Governance DNA as non-participating, with the timestamp of their last confirmed network activity. The Land Committee reviews the non-participation. If the Land Committee determines, by majority vote, that non-participation was non-technical — based on the validator's documented network activity, their stated reason, or pattern of prior non-participation — it may authorise a substitute validator from the same required category, with the substitution and its rationale recorded publicly.

Critically: the substitute validator process is itself observable and challengeable. If a community member believes the Land Committee used the substitute process to swap in a friendly validator, they may challenge the substitution within 30 days. The challenge goes to the independent observer for review. This does not prevent the stall-as-veto attack in the short term — a determined obstructor can still delay a claim by weeks. It makes sustained obstruction visible, accountable, and ultimately subject to the graduated sanctions process.

**The slow-motion stall attack:**

The governance response above addresses the obvious version of the stall-as-veto. It does not prevent the slow-motion version. A determined bad actor does not need to sustain non-participation; they only need to use it strategically across the multiple stages of the governance response. A validator goes offline; the Land Committee convenes; the substitution is made; the 30-day challenge window opens; a challenge is filed; the independent observer reviews. Each stage is legitimate. Each stage takes time. A contested claim can be kept in limbo for months through entirely legitimate-looking process friction, without any single act being provably obstructive.

There is no clean technical solution to this. The governance framework's response is transparency and mandatory time limits: every stage of the substitution and challenge process must have a documented maximum duration — proposed: Land Committee review within 7 days of timeout, independent observer review within 14 days of challenge. Those durations are published via the HTTP Gateway. A validator who routinely triggers the maximum-duration process across multiple claims, always involving the same claimant or the same land area, is visible in the aggregate even when each individual event looks legitimate. The graduated sanctions process applies to this pattern.

*First-level misconduct* (first warrant issued, pattern of abstentions without documented reason, minor inconsistencies in validation history): formal notation in the Governance DNA, mandatory review meeting with Land Committee, no immediate credential suspension.

*Second-level misconduct* (second warrant within 24 months, statistical evidence of systematic bias, refusal to participate in Land Committee review): temporary credential suspension, pending a hearing. Suspension means no new validation assignments — it does not invalidate previous validations.

*Third-level misconduct* (warrant for deliberate manipulation, evidence of payment for false validation, two or more second-level findings): permanent credential revocation, recorded in the Governance DNA. All claims validated by this validator in the previous 12 months are flagged for review.

The 12-month retrospective review is expensive. In a 500-household pilot where an active validator has conducted 50–100 validations per year, a third-level finding triggers a review of up to 100 claims — each requiring a fresh validation session with newly assigned validators, claimant notification, and Land Committee oversight. This is a substantial community burden that falls at exactly the moment when the community's trust in the validation system has been most damaged.

The governance framework must specify how retrospective reviews are managed:

- **Coordination:** The Land Committee chair convenes a retrospective review panel drawn from validators who had no prior involvement with the sanctioned validator's claims. The independent observer attends all panel sessions.
- **Re-validation validators:** Re-validations must be assigned to validators from different category pools than the original validator where possible, to reduce the risk that social networks connected to the sanctioned validator corrupt the re-validation.
- **Timeline:** The Land Committee must complete the retrospective review within 90 days of the third-level finding being confirmed. Extensions require independent observer authorisation.
- **Funding:** The NGO partner is contractually responsible for funding retrospective review costs during the pilot phase, including facilitator time, validator compensation, and independent observer attendance. Post-pilot, the governance framework's long-term sustainability model must include a reserve fund for retrospective review costs — this is a known, periodic, unavoidable expense for any system that relies on human validators.
- **Community communication:** Every claimant whose claim is under retrospective review must be notified in accessible language within 7 days of the review being initiated. The notification must explain what is being reviewed, why, and what the possible outcomes are.

Communities must understand before deployment that systematic validator fraud has a remediation cost — and that the cost is part of why fraud deterrence matters.

**The knowledge fraud problem — governance response:**

Since the warrant system cannot detect a validator who follows the protocol while providing false testimony, the governance response is reputation over time, not technical detection.

Every validator's history — claims validated, agreement rates, concordance with other validators, cases later referred to arbitration, arbitration outcomes — is maintained in the Governance DNA and observable by the Land Committee. A validator whose assessments are systematically inconsistent with eventual arbitration outcomes, or whose "Agree" assessments are disproportionately associated with claims that later face dispute, becomes visible to the Land Committee over time.

This is emphatically not a pilot-phase tool. A 500-household pilot producing a few hundred validations per year does not generate the sample size needed for reliable statistical pattern detection. Collusion in the pilot phase will not be caught by data analysis — it will be caught, if at all, by active Land Committee oversight, community challenge processes, and the independent observer's quarterly audit. The statistical layer is a long-term monitoring mechanism that becomes meaningful after years of accumulated validation history across a mature network. Presenting it as a near-term fraud detection system would be dishonest.

This is not a substitute for cryptographic proof. It is the honest answer to what governance can provide when cryptographic proof is unavailable. Reputation builds slowly. Fraud may succeed for months before a pattern becomes visible. The governance framework acknowledges this and compensates by making the Land Committee's active oversight — not passive data analysis — the primary fraud detection mechanism in the pilot phase.

---

### Facilitator Credential Model

Facilitators are community members who assist households — particularly those with low digital literacy — through the claim submission, validation, and transfer processes. In communities where the majority of participants cannot operate the application independently, facilitators are not a convenience feature; they are the primary interface between the household and the system. This makes them structurally powerful and currently ungoverned.

The current architecture does not specify facilitator credentials, record facilitator actions, or provide any mechanism by which a household can challenge what a facilitator did on their behalf. A facilitator who submits a false GPS boundary, selects the wrong documents, or — in the most serious case — uses a household's device and access to register a claim on behalf of a third party faces no protocol-level accountability. Their actions would look identical to the household's own.

**Facilitator credential type:**

Facilitators must hold a distinct credential type, separate from the standard Community Registration credential, issued by the Land Committee. This is not the same as the NGO issuing a credential — the Land Committee issues the facilitator credential under the community's authority, not the NGO's. The facilitator credential is recorded in the Governance DNA with: the facilitator's identity, the date of issue, the Land Committee members who authorised it, and the scope of permitted facilitation (full submission, GPS assistance only, document scanning only, or other defined scope).

**Eligibility criteria:**

A facilitator must: hold a standard Community Registration credential in good standing; have no certified claim currently under dispute; have completed the facilitator training programme defined in the pilot plan; be approved by a Land Committee majority vote; and not be a close family member of any household they facilitate for (to be defined by community kinship norms).

**Recorded facilitation:**

Every facilitation action — every session where a facilitator acts on behalf of a household — must be recorded in the Governance DNA as a `FacilitationRecord` entry: facilitator identity, household identity, action taken, timestamp. This creates an auditable log. A household that later discovers their claim was registered incorrectly has a documented trail of who facilitated and what they did.

**Household challenge pathway:**

A household may challenge a facilitation action at any time by filing a written complaint with the Land Committee. The complaint must state what the facilitator did and what the household asserts should have been done differently. The Land Committee must respond within 14 days. If the facilitation produced a certified claim that the household disputes, the challenge escalates through the standard arbitration process. If the facilitation produced an incorrect but uncertified entry, the Land Committee may authorise a corrected re-submission without full arbitration.

**The zero-literacy verification problem:**

The challenge pathway above has a structural limitation. It works for households that are sufficiently literate or digitally competent to know what was submitted on their behalf. For households with zero digital literacy — those most dependent on facilitators — the challenge pathway is practically unavailable. They cannot read GPS coordinates, verify document fingerprints, or identify discrepancies between what they intended and what was submitted. They saw the facilitator tap a screen. They have no independent basis for knowing whether the submission was accurate.

The governance response is a mandatory independent verification step before any submission made on behalf of a zero-literacy household is finalised. A second community member — not the facilitator, not a family member of the facilitator, and with no financial relationship with the claimant — must be present at the point of submission and must read back to the household, in accessible local language, what is about to be submitted: the parcel location, the boundary description, the documents included, and the validators identified. The household's verbal confirmation is witnessed by this second person and recorded in the FacilitationRecord entry alongside the facilitator's identity.

This does not require the household to understand the technology. It requires that the content of the submission be communicated to them in human terms they can understand and confirm before it is committed. The second community member serves as an accessible-language bridge, not a technical reviewer.

The Land Committee must designate a pool of trained verification witnesses before deployment begins. Verification witnesses are not facilitators — they do not operate the application. They are community members with sufficient standing and literacy to provide the read-back service reliably. They may be compensated from the same model as validators.

**Graduated sanctions:**

First finding (minor error, no evident bad intent): remediation training, notation in Governance DNA. Second finding (pattern of errors, or a single error with material effect on a household's rights): temporary suspension from facilitation pending Land Committee review. Third finding (deliberate manipulation, evidence of false submission or fraud): permanent revocation of facilitator credential, recorded in Governance DNA, all facilitated claims in previous 12 months flagged for review using the same retrospective review protocol as validator misconduct.

**Facilitator scope as a governance variable:** The Land Committee may choose to restrict what facilitators are permitted to do — GPS recording only, leaving document submission to the household; or full facilitation in the first year with scope reduction as community digital literacy improves. This scope is encoded in the facilitator credential and enforced at the governance level, not the protocol level. It is explicitly a community decision, not a TerraCommons default.

**Pre-registration device backup — mandatory facilitator responsibility:**

DNA 1 lives on the household's device and nowhere else. If that device is lost or destroyed, the household's private evidence chain is gone — the certified claim survives on the shared DHT, but the supporting evidence for any future dispute or legal proceeding does not. This is not a rare edge case at pilot scale.

Before a household's first submission, the facilitator must: explain to the household in accessible language that their device is the sole location of their private evidence; assist them in creating at least one backup of their DNA 1 agent key using whatever method is appropriate to their context (second device, printed record, community device store); and record in the FacilitationRecord entry that backup was completed. No claim submission should proceed from a household that has not completed this step. The backup method need not be technically sophisticated — the requirement is that a second copy exists somewhere the household can access it independently of the primary device.

---

When a claim is referred to arbitration — either by the commit-reveal protocol (agreement below the arbitration threshold) or by a community member challenge — the governance layer handles resolution.

**Arbitration composition:**

A three-person arbitration panel is convened for each dispute:
- One member appointed by the Land Committee
- One member selected by the claimant
- One member selected by the challenging party

No panel member may hold a certified claim adjacent to the disputed land. No panel member may have a family relationship with either party. If the parties cannot agree on their appointees, the Land Committee appoints both.

**Arbitration process:**

1. Both parties present evidence — GPS coordinates, document fingerprints, witness testimony, occupation history
2. The panel may request additional validator assessments from the community
3. The panel issues a written finding within 30 days
4. The finding is recorded in the Governance DNA with full rationale
5. Either party may appeal to an external authority (the NGO partner's legal team, a national land rights organisation, or formal courts) within 60 days

**Arbitration is not binding on formal legal systems.** A TerraCommons arbitration finding is not a court judgment. What it is: a documented, timestamped, evidence-referenced record of a community-based dispute resolution process, conducted by named and accountable community members, readable by any external party. In a legal proceeding, that is a more credible piece of evidence than an undocumented oral claim. It is not equivalent to a title deed.

**Arbitration has no enforcement mechanism.** This must be stated plainly. A TerraCommons arbitration panel produces a written finding. That finding is recorded in the Governance DNA, attributable, and irrefutable as a record of process. It cannot compel a party to return money. It cannot transfer land. It cannot restrain a developer with a government title deed. A community whose member has committed fraud — received payment from two buyers before a source chain fork was detected — has a documented record of that fraud and no automatic remedy. The governance framework's response to a confirmed fraud finding is: the finding is escalated to whatever external legal or institutional authority has jurisdiction, with the TerraCommons record as evidence. The quality of that evidence is better than the alternative. The enforcement depends entirely on the external authority's willingness and capacity to act.

Communities must understand this before deployment. A governance system that documents injustice without remedying it changes the evidentiary landscape. It does not change the power landscape. For communities facing well-resourced adversaries — developers with government connections, officials with paper title deeds — a better evidentiary position is genuinely useful only if someone with enforcement capacity is willing to act on it. TerraCommons cannot guarantee that. It can make the record available to those who might.

This limitation must be communicated to communities before deployment. Communities must not be misled into believing that a TerraCommons arbitration finding will be respected by courts or officials who hold conflicting paper records.

---

## Migration Governance

Network migration — deploying a new DNA version — is the most structurally dangerous moment in TerraCommons' lifecycle. It is the moment when historical records are most vulnerable to manipulation, and when a powerful actor who cannot invalidate an inconvenient claim through normal governance channels has the greatest structural opportunity to do so.

### The Migration-as-Erasure Attack

When a mandatory migration occurs — because an issuer key must be rotated, thresholds changed, or entry types updated — whoever controls the migration process has structural power over which historical records survive. A claim that is "disputed" or "unresolvable" at migration time may be excluded from the new network. This is the digital equivalent of what corrupt officials do during paper registry transitions.

### Migration Governance Protocol

**A note on upgrade continuity:** In current Holochain, participants do not all have to upgrade to a new DNA simultaneously — they must upgrade eventually to participate in the new shared space, but there is continuity within the existing network during the transition window. This means the migration process is less brittle than a hard simultaneous cut-over: participants on the old DNA version can continue accessing existing records during the transition period. However, this does not eliminate the requirement for coordinated migration planning. Inconsistent state between TerraCommons' four DNAs — where DNA 2 has migrated but DNA 3 has not — remains a data integrity risk regardless of how gracefully individual participants transition. The coordinated multi-DNA migration requirement below applies to the DNAs as a system, not to individual participant upgrade timing.

**ValiChord finding:** The migration machinery in this section guards a narrower class of change than the framework currently implies. In ValiChord — built on the identical four-DNA structure — *coordinator-only* upgrades (`UpdateCoordinators`) ship live without altering any DNA hash, and therefore without any migration at all: bug fixes, new read/query functions, warrant-gate adjustments, and scheduling changes all deploy this way. Only true *integrity* changes — new entry or link types, altered validation rules, and the baked-in thresholds and issuer keys — force a new DNA hash and a network migration. The heavy migration-governance protocol below is therefore required only for that integrity class; the larger category of routine operational fixes does not trigger the migration-as-erasure risk and should not be gated behind a community-assembly supermajority. This distinction should be made explicit to communities, so that ordinary maintenance is not mistaken for a constitutional event — and so that the genuine constitutional events are not diluted by lumping them together with routine ones.

**Triggers for migration:**

A DNA migration may only be initiated by a three-quarters supermajority vote of a community assembly, or by a two-thirds vote of the Land Committee following documented evidence that the current DNA has a critical security flaw. No migration may be initiated by the NGO partner alone, by the credential issuer alone, or by any external actor without community assembly ratification.

**Coordinated multi-DNA migration requirement:**

TerraCommons uses a four-DNA architecture. When a migration is required, all affected DNAs must migrate in coordination. A migration that updates DNA 2 (Community Registration) without simultaneously updating DNA 3 (Governance) to match creates inconsistent state between networks — credential validity rules, threshold values, and entry type definitions may diverge. In a land rights context, inconsistent state between DNAs is not a recoverable error; it is a data integrity failure that may render certified claims unverifiable. The migration plan must specify which DNAs are affected, in what sequence they will be migrated, what the acceptable state gap between DNA migrations is (likely zero — all must migrate atomically or in a single coordinated sequence), and what the rollback procedure is if one DNA migration fails partway through. This is an engineering requirement that must be specified before any migration is authorised, regardless of urgency.

**Migration record requirements:**

Every certified claim in the current network must be documented before migration begins. The documentation — claim hashes, certification records, arbitration outcomes — must be published via the HTTP Gateway before the migration commences. This creates a publicly accessible snapshot of the pre-migration state that cannot be altered after the fact.

**Porting requirements:**

Every certified claim in the pre-migration network must be ported to the new network unless:
- The claim is currently in active dispute, in which case it is ported as "Under Review" pending resolution
- The claim holder has explicitly consented to exclusion
- The Land Committee has documented, publicly stated grounds for exclusion, subject to community challenge for 30 days

Any claim excluded from a migration without meeting one of these conditions is a governance violation. The independent observer has standing to challenge exclusions.

**Migration audit:**

Within 60 days of a completed migration, the independent observer must publish an audit comparing pre- and post-migration claim records. Discrepancies not accounted for by the three conditions above must be reported to the community assembly and, if unresolved, to the NGO partner's governance body and any international institutional partners.

**The audit is accountability and deterrence, not prevention.** This must be stated plainly. A powerful actor who excludes inconvenient claims during a migration has already succeeded by the time the audit reports. The pre-migration public snapshot — the requirement that all certified claims be published via the HTTP Gateway before migration commences — is the preventive mechanism. It creates a permanent, publicly accessible record of what existed before the migration that cannot be retroactively altered. The audit then compares what was promised against what arrived. If discrepancies are found, the harm has occurred but it is documented, attributable, and irrefutable. The governance framework's response to a confirmed erasure is escalation to international institutional partners and, where legal pathways exist, litigation. Deterrence depends on the credible threat of these consequences being enforced. Communities and institutional partners must understand this distinction before migration governance is invoked.

---

## NGO Partnership and Exit Design

The NGO implementation partner is not a neutral technical facilitator. It has institutional interests: funding, reputation, relationships with government, the need to demonstrate success to its own donors. These interests do not always align with the community's interests. The governance framework is designed with that misalignment as a permanent assumption, not an edge case.

**The pilot success metrics must measure what matters, not what is measurable.** There is a specific risk with the "zero corruption incidents" metric used in the pilot plan. The warrant system and protocol violation monitoring capture cryptographic fraud — commit-reveal mismatches, source chain forks, ungrounded reveals. Social fraud — validators who follow the protocol correctly while providing false testimony — is excluded from this metric by definition, because no protocol violation occurs. An NGO under funder pressure can report "zero corruption incidents" in a pilot that is systematically corrupted through bribed validators, because the corruption is invisible to the metric.

The pilot plan must include the following qualitative indicators alongside protocol-level metrics. Funders must understand that "zero protocol violations" is not the same as "no corruption":

- **Dispute rate post-certification:** The proportion of certified claims that are subsequently disputed by a neighbouring household within 12 months. A high rate suggests validation quality is poor even where protocol compliance is perfect.
- **Arbitration request rate:** The number of arbitration requests filed per 100 registered claims. A low rate may indicate genuine accuracy; it may also indicate that community members do not trust the arbitration process or fear retaliation for challenging.
- **Registry challenge rate:** The number of challenges filed against validator registry entries. Zero challenges in a functioning community is a warning sign, not a success indicator — it may mean the registry is not being scrutinised.
- **Claim challenge rate:** The proportion of validations challenged by any party during the 30-day challenge window. Again, zero challenges warrants scrutiny, not celebration.
- **Community satisfaction survey:** An independent survey of registered and non-registered households asking whether the process felt fair, whether they understood what they were signing, and whether they feel their rights are better or worse protected than before. Conducted at month 12 and month 24 by an evaluator with no operational relationship to the NGO partner.
- **Non-registration rate and reason:** How many eligible households chose not to register, and why. Voluntary non-registration is itself data — it may indicate that the community does not trust the system, that certain groups have been excluded, or that informal arrangements are considered more reliable.

These indicators will not catch all social fraud. They will make it harder to hide, and harder for funders to ignore.

### NGO Role Constraints

**The NGO may:**
- Provide technical implementation capacity
- Hold one seat on the credential issuance multi-signature scheme
- Hold a non-voting observer seat on the Land Committee
- Provide legal and advocacy support
- Report to funders on pilot progress

**The NGO may not:**
- Appoint Land Committee members
- Hold the sole issuing key
- Deploy the DNA without community assembly ratification of the thresholds and rules
- Modify governance rules without community assembly ratification
- Withhold technical access or data as leverage in funding or governance disputes

**The NGO must:**
- Document its own governance relationship with TerraCommons in writing, signed by the community assembly, before deployment
- Maintain the always-on gateway node as a contractual commitment for a minimum of 36 months post-deployment
- Provide key rotation support within 30 days of any staff change affecting a keyholder
- Conduct a documented exit planning process beginning no later than month 12 of the pilot
- Disburse Track B co-design funding separately from Track A operational funding, through a disbursement structure verified by the independent observer — see below

**Track B budget disbursement — structural enforcement:**

The Vision & Architecture document requires Track B (pastoral governance ledger co-design) to have a non-transferable budget line. The governance framework implements this structurally rather than relying on the NGO's internal budget discipline.

Track B funds must be disbursed through a separate accounting line, held by a different implementing organisation where possible — ideally a pastoral rights specialist organisation rather than the land NGO holding Track A funds. Where a single NGO holds both, Track B funds must be held in a ring-fenced account and released only against Track B milestones verified by the independent observer.

Track B milestones for disbursement purposes: Month 3 — pastoral rights specialist engaged and community consultation schedule agreed; Month 6 — first community consultation sessions completed and documented; Month 12 — draft governance-ledger architecture available for community review.

If a Track B milestone is not met, the corresponding disbursement tranche is held by the independent observer until the milestone is achieved or until the Land Committee and community assembly vote to formally acknowledge Track B has failed its Month 12 primary metric. Funds cannot be redirected to Track A without a community assembly vote and documented recording of the Track B failure.

### Exit Design

The NGO will eventually leave. This is not a risk to be prevented — it is a certainty to be planned for. The governance framework treats exit planning as a first-day responsibility, not an end-of-project afterthought.

**Technical exit requirements:**

Before the NGO can formally exit:
- At least two community members must hold valid issuing key shares in the multi-signature scheme
- At least one community member must be trained and documented as capable of administering the always-on gateway node
- The credential issuer composition must have been transitioned to a community-majority structure
- A successor organisation (a community land trust, a national land rights NGO, or a regional institution) must have been identified and briefed

**The realistic minimum viable exit state:**

The governance framework must be honest about what the above requirements actually achieve. Requiring that a community member be "trained and documented as capable of administering the always-on gateway node" creates a checkbox, not a capability. In eighteen months, in a community with low digital literacy and intermittent connectivity, producing a community member who genuinely understands Holochain conductor configuration, DHT behaviour, and key management — not just someone who can follow a script until the first novel error — is not a realistic training outcome.

The honest minimum viable exit state is therefore probably: a regional successor organisation, not an individual community member, holds technical operational responsibility for the infrastructure layer after the pilot NGO exits. The community holds governance ownership — the Land Committee, the community assembly, the credential issuer composition, the arbitration process. The technical infrastructure layer — gateway node administration, software updates, connectivity management — is held by an institution with the ongoing technical capacity to maintain it.

This is less ideologically pure than full community technical ownership. It is substantially more realistic. The governance framework should name this as the expected exit structure rather than treating technical community ownership as the default and institutional technical support as a fallback. Funders should be told this explicitly: TerraCommons' exit model is governance devolution to the community, technical devolution to a regional institution. Both are necessary. Neither alone is sufficient.

**Long-term operational funding sustainability:**

The pilot has a defined budget. Phase 2 has a budget framework. The documents do not address what happens in year 5, year 10, or year 20. The always-on gateway node requires ongoing hosting. Software requires maintenance when Holochain releases updates — some of which may be breaking changes requiring engineering work. The regional successor organisation requires institutional capacity that must be funded. The independent observer role, if it continues post-pilot, requires funding.

This is not a problem unique to TerraCommons, but it is a problem that has killed many community technology projects in Sub-Saharan Africa. A system that works at pilot scale and then degrades into an unmaintained archive within five years has caused harm — it has created expectations, disclosed information, and shaped community understanding of land rights in ways that cannot be easily undone.

The governance framework requires that a long-term sustainability model be identified and documented before Phase 2 begins — not assumed away. Three candidate models exist, each with different implications:

*Community fee model:* Households pay a small fee for registration, transfer, and arbitration services. This creates financial sustainability and aligns incentives — communities that value the system fund it. The risk is that fees create barriers to participation, particularly for the poorest households, and may recreate the economic exclusion that land formalisation is supposed to address. Any fee structure requires explicit pro-poor provisions.

*Institutional subsidy model:* A national land rights organisation, an international NGO, or a government body subsidises ongoing operations. This is the most common model for community infrastructure in Sub-Saharan Africa. The risk is that the subsidising institution acquires leverage over the system — a dependency relationship that recreates some of the NGO dependency problems the exit plan is designed to prevent.

*Government integration model:* TerraCommons feeds into the national land registry system and receives operational funding as part of the official land administration infrastructure. This is the highest-value outcome and the hardest to achieve. It is the long-term goal of the legal recognition strategy.

The governance framework does not prescribe which model to adopt — that is a community and institutional decision. It requires that the question be asked and answered before Phase 2 funding is sought, and that the answer be disclosed to funders, communities, and institutional partners as part of the Phase 2 proposal.

**Sustainability hard deadlines:**

The governance framework requires that the sustainability question not drift. The following milestones are mandatory:

*Month 12:* The NGO partner presents all three candidate sustainability models to a community assembly, with honest assessments of each model's implications for community autonomy, access barriers, and dependency risks. The presentation must be followed by community deliberation. The assembly does not need to decide at Month 12, but the conversation must have happened and been documented.

*Month 18:* The community assembly, with Land Committee input, selects a sustainability model. The chosen model must be operationally designed — not just selected in principle — with a documented transition plan specifying who holds what role, what the funding mechanism is, and how governance changes if the funding structure changes.

*Month 24:* Pilot funding is treated as fully replaced by the chosen sustainability model. If the model is not operational by Month 24, the wind-down protocol is triggered. The wind-down protocol must be documented before deployment begins and must specify: the minimum conditions under which the network remains operationally valuable to the community, what the community's rights are with respect to the data, and how archived records remain accessible after active operations cease.

If no sustainability model is operational by Month 24 and the wind-down protocol is triggered, this is recorded as a pilot failure on the sustainability metric — not a successful pilot with an unfortunate postscript. Funders, institutional partners, and any publications must report it as such.

---

### Governance Overload and Operational Simplicity

The governance framework described in this document is comprehensive. For communities meeting monthly, with Land Committee members who are farmers, elders, and household heads rather than administrators, it may also be overwhelming. A governance system that is technically rigorous but operationally unmanageable will not be used. It will be bypassed, ignored, or delegated to the NGO partner — which recreates the dependency the framework is designed to prevent.

The governance framework must pass a simple test: can it be explained to a Land Committee member in under an hour, and can they remember their obligations between quarterly meetings? If not, it is too complex for the pilot context.

The response to this tension is tiering: distinguishing between obligations that require active human decision-making and those that can be automated, delegated, or simplified.

**Automate wherever possible:** DHT health checks, session timeout triggers, credential issuance rate anomaly alerts, and registry publication all occur as protocol events or scheduled processes — not as Land Committee agenda items. The Land Committee should receive a dashboard summary at each quarterly meeting, not a list of individual technical events to review. The pilot engineer is responsible for designing this summary interface. If it does not exist at deployment, the governance system will produce committee overload.

**Delegate to the independent observer:** The independent observer is the standing reviewer for migration audits, retrospective review coordination, registry challenge adjudication, and mode-switch security assessments. The Land Committee authorises; the observer executes and reports. This division exists so that the Land Committee makes decisions but does not administer processes.

**Simplify graduated sanctions for the pilot:** The three-tier graduated sanctions model for validators and facilitators is appropriate for a mature network with abundant precedent. For a 500-household pilot in its first two years, the Land Committee should operate a binary threshold for the pilot phase: documented concern (first finding of any kind) versus referral for formal sanction (second finding within 24 months, or a single finding of deliberate manipulation). The full three-tier model can be implemented in Phase 2 as the community builds familiarity with governance processes. The binary threshold is not a weaker standard — it is a more actionable one for a community governance body operating at pilot scale.

**Governance review at Month 12:** The Land Committee must formally assess whether the governance framework is operationally manageable at the Month 12 community assembly. If committee members report consistent difficulty operating any aspect of the framework, that is a governance design failure that must be documented and remediated before Phase 2 — not attributed to Land Committee incapacity.

If no sustainability model is operational by Month 24 and the wind-down protocol is triggered, this is recorded as a pilot failure on the sustainability metric — not a successful pilot with an unfortunate postscript. Funders, institutional partners, and any publications must report it as such.

If the NGO exits without completing the above requirements and the issuer key is lost or becomes inaccessible, the network freezes — existing records persist but no new members can join. This is acknowledged as a failure mode. The governance framework's response is to make it contractually the NGO's responsibility to prevent it — not to pretend the technical architecture resolves it.

**Archive mode — persistence is not the same as functionality:**

A network with too few active participants to sustain DHT neighbourhoods, session completion, or credential issuance is not a functioning network. It is an archive. The distinction matters because it changes what the community can rely on: an archive can provide evidence of historical claims but cannot process new registrations, transfers, or disputes. A community that does not understand this distinction may believe their land rights infrastructure is intact when it is not.

The governance framework defines a minimum active node threshold below which the network enters archive mode. This threshold is an engineering decision, but must be specified before deployment (likely: fewer than 10 independently operated active nodes, or a DHT health check failing for two consecutive quarterly audits). Below this threshold:

- New claim registrations, transfers, and validation sessions are suspended
- Existing certified records remain readable via the always-on gateway node
- The Land Committee must escalate to the designated successor organisation within 30 days
- No new activity may commence until the successor organisation has restored the network above the minimum threshold and the Land Committee has documented the restoration

The successor organisation must be named before deployment — it is part of the exit plan. It may be a national land rights body, a regional institution, a university partner, or another community deploying TerraCommons. The successor must have agreed in writing to take on technical responsibility for a failing network before the pilot begins. An exit plan that does not name a successor is not a complete exit plan.

Archive mode is not a disaster. A well-maintained always-on gateway node preserves all certified records indefinitely. The records remain available as evidence, remain readable by institutional partners, and remain the foundation for any eventual restoration of full functionality. The governance framework's obligation is to ensure the community understands the difference between their records being safe and their governance infrastructure being operational — and to ensure they have a path back to operational status.

**A specific harm category that must be named before deployment:** A community in archive mode cannot process new transfers, inheritance registrations, or dispute resolutions. This creates a distinct injury that is worse than simply having no registry at all. A community that never registered has oral history — living, adaptive, capable of incorporating new heirs, new sales, and new disputes as they arise. A community in archive mode has a frozen record. The registered household member dies; their heir cannot be recorded. A legitimate sale is agreed; it cannot be countersigned. A boundary dispute arises; it cannot be arbitrated through the system. The registered record shows a dead person as the rights holder, or a seller who has already received payment, indefinitely — because the system that would update it is frozen.

This is the "digital fossil" risk: a registry that cannot be updated is not neutral infrastructure. It is actively misleading, showing a snapshot of rights as they stood at the moment of freezing, while life and land continue to move around it. Communities must be told this explicitly before registration begins — not as a footnote in a governance framework, but as a clear condition of participation: *if this system enters archive mode and cannot be restored, your land rights record will be frozen at that moment. New heirs, new transfers, and new disputes will have no path through this system until it is restored. You should maintain your existing oral and documentary practices alongside TerraCommons, not instead of them.*

**What happens if the pilot runs out of money mid-deployment:**

This is not an edge case. It is a common outcome in humanitarian technology projects. The governance framework requires a documented contingency plan to be agreed between the community assembly and the NGO before funding is committed, covering:

- What the state of the network is at various stopping points (month 6, month 9, month 12)
- Whether a partially-registered community is better or worse off than a pre-deployment community at each stopping point
- What the minimum viable network state is at which the community can continue without external support
- What the community's rights are with respect to the data if the NGO closes

A community that begins registration and then has the pilot abandoned mid-process faces a specific harm: they have created a partial record that is not comprehensive enough to be useful, may have disclosed information about their claims to a broader network, and are in an ambiguous legal position with respect to claims that are partially certified. The governance framework must require that this harm is explicitly assessed before deployment begins.

---

## Community Capture Resistance

### The Divided Community Problem

TerraCommons is not suitable for deployment in communities with active violent conflict over land. A system that records claims and validates them through community consensus cannot function in a community where the consensus process itself is a weapon.

There is a second, less obvious exclusion criterion that is equally important: **TerraCommons must not deploy in communities where the population currently present is not representative of all people with legitimate claims to land in that area.** In post-conflict and displacement contexts, the community that remains may be the community that benefitted from the displacement of others. If the displaced minority is absent from the network — because they are in a refugee camp, have fled to a city, or have been killed — then TerraCommons does not record community consensus. It records the dispossessors' version of events, in high resolution, with cryptographic durability. This is the coordinated erasure risk: the system becoming an automated engine for formalising ethnic land-grabbing under the appearance of community validation.

No distributed validation system can protect the rights of people who are not in the network. The governance framework's response to this risk is an absolute deployment exclusion: TerraCommons does not operate in contexts where significant numbers of legitimate claimants are absent due to displacement, conflict, or persecution, unless a credible process exists for incorporating absent claimants' evidence into the validation process before certification occurs.

This is not a failure of the design. It is a scope boundary. The governance framework must include explicit deployment criteria that screen for minimum conditions:

- No active violent conflict over land claims in the target community in the previous 12 months
- No significant population of displaced persons with legitimate land claims who are absent from the community and therefore absent from the network
- An existing community governance structure (formal or customary) that is recognised as legitimate by the majority of community members — including those who may have minority or historically marginalised status
- Women's participation in community decision-making at a level that allows meaningful participation in Land Committee selection
- A minimum level of inter-clan or inter-ethnic communication sufficient to allow a contested claim to be arbitrated
- The national government must not be an active adversary to the communities whose claims are being recorded — see the Public Record Intelligence Vector criterion below
- In communities where pastoral tenure coexists with settled household tenure, known grazing corridors and seasonal routes must be documented before any settled-parcel claims are certified — see the Grazing Corridor Boundary Check below

**The grazing corridor boundary check:**

In communities where pastoral and settled tenure coexist — including the Turkana pilot — Phase 1 settled-parcel certified claims carry a specific harm risk that the documents have not previously addressed. A certified polygon claim for a settled household that sits across a traditional grazing corridor or seasonal movement route creates a cryptographically durable individual ownership record. That record does not erase the corridor, but it makes the corridor harder to assert in Phase 2: the individual claim is certified, attributable, and timestamped; the corridor right is unrecorded and informal. If the corridor claim and the certified parcel overlap, the parcel has the evidentiary advantage.

Phase 1 settled-parcel registration therefore has the potential to actively worsen the pastoralist rights problem it defers — locking in individual claims that conflict with unrecorded communal ones before the governance-ledger approach exists to record the communal side.

The governance framework requires the following before any settled-parcel claims are certified in a community with coexisting pastoral tenure: the independent observer, the NGO partner, and at least two community members with pastoral rights knowledge must jointly document known grazing corridors and seasonal movement routes in the target area through whatever means are available — GPS waypoints, oral testimony recorded and witnessed, community sketch maps, or existing NGO mapping data. This documentation does not need to be comprehensive or legally precise. It needs to be sufficient to flag which proposed settled-parcel boundaries intersect known corridor areas. Any proposed claim that intersects a documented corridor must be reviewed by the Land Committee before certification, with the corridor documentation attached to the claim record and the conflict noted. Certification of an intersecting claim proceeds only with explicit Land Committee acknowledgment of the known corridor, recorded publicly, creating a paper trail for Phase 2 resolution rather than pretending the conflict does not exist.

**The public record as intelligence vector:**

TerraCommons' Governance DNA is readable by anyone via the HTTP Gateway. This openness is a design feature: it creates accountability, enables institutional recognition, and allows journalists and advocates to verify community claims. But the same openness that makes TerraCommons legible to the World Bank makes it legible to a hostile government.

A government that wants to identify, locate, and target land claimants in a disputed area can use TerraCommons' own public record layer to do so. This is not the same as the coordinated erasure attack — it does not require controlling the network. It requires only reading it. A certified claim record showing household location, GPS boundary, claimant identity, and validation history is exactly the intelligence a hostile state actor needs to target communities for eviction, harassment, or worse. TerraCommons could become an instrument of the dispossession it was designed to prevent.

This risk requires an additional deployment exclusion criterion: **TerraCommons does not operate in contexts where the national or regional government is an active adversary to the specific communities whose claims are being recorded**, unless the public record layer is restricted to credentialed institutional access rather than open HTTP. "Active adversary" means: documented history of using land records to target communities, ongoing security operations against the target community, or a legal or political environment in which public identification of land claimants creates personal safety risk.

Where restricted public access is required, the HTTP Gateway must be gated behind credential verification — accessible to approved institutional partners (World Bank, Landesa, national land rights organisations) but not to unauthenticated queries. This trades some of TerraCommons' institutional legibility for claimant safety. In contexts where the government is adversarial, that trade is mandatory.

These are conditions, not guarantees. Communities that meet these criteria can still have significant internal tension. The governance framework manages that tension. Communities that do not meet these criteria should not be pilot deployment sites, regardless of how well they match other selection criteria.

### Elite Capture Prevention

Elite capture — the process by which a small group of well-connected community members gains disproportionate influence over a commons institution — is the primary long-term governance risk for TerraCommons.

The structural protections are:

**Distributed key holding.** No single person or institution holds the issuing key alone. Multi-signature structure distributes power.

**Term limits.** No Land Committee member serves indefinitely. Rotation prevents entrenchment.

**Public validator registry.** The registry of who belongs to each validator category is public and challengeable. Manipulation is visible.

**Threshold lock.** Validation thresholds are baked into the DNA at deployment. They cannot be quietly changed by the Land Committee — changing them requires a community assembly supermajority and a network migration, both of which are visible and costly.

**Community assembly supremacy.** Constitutional decisions — migration, threshold change, issuer key change — require community assembly ratification, not just Land Committee approval. This distributes ultimate authority to the broadest possible base.

**Independent observer.** An independent observer — a representative from a national land rights network, a university partner, or an international NGO with no operational interest — holds a standing audit mandate. Their findings are published publicly. They have standing to raise concerns with international institutional partners.

---

## The Decolonisation Commitment

TerraCommons was designed by a non-African author in collaboration with AI, without consultation with Kenyan communities, Kenyan land rights experts, or Kenyan legal practitioners. This document exists as a set of design principles, not as a governance structure that has been agreed to by the communities it proposes to serve.

This is a significant limitation, and the governance framework must be honest about it.

The governance structures described in this document are proposals for what good governance might look like. They draw on Ostrom's principles, on documented failures of previous land governance systems in East Africa, and on the red-team critiques of independent reviewers. They do not draw on the knowledge of Turkana community members about how their own governance works, what external governance structures they have experienced as legitimate or illegitimate, or what their specific concerns about a system like TerraCommons would be.

**The decolonisation commitment is not a disclaimer appended to protect the project from criticism. It is a design requirement.** A governance framework that has not been built with the community it governs is not yet a governance framework. It is a proposal awaiting the conversation that makes it real.

Before any pilot deployment, the following must happen:

**Free, Prior and Informed Consent (FPIC).** TerraCommons adopts FPIC — the international standard established under Article 19 of the UN Declaration on the Rights of Indigenous Peoples — as the minimum consent standard for all deployments. FPIC means three things precisely: consent must be *free* from coercion or pressure; it must be *prior* to any deployment activity, not sought retrospectively; and it must be *informed* — communities must understand what TerraCommons does, what it cannot do, what risks it carries, and what alternatives exist, in accessible language, before any consent is given. FPIC operates as a universal minimum standard regardless of whether domestic law in the deployment jurisdiction recognises it. A deployment that cannot satisfy FPIC does not proceed. This applies to Turkana and to every other deployment context.

**Participatory Action Research (PAR) methodology for co-design.** The governance design workshops required before deployment are not consultation sessions in which a designed system is explained and community approval is sought. They are co-design processes in which the community shapes the system. TerraCommons adopts Participatory Action Research as the named methodology for these workshops — a framework developed by Freire and Chambers specifically to address the problem of external actors designing for communities rather than with them. PAR requires community members to be co-researchers and co-designers, not subjects or consultants. The governance structures that emerge from PAR workshops may differ substantially from those proposed in this document. That is the point. This document is input to those workshops, not their predetermined output.

**Legal review under applicable national law.** A qualified land rights lawyer in each deployment jurisdiction must review the governance framework for compatibility with applicable land registration law, customary law frameworks, and relevant statutory instruments before deployment. Specifically, the review must address whether TerraCommons' certification process constitutes "registration" within the meaning of applicable statutes — if it does, it may require formal authorisation; if it does not, that limitation must be clearly communicated to communities. This review is not optional and cannot be deferred to post-deployment.

*Kenya-specific implementation:* Kenyan legal review must cover compatibility with the Community Land Act 2016, the Land Registration Act 2012, and the National Land Commission Act 2012. The CLA 2016 creates a pathway for community land registers with legal standing — TerraCommons should be designed to feed into that framework where possible, not to replace it. Where TerraCommons records can support a CLA 2016 registration application, that pathway must be explicitly documented and facilitated.

**Statutory alignment for deployment jurisdiction.** Each deployment should identify its equivalent to the CLA 2016 framework — the national or customary legal mechanism through which community land documentation achieves formal recognition — and TerraCommons should be positioned as supporting that pathway, not competing with it.

**Pastoralist rights specialists.** The governance framework as written addresses individual parcel claims more confidently than communal and pastoral rights. Before deployment in any predominantly pastoralist community, specialists in pastoralist land rights governance must review and contribute to the governance design for seasonal use rights and communal claims. Relevant organisations include RECONCILE, the Pastoral and Environmental Network in the Horn of Africa (PENHA), the International Land Coalition's pastoralist programmes, and — as a potential funder and co-design partner — The Tenure Facility, a Swedish-funded organisation with direct experience of community land rights co-design across Sub-Saharan Africa and Southeast Asia.

The deeper issue is one of design philosophy, not just technical detail. Pastoralist land use systems function because they are flexible and negotiated — seasonal corridors, grazing rights, and movement routes adapt with rainfall, community relationships, and ecological conditions. Attempting to encode these as fixed polygon records, even flexible polygon records, risks freezing arrangements that depend on adaptive social process. What TerraCommons can meaningfully offer for pastoral rights is not a land register but a governance ledger: a cryptographically attributable record of who negotiated what, with whom, when, and with what witnesses — the process, not the permanent outcome. Designing this correctly requires pastoralist communities and specialist organisations to shape the approach from the ground up, not to validate a structure designed elsewhere.

**Warrant issuer protection.** Issuing a warrant against a validator is a public, attributable act. In a small community where the corrupt validator holds social or economic power, the community member willing to issue a warrant against them is exposed to retaliation. The governance framework has no whistleblower protection mechanism that can prevent social or economic pressure in small communities.

The international framework for this problem is the UN Special Rapporteur on Human Rights Defenders, which specifically covers community members who document land rights violations. TerraCommons cannot substitute for the domestic legal protections that framework calls for. What the governance framework can do is name the independent observer as the warrant issuer's proxy: where a community member cannot safely issue a warrant directly without risk of retaliation, they may bring their evidence to the independent observer, who has standing to raise the concern formally without identifying the source. The independent observer's mandate explicitly includes this whistleblower proxy function. This does not eliminate the risk of retaliation — it reduces the exposure of the most vulnerable community members by routing accountability through an external actor with greater institutional protection.

*Universal principle:* Any deployment where community members face credible risk of serious harm for participating in governance accountability processes must address that risk explicitly in the deployment design, not assume it away.



---

## Non-Negotiables

Following the model established by ValiChord's Epistemic Integrity Commitments, TerraCommons has a set of governance commitments that cannot be conceded regardless of institutional pressure, funder requirements, or pilot expediency.

These are not aspirations. They are lines. If any of them is crossed, the project is no longer TerraCommons — it is a different project with TerraCommons' name attached.

**1. Community assembly ratification of all constitutional decisions.** No migration, no threshold change, no issuer key change without a community assembly supermajority. Not a Land Committee decision. Not an NGO decision. Not a funder requirement. The community ratifies or it does not happen.

**2. No sole issuing key.** The issuing authority is always multi-signature. A single individual or institution holding the sole key is a structural point of capture that cannot be mitigated by any other governance mechanism.

**3. No claim erasure without documented grounds.** No certified claim is excluded from a network migration without meeting the documented conditions and surviving a 30-day community challenge period. The migration-as-erasure attack must never succeed.

**4. Honest scope communication.** TerraCommons will not claim to provide legally enforceable land rights where it cannot. Communities will be told explicitly and in accessible language what a TerraCommons certification means and does not mean in their legal context before they commit to the process.

This includes a specific warning that must be communicated before any household registers a claim: a TerraCommons certified claim is only as trustworthy as the governance process that produced it. If the validators who certified a claim were bribed, coerced, or socially pressured — and if they followed the protocol correctly while doing so — the certification makes that fraud more durable, not less. A fraudulent certified claim carries the appearance of multi-validator consensus. It is harder to challenge and harder to dislodge than a fraudulent oral claim. The certified stamp does not retrospectively validate the honesty of the process that produced it. Communities must understand this before they invest confidence in the system, and the Land Committee must understand it before they invest authority in the certification process.

**5. Women's meaningful participation.** The 40% Land Committee requirement is a floor, not a target. If the community governance design process produces a Land Committee with less than 40% women, deployment does not proceed until the composition is corrected. Women's exclusion from land governance is the primary mechanism by which women's exclusion from land ownership is perpetuated. This line cannot be negotiated.

**6. Exit planning before deployment.** The NGO exit plan is agreed and documented before the first household registers a claim. A project that begins without an exit plan is not a pilot — it is an indefinite dependency.

**7. No deployment in active conflict.** TerraCommons does not deploy in communities with active violent conflict over land. The system cannot resolve that conflict; it can only make its record a new front in the fighting.

**8. No open public record where the government is an active adversary.** TerraCommons' HTTP Gateway makes certified claims readable by anyone. Where the national or regional government is an active adversary to the communities being recorded — with a documented history of using land records to target communities, or a legal environment where public identification of claimants creates personal safety risk — the public record layer must be restricted to credentialed institutional access. Open HTTP access in an adversarial government context turns TerraCommons into an intelligence tool for the dispossessors. The openness that creates accountability in a safe context creates danger in a hostile one. This line cannot be negotiated in the name of institutional legibility.

**The institutional legibility paradox:** This Non-Negotiable creates a genuine strategic tension that must be named and resolved rather than papered over. TerraCommons' multi-layer legitimacy strategy depends on institutional legibility — on the World Bank, Landesa, and international partners being able to read, verify, and recognise TerraCommons records. But the World Bank operates in partnership with national governments. A restricted, credentialed-access-only gateway that protects communities from a hostile government is simultaneously invisible to that government's international partners. You cannot be institutionally legible to a global funder while remaining invisible to the national government that funder works with. Restricting the gateway for community safety destroys the transparency that Ostrom's principles require and that the institutional recognition strategy depends on.

The honest resolution is that in contexts where Non-Negotiable 8 applies, TerraCommons operates in a different mode with a different institutional strategy. The two modes are:

*Open-record mode* — for contexts where the government is not an active adversary. The HTTP Gateway is publicly readable. The legitimacy strategy is institutional legibility: TerraCommons seeks World Bank recognition, feeds into national land registry frameworks, and builds the evidentiary base for formal titling. This is the primary mode for the Turkana pilot under current conditions.

*Protected-record mode* — for contexts where the government is an active adversary. The HTTP Gateway is restricted to credentialed institutional partners. The legitimacy strategy shifts from recognition to legal empowerment: TerraCommons becomes evidence infrastructure for litigation, advocacy, and international human rights mechanisms rather than a record seeking formal government recognition. Institutional partners in this mode are legal empowerment organisations, international human rights bodies, and diaspora networks — not government land agencies or their funders.

These are different projects with different partners, different risk profiles, and different success criteria. TerraCommons must be honest about which mode it is operating in at any given deployment, and must not present itself to funders as operating in open-record mode while actually operating in protected-record mode, or vice versa.

**Mode selection and switching process:**

Mode selection is not a one-time deployment decision made by the NGO. It is a community assembly decision, revisable as conditions change. The following process governs both initial mode selection and any subsequent change:

Initial mode selection must be made by a community assembly vote before deployment, based on a documented security assessment prepared by the independent observer. The assessment must cover: documented evidence of government posture toward the community, legal environment for land rights organisations, known surveillance risks, and international partner implications of each mode. The assembly vote must be recorded in the Governance DNA.

Mode switching — from open-record to protected-record or vice versa — requires a new community assembly vote with a new independent observer security assessment. The Land Committee alone may not switch modes. A switch to protected-record mode requires a two-thirds supermajority (the greater security risk justifies the higher threshold). A switch to open-record mode requires a simple majority. Any mode switch must be communicated in writing to all international institutional partners within 30 days.

The mode must be reviewed at each annual community assembly as a standing agenda item, regardless of whether a switch is proposed. Changing threat environments — a new county government, a change in national land policy, increased resource extraction pressure — may warrant a mode change the community did not anticipate at deployment. Annual review ensures the community actively reconfirms their mode rather than drifting in a default that no longer reflects their situation.

**One additional commitment under this Non-Negotiable:** TerraCommons will never misrepresent to funders, institutional partners, or the public which mode a given deployment is operating in. Describing a protected-record deployment as an open, transparent system — because that framing is more fundable — is a violation of this commitment and grounds for any institutional partner to terminate their relationship with the project.

**9. Legal review before deployment in every jurisdiction.** Creating a system that communities use as a de facto land registry — issuing what functions as certificates of land ownership outside the official registry — may be unlawful in the deployment jurisdiction. If TerraCommons' certification process constitutes "registration" within the meaning of applicable statutes, operating without formal authorisation could expose the NGO partner to regulatory action and expose communities to claims that their certifications are legally void. A qualified land rights lawyer in each deployment jurisdiction must review this question and provide a written opinion before any household registers a claim.

*Kenya:* The relevant statutes are the Land Registration Act 2012, the Community Land Act 2016, and the National Land Commission Act 2012. The CLA 2016 creates space for community land documentation with a pathway to legal standing — TerraCommons should be positioned within that framework wherever possible.

*Other jurisdictions:* The equivalent statutory framework must be identified and reviewed. Where no domestic framework provides a clear legal home for community land documentation, TerraCommons operates as an evidentiary system — improving the quality of community evidence — rather than as a parallel registry. This limitation must be communicated to communities in accessible language before deployment.

**10. Data protection compliance in every jurisdiction.** TerraCommons' public HTTP Gateway serves GPS boundary coordinates, claimant identities, household locations, and validation histories. This is personal data within the meaning of data protection law in most jurisdictions. Operating a public gateway that serves this data without lawful basis, appropriate safeguards, and — where required — data subject consent may expose the NGO partner to regulatory penalties and expose claimants to privacy violations.

*Universal principle:* Every deployment must obtain a data protection legal opinion covering: whether TerraCommons processes personal data within the meaning of applicable law; what lawful basis applies; what retention, deletion, and subject access rights apply; and whether the public HTTP Gateway requires any access restrictions to comply.

*Kenya:* The Kenya Data Protection Act 2019 imposes obligations on data controllers and processors of personal data. The Act requires registration with the Office of the Data Protection Commissioner, documented lawful basis for processing, data subject rights including access and erasure, and restrictions on cross-border data transfer. The HTTP Gateway's public accessibility must be assessed against these requirements before deployment. If GPS boundary data and claimant identity constitute personal data under the Act — which is likely — the network's public record layer may require access controls beyond what the current open HTTP model provides.

*Other jurisdictions:* GDPR in the EU, POPIA in South Africa, and equivalent instruments in other deployment contexts impose comparable obligations. Each jurisdiction requires its own legal opinion.

**11. TerraCommons records are not a defence against compulsory acquisition.** This must be communicated to communities before registration begins. In Kenya and most other jurisdictions, governments retain statutory powers of compulsory acquisition that can override community land rights regardless of how well documented those rights are. If a county government invokes compulsory acquisition powers over land where TerraCommons records exist, those records have no legal standing to prevent the acquisition. They may strengthen a compensation claim, support a legal challenge to the process, or provide evidence of prior occupation — but they cannot block a government with statutory acquisition authority from proceeding.

This is a specific scenario distinct from the general "legally worthless" warning. The general warning covers the absence of legal recognition. The compulsory acquisition scenario covers the case where even a sympathetic court might find the evidence compelling but cannot override a statutory acquisition power. Communities facing development pressure, infrastructure projects, or resource extraction in their area are particularly exposed to this risk. TerraCommons should not be presented to such communities as protection against compulsory acquisition. It provides evidentiary standing and a documented record for compensation or challenge proceedings. It does not provide a veto.

---

## Where We Are Now

This governance framework is a design document, not an operational system. No Land Committee has been convened under it. No community assembly has ratified its principles. No Kenyan land rights lawyer has reviewed it. No pastoralist rights specialist has contributed to its treatment of seasonal and communal rights.

What it represents is a serious attempt to design governance that has learned from the failures of previous land rights initiatives — specifically the failures of elite capture, NGO dependency, institutional mission creep, and the gap between technical protocol integrity and social fraud. It is a starting point for a governance design process that must be conducted with the communities it proposes to serve, not for them.

The governance framework will be strong when it has been stress-tested by the people who know Turkana, shaped by the people who understand Kenyan land law, and ratified by the community whose land rights it is designed to protect. Until then it is, at best, a well-reasoned proposal.

---

**Companion Documents:**
- *TerraCommons Vision & Architecture* — What the system is and why it matters
- *TerraCommons Technical Reference* — Illustrative architecture sketches for engineering discussion
- *TerraCommons Pilot Plan* — Detailed 18-month Kenya implementation

**Contact:** Ceri John — topeuph@gmail.com

**© 2026 Ceri John. Licensed under the Apache License, Version 2.0.**