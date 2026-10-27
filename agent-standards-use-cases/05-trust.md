# 5. Trust (TRU)

## Scope, status, and terminology

This document is a problem statement, not a solution proposal. It argues that relying parties encountering unknown agents have no portable, verifiable, sufficiently fresh evidence on which to base risk decisions, and that this gap produces observable failures distinct from anything the identity, authorization, or delegation documents cover. It stops short of prescribing an evidence format, a computation methodology, or — deliberately — any score.

**The word "trust" is used here as a relation, not a property.** Trust is a judgment made *by* a relying party, *about* an agent, *for* a class of action, *in* a context. It is not a quantity the agent carries. This framing is load-bearing for the whole document: two relying parties with different risk tolerances can evaluate the same agent for similar actions and legitimately reach different decisions, and any framing that assigns the agent a single freestanding trust value has already made a solution choice — and, as the prior-art section argues, a historically unsuccessful one. What can be common and substrate-shaped is not the judgment but the **evidence**: attestations, behavioral history, provenance, and standing, carried in a form that is portable across contexts, attributable to its sources, fresh within known bounds, and resistant to manipulation. Judgment stays local; evidence is the shared layer. This is also the boundary with verification and attestation: trust is the relying party's evaluation *of* verified evidence, not a substance produced alongside it.

**This document receives three seams declared earlier in the suite.** Identity (02) established that an authenticated identifier makes an agent *accountable*, not *trustworthy*. Authorization (03) established that a verifiable chain proves who authorized an action, not that the root principal is good for the money. Delegation (04) established that a well-formed mandate from an unknown principal raises, rather than answers, the question of whether to accept it. All three route here. The question this document owns: given verified identity, verified lineage, and a referenceable mandate, on what evidence does a relying party decide whether to proceed?

## The skeptical case

Trust infrastructure has a long history, and the strong skeptic can point to a great deal of it. PKI and TLS establish authenticity of origin; code signing proves source. Platform reputation systems — marketplace ratings, reviews, stars — provide behavioral signals at consumer scale. The attestation economy — SOC 2, ISO 27001, PCI-DSS — provides independent third-party evaluation for exactly the "is this counterparty operationally sound" question. Financial ratings, insurance, and bonding provide standardized, skin-in-the-game answers for firm-level reliability.

The skeptic's deeper argument is structural, and it deserves to be stated at full strength: **trust is inherently subjective and contextual, so it belongs at the application layer.** What counts as trustworthy for a tax preparer differs from what counts for a search assistant; risk tolerance differs per relying party; a substrate-level trust value is a category error. And the commercial world's actual answer to counterparty risk is not reputation scores at all — it is brands, contracts, liability, insurance, and legal recourse. B2B commerce runs on enforceable agreements between identified parties, not on portable trust metrics. Agents operated by companies inherit that machinery: sue the operator, claim against the bond, terminate the contract.

At human scale and human cadence, this case simply holds, and much of it should be conceded rather than fought.

## Where the mismatch actually lies

The subjectivity half of the skeptic's case is correct, and this document adopts it: judgment is local, contextual, and per-relying-party, and nothing below argues for a common judgment. What does not survive the agent regime is the *evidence* situation, on three axes:

**Volume and speed.** The skeptic's machinery — contract negotiation, manual due diligence, reference checks, brand familiarity — operates at human frequency between parties who expect a durable relationship. Agents encounter previously unknown counterparties routinely, at machine speed, often for single transactions. Discovery (next document) will return twenty claimants; something must narrow them before authorization is worth considering, and "negotiate a contract with each" is not an available move. Without evidence, the relying party's options collapse to two, both failing: accept claims on no evidence (indefensible under an adversarial model) or refuse all unknowns (which reduces the ecosystem to hand-curated allowlists and forfeits the point of open coordination).

**Portability and verifiability of what evidence exists.** Behavioral signals today are siloed per platform, expressed in incompatible forms, non-portable across contexts, and mostly unverifiable — a receiving party cannot distinguish independently attested claims from self-asserted ones dressed in the same formatting. An agent's record does not follow it; a well-behaved agent restarts from zero in every new context, which does not scale, while a badly behaved one restarts from zero by choice, which is worse (developed below as whitewashing).

**Evidence about the right subject at the right grain.** The attestation economy evaluates *organizations* annually; agent decisions need evidence about *specific agents* (identity's instance distinction) for *specific action classes* at decision time. A parent company's SOC 2 says little about whether this agent instance handles financial data reliably — and an undifferentiated signal actively misleads, because competence is per-action-class: excellent retrieval history is not evidence about payment handling.

The capability-claim threat model, inherited from the authorization document's seam with discovery, makes the gap adversarial rather than merely inconvenient: claims are free to make, and a claim's existence is evidence of nothing. Some evaluation layer must exist between "twenty agents claim tax expertise" and "one receives my financial records," and today that layer is either rebuilt per application from unverifiable materials, or absent.

## What observably breaks today

**Selection runs on formatting.** Choosing among unknown agents today means judging websites, marketing copy, and self-descriptions — materials whose quality correlates with budget, not behavior. Fraudulent agents present professional certificates, purchased reviews, and paid placements; nothing structural distinguishes them from legitimate ones at the moment of decision.

**Verified and self-asserted claims are indistinguishable on the wire.** A claim of CPA-firm backing, a claim of SOC 2 attestation, and a claim of five years' clean history arrive as identical unverifiable text, whether true or invented.

**Records do not travel.** An agent with a substantial history in one marketplace arrives everywhere else with nothing. Every relying party re-derives from scratch what others already learned, and none of the re-derivation is available to the next party.

**History resets are free.** An agent that defects can retire its identifier and return new. Because history attaches to platform accounts rather than to durable identity, whitewashing costs approximately nothing — and this interacts perversely with cold start: the harsher the treatment of history-less newcomers (the only defense against whitewashing), the higher the barrier facing honest new entrants.

**Context-specificity is an attack vector, not just an accuracy problem.** The cheap path to standing — accumulate flawless history on low-stakes, high-volume tasks, then defect on the first high-stakes one — is the classic marketplace exit scam, and any undifferentiated signal is built to be farmed this way.

**Negative evidence is suppressed.** Parties that experience failures have liability, retaliation, and confidentiality reasons not to publish them; the record that exists skews positive. Meanwhile the recording problem is unsolved in the other direction too: counterparty ratings invite collusion and retaliation; self-reports are worthless. Who truthfully attests that a transaction went well or badly — the oracle problem — is as unsolved as evidence carriage.

**Stale positives mask current risk.** An agent compromised or degraded last week arrives carrying last year's accumulated standing, with no freshness bound distinguishing current from historical evidence.

## Properties without a common definition

Framed as what a relying party must be able to do with evidence, not as features of a scoring system:

- **Distinguish attested from asserted.** Determine which claims carry independent, verifiable backing, from which sources, at which assurance levels — and evaluate the sources themselves (attestor quality, audit-shopping exposure).
- **Validate the instrument.** Where attestation arrives as a credential — the agent's analog of the TLS certificate — validate it by common rules: a format strangers parse identically, a chain to a named issuer, validity checkable at the point of use without the issuer online, revocation that reaches the verifier, and renewal that never disturbs the underlying identity (the credential rotates; the identifier persists, per identity's continuity rules). The instrument is the standardizable carrier of attested evidence; without common validation, "attested" collapses back into formatting.
- **Evaluate per action class.** Obtain evidence differentiated by the class of action contemplated, so retrieval history is not silently spent as payment-handling credibility.
- **Apply local policy to common evidence.** Reach a party-specific decision from party-independent evidence — different cutoffs, different weightings, same underlying record — rather than consuming someone else's judgment.
- **Bound freshness.** Know the age of each item of evidence and obtain recent-behavior signals ahead of accumulated history, with defined ordering when they conflict.
- **Weigh institutional backing against behavioral absence.** Evaluate the newcomer with strong organizational attestation and no track record — the cold-start case — under an explicit policy rather than an accident of formatting.
- **Bound contamination.** Prevent evidence from low-assurance or cheaply minted sources from influencing the standing of high-assurance subjects; sybil resistance of the evidence base itself.
- **Follow the subject.** Attach evidence to durable agent identity (identity's continuity and instance rules) so records survive infrastructure changes and cannot be shed by re-minting — with retirement preserving history, per the identity document's lifecycle requirement.
- **Compose heterogeneous evidence.** Combine certifications, transparency-log history, organizational standing, and bonding without a mandated single methodology — composition rules are local; comparability of inputs is the common requirement.

## Prior art, and what it constrains

**Credit bureaus** are the analogy earlier drafts leaned on, and the honest reading cuts both ways. The system demonstrates portable, multi-source, automated-decision evidence at national scale — and it exists because of law, not technology: a regulated oligopoly under statutory accuracy and dispute obligations, computing toward a *single* well-defined target (default probability). Agent trust has no single target — the relation is per-action-class by nature — and no legislature standing behind it. The bureaus also model the failure costs: centralized evidence stores as breach targets, governance capture, and an industry devoted to gaming the score. The transferable lesson is narrower than the analogy suggests: different lenders apply different cutoffs to the same underlying data — evidence common, judgment local — which is precisely the framing this document adopts.

**The WebPKI at its base tier** is the deployed proof that the certification half of this problem standardizes: X.509 formats and validation rules are common, every relying party validates identically without bilateral setup, certificate authorities compete as businesses above the standard, and Let's Encrypt demonstrated what happens when issuance is priced as attestation rather than as allocation — the price of the baseline instrument fell to zero while the validation standard held. The lesson cuts precisely: the instrument and its validation standardize; the decision to rely, and everything above baseline domain-control assurance, stays with the relying party and the market.

**EV certificates** are the deployed cautionary tale for graduated trust signals: a cryptographically sound, independently verified assurance tier that browsers eventually stripped from the UI because it demonstrably failed to change relying-party behavior. A signal's soundness does not make it consumable.

**PGP's web of trust** proved decentralized trust computation possible and unusable; its adoption failure is the standing warning for any scheme requiring relying parties to curate trust graphs.

**The sybil-resistance literature** sets a floor: Douceur's result and its successors (EigenTrust-family reputation aggregation, SybilGuard-style social defenses) establish that without some costly or externally anchored identity, sybil manipulation of open reputation systems cannot be prevented — binding this document's contamination requirement directly to the identity document's issuance-cost and lifecycle questions.

**Marketplace reputation research** documents the pathologies now inherited: rating inflation toward uniform positivity, retaliation-driven suppression of negatives, collusive boosting, and whitespace re-entry (whitewashing). These are dynamics of the evidence-generation game, not bugs of particular platforms.

**Certificate Transparency** is the deployed existence proof for the transparency-log evidence model — append-only, publicly auditable, monitorable — and a demonstration of its operating costs (log governance, monitoring economics).

**The attestation economy (SOC 2, ISO 27001)** supplies the independent-attestor model and its known failure mode, audit shopping; its grain (organization, annual) versus the needed grain (agent, action class, decision time) is the gap stated above.

**Insurance and bonding** are structurally different prior art: skin-in-the-game replaces prediction — the relying party is made whole rather than made prescient. A problem statement should not foreclose this family; recourse-based standing may be evidence, may complement behavioral history, and reaches the cold-start case (a bonded newcomer is a different risk than a bare one) in a way pure history cannot.

The constraint the history imposes: portable evidence, decentralized computation, transparency logs, independent attestation, and recourse mechanisms each exist somewhere. What nothing provides is verifiable, action-class-differentiated, freshness-bounded evidence attached to durable agent identity across contexts — and the literature's hard results (sybil floors, signal-consumption failures, oracle pathologies) bound what any solution may claim.

## The core use case: selecting among many claimants

An individual needs an agent for tax preparation and receives twenty claimants. Some are established services with real records; some are new but honest; some are fraudulent, built to harvest financial data or deliver deliberately wrong advice. The individual must decide which, if any, receives their financial records and a mandate to act.

**What breaks today.** Every distinguishing signal available at the moment of decision is producible by all three groups. Certificates, testimonials, professional presentation, and platform ratings are either self-asserted, purchasable, or platform-siloed; verified and invented claims render identically. Research is manual, duplicated per claimant and per deciding party, and non-portable to the next decision. The honest newcomer and the whitewashed defector are indistinguishable — both present clean slates. And the record consulted, wherever one exists, is undifferentiated: strong general ratings say nothing about conduct with sensitive financial data, which is the action class actually at stake.

**What any adequate answer must make possible.** The relying party can distinguish independently attested claims (state-board CPA verification, audited membership, organizational backing with its own standing) from self-asserted ones; can obtain behavioral evidence differentiated by action class and bounded in freshness; can see that evidence attached to a durable identity that infrastructure changes and re-minting do not reset; and can apply its own thresholds — a cautious retiree and a risk-tolerant startup reaching different decisions from the same record, both correctly.

**What no answer can promise, and this document does not claim.** That fraud is prevented — fraudulent agents will appear, acquire cheap attestations, and farm low-stakes history; the achievable property is that absence of independent, action-class-relevant evidence is *visible* at decision time rather than papered over. That the cold-start/whitewashing tension is escaped — any system either burdens honest newcomers or subsidizes identity resets, and the achievable requirement is a defined, inspectable position on that tradeoff, not its absence. That the oracle problem is solved — truthful outcome recording under collusion, retaliation, and suppression incentives has no known clean answer; the achievable requirement is that evidence carry its provenance so relying parties can discount accordingly.

**Open questions this use case exposes.** Where evidence lives (per-agent carried credentials, transparency logs, attestor-hosted, some mix); who the attestors are and what disciplines them; and how recourse-based standing (bonding, insurance) composes with behavioral evidence — all solution-space, all downstream of the failure modes, which stand regardless.

## Additional trust needs

Stated as relying-party decisions or present-day failures; mechanism language is deliberately avoided.

**Decision-time evaluation**
- Decide whether a capability claim encountered at discovery is worth acting on, given that claims are free to make. *(The seam authorization (03) declared with discovery lands here.)*
- Evaluate an agent for a high-stakes action class when its record exists only for unrelated low-stakes classes.
- Evaluate a newcomer with institutional attestation and no behavioral history under an explicit policy for that combination.
- Require, per local policy, a minimum evidentiary basis for classes of action — and express that requirement against common evidence rather than per-platform improvisation.

**Evidence integrity**
- Distinguish independently attested claims from self-asserted ones as a structural fact rather than a formatting inference.
- Verify a certification claim against its issuing authority without per-authority bespoke integration.
- Validate the certification instrument itself by common rules: parse, chain, expiry, and revocation status, at the point of use, offline where the deployment requires it.
- Accept certifications from multiple independent issuers under local acceptance policy, with no single mandatory root.
- Survive credential rotation and issuer transitions without loss of the subject's evidence history.
- Detect and bound the influence of manufactured evidence — purchased ratings, collusive boosting, sybil sources — on the standing of legitimate subjects.
- Discount evidence by the demonstrated reliability of its source, including attestors.

**Evidence over time**
- Weigh recent negative evidence against accumulated positive history under defined ordering, so standing degrades when behavior does.
- Bound the staleness of evidence consumed in automated decisions.
- Preserve evidence across the subject's infrastructure changes and retirement *(identity's continuity and retirement-with-history rules)*, and deny evidence resets via identity re-minting *(identity's issuance-cost question)*.

**Evidence across contexts**
- Carry evidence earned in one context into evaluation in another, with the context of its earning visible rather than laundered away.
- Support divergent decisions by different relying parties from the same evidence, without forcing a shared judgment.

## What this document does not claim

- **Not** a single global trust score, nor any mandated computation. The score framing of earlier drafts is retired: the relation is per-party, per-action-class, per-context, and the prior art (EV's consumption failure, the bureaus' statutory preconditions) argues against a universal scalar even as a solution candidate. The claim is confined to evidence properties.
- **Not** that sybil-resistant reputation is achievable over free identities. The literature's floor stands; contamination bounds inherit the identity document's issuance-cost problem rather than escaping it.
- **Not** that the cold-start/whitewashing tradeoff can be dissolved — only that a solution must occupy a defined, inspectable position on it.
- **Not** that the oracle problem is solved, or that behavioral evidence can be made incorruptible — only that provenance travel with evidence so discounting is possible.
- **Not** that evidence displaces contracts, insurance, bonding, or legal recourse. These are complements, and recourse-based standing is itself a candidate evidence family.
- **Not** that trust judgments can or should be made objective or common. Judgment is local by construction here; commonality is claimed only for the evidence layer.

The claim is: relying parties facing unknown agents at machine speed and volume have no evidence layer — no way to distinguish attested from asserted, no record that travels with the subject, no differentiation by action class, no freshness bounds, no resistance to resets and manufactured standing — so selection today runs on formatting, fraud is structurally indistinguishable from legitimacy at decision time, and the only scaling alternatives are blind acceptance or closed allowlists, both of which fail the open coordination the rest of this suite describes.

## Forward and backward dependencies

- **Identity (02), backward:** receives the accountable/trustworthy distinction as its founding premise; depends on identifier continuity and instance distinction for evidence attachment, on retirement-with-history for record preservation, and on the issuance-cost question for sybil and whitewashing bounds. Trust without durable identity is not merely weak — it is undefined.
- **Authorization (03), backward:** receives the root-worthiness question — lineage proves who authorized, evidence informs whether to transact; trust evaluation is an input to the grant decision, not part of chain verification.
- **Delegation (04), backward:** receives the mandate-acceptance question — whether a well-formed mandate from an unknown principal is worth honoring is evaluated on the evidence described here.
- **Discovery (06), forward:** discovery returns claimants and unverified capability claims; the evaluation layer between discovery results and authorization decisions is this document's subject, and evidence-based filtering at discovery time is jointly owned. Contamination of discovery results by manufactured standing is a shared threat.
- **Audit, forward:** behavioral evidence is generated substantially from the audit layer's records; the oracle problem — who truthfully records outcomes — is jointly owned, and the audit document must say what its records can and cannot attest.

---

[← Delegation](04-delegation.md) &nbsp;·&nbsp; [README](README.md) &nbsp;·&nbsp; [Discovery →](06-discovery.md)
