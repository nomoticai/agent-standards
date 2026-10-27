# 12. Audit (AUD)

## Scope, status, and terminology

This document is a problem statement, not a solution proposal. Its subject is **evidence of agent action**: records cryptographically attributed to specific agents, preserving delegation chains, selectively disclosable, tamper-evident in sequence, and verifiable across organizational boundaries without the cooperation of the party under scrutiny. It argues that neither of the two deployed classes of audit infrastructure provides these properties for continuous, cross-organization agent activity, and it stops short of prescribing data structures, storage architectures, or cryptographic constructions — tamper evidence, not hash chains; selective disclosure, not Merkle proofs; the properties, with the constructions left to the solution space.

**This document is where the suite's receipts come due.** Nearly every preceding document declared an audit seam, and they are received here as this document's inbound requirements: transport's single causal trace across a fragmented workflow (01); attribution that joins on a common agent identifier across stacks (02); the chain records behind the finance-reconciliation query (03); the mandate as the fixed artifact disputes reference (04); the behavioral evidence that trust decisions consume — together with that document's explicit debt, that audit must state what its records can and cannot attest (05); the session record of membership, transitions, and partial-failure accounts (08); the bound act record that makes transactions adjudicable (09); the single correlated record across tool and agent steps, and invocation-time integrity evidence (10); and the dual-held transaction evidence of commerce (11). The refrain running through the suite — *recorded, referenceable, disputable* — cashes out here or nowhere.

## The skeptical case

Audit infrastructure is abundant and mature. Application logging is universal; aggregation and SIEM platforms centralize it at enterprise scale; cloud providers record every API call in their environments; compliance regimes (SOX, HIPAA, PCI-DSS, GDPR) impose established reporting patterns; and the high-integrity end is now standardized — Certificate Transparency proved the transparency-log pattern at internet scale, SCITT generalizes it for signed statements with inclusion proofs, and signing infrastructure for artifacts is deployed. The skeptic's composition: SCITT for high-consequence records, application logs for routine activity, SIEM for aggregation, cloud trails for API access. Every audit case an application has, covered by something shipping.

## Where the mismatch actually lies

The original two-level critique stands and is retained, sharpened.

**Level one: the audited party holds the pen.** Application logs are written by the application being audited — its format, its granularity, its attribution, its retention. Buggy applications log incompletely; compromised applications log falsely; decommissioned applications stop logging retroactively, by losing what they logged. Aggregation centralizes this dependence without removing it: the SIEM holds what applications chose to send. Cloud trails are stronger inside one provider's boundary and end at it. The deployed cheap countermeasure — ship logs off-host so the audited host cannot rewrite them — defeats storage tampering and not emission tampering: the party that controls the host controls what is emitted in the first place. This is the coverage problem, and it is developed below because it bounds everything.

**Level two: the high-integrity systems have a different design center.** CT records discrete issuance events; SCITT records discrete signed claims; ledgers record discrete state changes; compliance reporting is periodic and coarse. None was designed for continuous per-action attribution at machine speed, with delegation chains preserved through multi-hop, multi-organization workflows, queryable selectively. The stance toward them is composition, not replacement: the highest-consequence public claims belong on transparency services; the gap is the continuous, chain-preserving, selectively disclosable evidence of ordinary agent activity underneath.

**What agent activity specifically stresses.** Actions execute at machine speed under delegated authority spanning organizations; accountability questions require walking a causal chain from a concrete action to an originating principal; investigations proceed without full cooperation from every participant; and answering a narrow question must not require disclosing an entire activity history. Under those conditions, the deployed classes deliver: attribution to service accounts and processes rather than agents (02's attribution collapse, at the record layer); chains reconstructed from cross-system correlation rather than preserved (03's archaeology); timestamps as the correlation mechanism of last resort; dependence on the goodwill of the parties under scrutiny; and no basis for trusting that the records are complete or unaltered.

## What observably breaks today

**Attribution ends at the service account.** The payment system's log names a credential; which agent, under which delegation, acted through it is not in the record — the shared-credential failure of document 02, now at the moment it matters most.

**Chains are reconstructed, not preserved.** The delegation lineage behind an action exists, if at all, as an investigator's correlation across systems using different identifiers — and the correlation degrades with every rotated log, decommissioned system, and clock skew.

**Cross-organization investigation runs on cooperation.** Two organizations disputing whether a delegated action was authorized each hold only their own application logs; there is no record both can trust, and an uncooperative or defunct participant stalls the investigation at their boundary. *(Reviewer's cases, embedded: the external auditor who cannot compel production; the participant whose logs are rotated, lost, or suspect.)*

**Narrow questions cost whole histories.** Proving that one check was performed and what it returned currently means producing logs that reveal everything adjacent — so disclosure is over-broad, or refused, or negotiated at legal expense.

**Completeness is unverifiable, and incompleteness is sometimes rational.** Nothing distinguishes "no record because it didn't happen" from "no record because it wasn't logged, was suppressed, or happened off-substrate." And the incentive runs the wrong way: everything recorded is discoverable in litigation, so organizations rationally minimize logging — the pressure against complete evidence is structural, not sloppy.

**The record is a target and a liability in itself.** A continuous per-agent record of agents acting for human principals is a detailed record of the principals' lives — a surveillance instrument if misheld, a concentrated breach target (document 05's bureau lesson recurring), and a standing collision with erasure rights: append-only tamper evidence versus the legal right to be forgotten is a known, unresolved conflict, not a detail.

## Properties without a common definition

Framed as what an investigator, counterparty, or regulator must be able to establish — with mechanisms deliberately unspecified:

- **Attribution:** which specific agent (02's identifier, instance-distinct) performed the action, verifiable from the record itself, independent of trust in the logging infrastructure.
- **Chain preservation:** the delegation lineage from action back to originating principal, carried in the record rather than reconstructed around it. *(03/04's chains and mandates, as record content.)*
- **Tamper-evident sequence:** alteration, insertion, deletion, or reordering of an agent's action history leaves evidence — property, not construction.
- **Selective disclosure:** proof that a specific action occurred, with its authority and result, without revealing unrelated activity — and the bounded converse, developed below.
- **Cross-boundary verifiability:** evidence checkable by parties outside the organization that generated it, without that organization's ongoing cooperation — including counterparty-signed responses captured at interaction time, so the external blacklist service's answer is in the record rather than in the external service's goodwill.
- **Defined coverage:** what is and is not recorded is itself a stated, verifiable property of the system. This is the property the earlier drafts missed and the one that makes the others meaningful: absence-proofs are statements about a *defined* record, unbounded coverage is legally and ethically indefensible, and undefined coverage is how "complete" logs lie by omission.
- **Custody and retention with the collision named:** where records live, who holds them, for how long, and how erasure obligations are met against tamper evidence — the operator question's fifth appearance in this suite, plus a legal tension no architecture dissolves.
- **Time:** action times verifiable to a standard better than cross-system clock correlation.

## Prior art, and what it constrains

**Certificate Transparency and SCITT** anchor the high-integrity end and are engaged as composition targets, exactly as earlier drafts had it: discrete, high-consequence, publicly verifiable claims are their design center and should remain their job.

**Estonia's KSI infrastructure** is the constructive existence proof the earlier drafts lacked: hash-linked integrity for government records, nationally deployed for over a decade — continuous tamper evidence at state scale is buildable and operable. Its scope is also its lesson: one government, one operator, one legal regime.

**OpenTelemetry distributed tracing** is the deployed answer to cross-system causal correlation — trace context propagating across services, reconstructing multi-hop workflows routinely — inside cooperating deployments, with no integrity guarantees and attribution to services, not agents: the suite's recurring single-trust-domain pattern (Temporal for sessions, Unix for tools), here for causality. **W3C PROV** is the standards-track attempt at causal provenance encoding, and its academic-bound adoption is the warning for chain-preservation formats. **RFC 3161 and eIDAS-grade timestamping** are the deployed partial answer to the time property. **Remote syslog** is the deployed partial answer to level one, with emission-suppression as its documented limit.

**The financial audit regime** is the deepest prior art and was missing entirely. Double-entry bookkeeping is the ancestral dual-held record — document 11's offer/acceptance evidence is double-entry across organizational boundaries, and the regime's five centuries of practice are the tradition this layer extends. Regulation-mandated WORM retention (SEC 17a-4) is deployed, legally enforced tamper evidence. And the independent-auditor profession is the institutional answer to level one — a licensed third party attests because the audited party's own records cannot be self-certifying — with audit-shopping (document 05) as its standing flaw. The regime's composite lesson: evidence integrity at societal scale has never been purely cryptographic; it is cryptography-or-paper *plus enforcement*.

**The three analogies are retained, each now carrying the limit that proves that lesson.** The **notary journal** — per-notary, chronological, tamper-evident, legally weighty — works because the notary is licensed, bonded, and personally liable; the journal is inseparable from the regime that punishes its falsification. **Chain of custody** works because an adjudicator with teeth excludes gapped evidence; the documentation has force only inside a court that enforces the rule. The **flight recorder** works because regulation mandates its presence and criminalizes tampering — and its honest details are the strongest material in this document: the FDR records a *bounded, specified parameter set* on a *self-erasing loop*, a deliberate settlement of the completeness-versus-privacy fight (cockpit recording was resisted by pilots' unions; the loop was the compromise). Aviation's gold standard of audit is intentionally incomplete, intentionally self-erasing, and effective anyway — because coverage is defined and the regime is real. All three analogies converge on the same conclusion: **the record's integrity is purchased by an enforcement regime, not produced by a data structure** — which is this document's cleanest handoff to governance (13).

## The core use case: investigating a payment that should have been blocked

Finance discovers a cleared payment to a supplier blacklisted after a prior compliance incident. Money moved; the organization must establish exactly what occurred. The investigation's questions, in sequence: which agent submitted the authorization; under what delegation; what the agent saw when deciding; whether the blacklist was checked and what it returned; whether an upstream delegation overrode the blacklist or the check failed silently; whether the blacklist was current at check time; and whether the agent's other actions raise the same concern.

**What breaks today.** The payment log attributes the authorization to a service account. The active delegation at decision time survives, or doesn't, per the agent framework's private logging, and reconstructing the chain means joining systems that share no identifiers. The blacklist service's log shows *a* check; linking it to *this* decision is timestamp-and-parameter correlation of varying reliability — and the blacklist is externally operated, so its records arrive by cooperation or subpoena or not at all. Rotated logs, decommissioned systems, and dropped entries subtract evidence invisibly. Every step rests on trusting that each application's logs are complete and honest — the exact property level one says cannot be established from outside.

**What any adequate answer must make possible.** The authorization is attributed, in the record, to a specific agent under a specific delegation — the chain walkable to the principal. The record of the blacklist query includes the externally-operated service's own signed response, captured at interaction time, verifiable without that organization's later cooperation. Sequence integrity lets the investigator trust the ordering — the check *before* the payment, or damningly absent. The specific facts are provable without disclosing the agent's unrelated activity, and "no such check occurred" is a meaningful statement because the record's coverage is defined and its sequence is tamper-evident.

**What no answer can promise, and this document does not claim.** *Truth of content:* tamper evidence authenticates that a statement was made, by whom, in what sequence — a signed lie chains perfectly, and a record fed poisoned inputs faithfully preserves the poison. *Attestation beyond the coordination surface:* the record captures what was claimed, signed, and exchanged on the substrate — not whether delivered goods matched (11's fulfillment residue), not whether the model's reasoning was sound, not off-substrate facts of any kind; this is document 05's oracle problem answered as that document required: audit's records attest the observable exchange, and behavioral evidence built on them inherits exactly that scope. *Completeness as cryptography:* no construction proves that everything that happened entered the record — off-substrate action leaves no trace, compromised participants suppress emission rather than alter storage, and absence-proofs are claims about a defined record, not about the world. Completeness, where it exists, is purchased by regime — mandated coverage, penalized suppression — which is governance's territory, not this layer's gift. *Privacy transcendence:* selective disclosure narrows queries; it does not resolve retention against erasure rights, and a solution owes a defined position on that collision, not a claim to have dissolved it.

**Open questions this use case exposes.** Custody of the records (per-agent journals, organizational stores, third-party holders, transparency services — each with the suite's catalogued costs); the coverage specification for agent activity (the FDR parameter-list question); and how record retention meets erasure law — solution-space and legal-space; the failures stand regardless.

## Additional audit needs

Stated as investigative or accountability failures; ownership tags per the suite.

**Attribution and chain**
- Establish which specific agent performed a given action, from the record, without trusting the logging host. *(02.)*
- Walk from any recorded action to its originating principal through the preserved chain. *(03/04.)*
- Join one workflow's records across stacks, tools, and organizations on common identifiers. *(01/08/10's correlated record, received.)*

**Verification across boundaries**
- Verify a participant's recorded conduct after that participant becomes uncooperative, defunct, or suspect.
- Rely on counterparty-signed responses captured at interaction time rather than on later production of the counterparty's logs.
- Resolve a two-organization dispute over a delegated action from records both can verify. *(04's dispute path; 09's adjudicability; 11's dual-held evidence — received.)*

**Disclosure and coverage**
- Prove a specific action, authority, and result while revealing nothing adjacent.
- Make "no such action is recorded" a meaningful statement, by defined coverage plus sequence integrity — and never conflate it with "no such action occurred."
- State, and let counterparties verify, what the record covers and what it deliberately omits.

**Integrity over time**
- Detect insertion, deletion, and reordering in an agent's action history, including by the record's custodian.
- Anchor action times to verifiable timestamps rather than cross-system clock agreement.
- Retain evidence across agent retirement *(02's retirement-with-history, received)* and organizational change, against a defined retention-and-erasure position.

**Evidence for other layers**
- Supply the behavioral record trust evaluation consumes, scoped to what the record can attest. *(05's debt, discharged above.)*
- Supply the session's membership, transition, and partial-failure accounts as evidence. *(08.)*
- Supply invocation-time integrity evidence for tool participation. *(10.)*

## What this document does not claim

- **Not** any construction. Hash chains, Merkle inclusion, and transparency logs are illustrative mechanisms from the prior art; the claims are the properties.
- **Not** that tamper evidence establishes truth, that records attest off-substrate facts, or that completeness is achievable cryptographically. The record authenticates the observable exchange; coverage is a defined boundary, not the world; completeness is a regime outcome.
- **Not** a replacement for transparency services, the auditing profession, or compliance regimes — composition with the first, extension of the second's record layer, input to the third.
- **Not** an escape from the privacy collision. Continuous per-agent records of principal-serving agents are dual-use by nature; erasure law versus tamper evidence is named and left to be answered, position by position.
- **Not** self-enforcing. Every analogy in this document works because a regime licenses, mandates, penalizes, or excludes; the evidence layer supplies what such a regime needs and cannot supply the regime.

The claim is: agent activity that crosses organizations through delegation chains at machine speed generates accountability questions that today can only be answered by correlating application-controlled logs across systems that share no identifiers, held by parties who may be uncooperative, incomplete by unverifiable amounts, and over-disclosing by design when they answer at all; the evidence properties required — per-agent attribution, preserved chains, tamper-evident sequence, selective disclosure, cross-boundary verifiability, defined coverage — exist piecewise across prior art and nowhere in combination; and this layer is where every preceding document's "recorded, referenceable, disputable" either becomes real or remains a phrase.

## Forward and backward dependencies

- **Backward, the received seams:** 01 (causal trace, workflow correlation), 02 (identifier joins, retirement-with-history), 03 (chain records, reconciliation), 04 (mandate reference, dispute path), 05 (behavioral evidence scoped by the attestation limits above — debt discharged), 08 (session record), 09 (bound acts, adjudicability), 10 (correlated tool/agent record, integrity evidence), 11 (dual-held transaction evidence as double-entry across organizations).
- **Governance (13), forward:** receives the enforcement question this document's every analogy raises — who mandates coverage, penalizes suppression and tampering, licenses custodians, and gives the evidence force in dispute. Audit supplies the record; governance supplies the regime; the notary without the notary law is a person with a nice notebook.

---

[← Commerce](11-commerce.md) &nbsp;·&nbsp; [README](README.md) &nbsp;·&nbsp; [Governance →](13-governance.md)
