# 13. Governance (GOV)

## Scope, status, and terminology

This document is a problem statement, not a solution proposal — and, as the closing document of the core set, it carries two jobs. Its primary subject is **accountability**: the durable, verifiable bind between agent action and a natural or legal person that legal systems can act upon. Its second job is to receive what the rest of the suite has been accumulating: every preceding document's analogies and requirements quietly presupposed an enforcement regime, and audit (12) handed the question over explicitly — the record supplies evidence; something else must give evidence force. Both jobs are done here, with the second collected and *named as unsolved* rather than pretended solved.

**The central distinction, stated first because everything follows from it.** Audit provides *attribution*: cryptographic evidence of which agent performed which action under which chain. Governance concerns *accountability*: which natural or legal person is responsible, in a form a court, regulator, or counterparty can act on. Attribution stops at the substrate boundary; without a bind from the chain to a person, legal action cannot proceed from it. This is the identity document's accountable/trustworthy distinction completing its arc — an authenticated identifier makes an agent traceable; only a bind to a legal person makes anyone *answerable*.

**A limit on what any substrate can claim, adopted from the outset.** A substrate can produce evidence with strong integrity, non-repudiation, clear scope, and an identifiable person behind it. Whether legal systems treat that evidence as they treat a power of attorney, a corporate resolution, or a qualified electronic signature is **recognition — and recognition is conferred, by legislatures and courts, not produced by design.** This document describes the properties that make recognition *possible and attractive*; it does not assert recognition will follow, and the phrase "legal-instrument-grade" from earlier drafts is retired as circular (grade is what legal systems confer).

## The skeptical case

Accountability infrastructure is among civilization's oldest and most developed. Contract law binds parties in essentially every jurisdiction; corporate law defines which officers bind which entities to what; legal-entity identifier systems (LEI, D-U-N-S, national registries) name legal persons at global scale; digital-signature statutes (eIDAS, ESIGN, UETA and counterparts) give electronic signatures the weight of ink; product liability holds software makers answerable for defects; AI-specific regulation is arriving by risk tier and sector; and enforcement bodies exist with jurisdiction and teeth.

The strong skeptic adds the point earlier drafts missed: **the law already crossed the automation threshold.** UETA's electronic-agent provisions, enacted in 1999, expressly contemplate contracts formed by software acting for a principal, and hold them binding. Automated contracting is not a legal novelty — it is a quarter-century-old statutory fact, and every algorithmic trading system and programmatic ad exchange runs on it daily. The skeptic's conclusion: agents are software; the deploying company is the accountable party; UETA-class provisions already bind automated commitments; when something goes wrong, contract law, agency doctrine, and product liability handle it as they have handled everything else.

## Where the mismatch actually lies

The skeptic's strongest point is conceded and repurposed: the law answered *whether* software can bind a principal long ago. What it did not answer is everything this suite describes agents actually doing — and the gaps are specific.

**The statutes bind; they do not verify.** UETA-class provisions make the automated commitment enforceable and say nothing about how a counterparty verifies, at machine speed, *which* principal stands behind *this* agent, under *what* scope, through *what* chain. The algorithmic systems that run on those provisions today operate under pre-negotiated bilateral agreements between identified parties — precisely the arrangement the suite's stranger-to-stranger, discovery-time, cross-organization pattern removes.

**Scale breaks the review assumption.** Contract and corporate-authority law scale through human attention: an entity is party to as many commitments as its officers can review, and signing authority is exercised deliberately, act by act. Agents commit at rates no review capacity covers, for principals absent at commitment time. The choice this forces, absent a better bind, is the unattractive one: human sign-off on every significant act (defeating delegation at scale) or broad, unpredictable liability defaults (discouraging deployment).

**The product frame misfits the representative.** Product liability fits software with author-specified behavior, where harm maps to defect. An agent's outcomes emerge from design *plus* delegation *plus* circumstance; "the product malfunctioned" and "the representative made a poor decision within scope" become indistinguishable, and the law's clean categories — defective tool, faithless employee, authorized deputy — each fit partially. Agency law is the closest fit, and engaging it honestly is where this document's core case gets harder, not easier (below).

**Attribution without a bind stops at the boundary.** Everything documents 02–12 construct — identifiers, chains, mandates, evidence, records — terminates, at its most consequential moment, in the question *who do we sue, regulate, or compel*, and today that question is answered by corporate archaeology through the same application-layer records every prior document found wanting.

**The analogies, kept, each carrying its shape and its cost.** The **power of attorney** is the target shape: an instrument that makes the representative's in-scope acts the principal's own, durable across time and institutions — and it works inside a regime of execution formalities and court recognition. The **corporate signing officer** is the delegation shape: the corporation bound, not the officer personally — by grace of corporate law's recognition of the authority structure. The **ship captain** is the cross-border shape: on-scene authority binding a distant principal in foreign ports — built by centuries of maritime practice *and treaties*. The **notarization** analogy is inherited from audit (12) with that document's conclusion intact: the journal has force because notary law exists. Every analogy is an instrument *plus a regime*; none is an instrument alone.

## What observably breaks today

**The accountable person is archaeology.** Establishing who stands behind a deployed agent — through the operating subsidiary, the cloud account, the API key owner, the departed employee's project (02's orphans, at the liability layer) — is an investigation, not a lookup, and it is conducted through records the investigated parties control (12's level-one problem, at the legal layer).

**Authority disputes are asymmetric-evidence contests.** When a principal claims its agent exceeded scope, the granted scope lives in the principal's internal systems; the counterparty has no independent evidence of what was actually authorized, and the court weighs two incomplete, differently formatted, self-serving accounts. *(Reviewer's case, embedded — and doc 04's mandate-as-prompt-text failure arriving in litigation.)*

**Evidence does not travel across borders.** Authorization evidence persuasive in the principal's jurisdiction may be inadmissible, unweighted, or simply unfamiliar where the action occurred or enforcement is sought — application logs least of all.

**Accountability does not survive transitions.** Corporate sales, restructurings, and agent reassignments move responsibility with no mechanism counterparties or courts can check; who answers for ongoing sessions and historical actions after the subsidiary is sold is currently a contract-drafting exercise per deal, when it is remembered at all. *(08's succession and 02's retirement, at the liability layer.)*

**Composed workflows have no responsibility segmentation.** When agents of several accountable persons jointly produce a bad outcome, nothing at any layer apportions which person answers for which segment — joint-and-several by default, litigated by archaeology.

**Where the anchor is absent, courts improvise.** The DAO cases are the live exhibit: action and commitment arriving with no clearly accountable person attached, met by regulators and courts stretching doctrines to reach *someone* — token holders, deployers, front-end operators. The lesson runs in this document's favor and against complacency simultaneously: legal systems will not tolerate an accountability vacuum, and what they improvise under incident pressure is worse for everyone than what could be designed.

## The core use case: a commercial dispute over an agent's authority to bind

An organization grants its assistant agent authority to enter procurement contracts within a defined scope. The assistant contracts with a supplier for a significant purchase. On delivery, a term is disputed: the supplier says the assistant accepted risk-transfer at port of origin; the buyer says the assistant had no authority to accept those terms. The value justifies formal proceedings. The court's question: did the assistant have authority to bind the buyer to the disputed terms?

**The doctrine this walks into, engaged rather than elided.** This is the actual-versus-apparent-authority question, and agency law has answered it for two centuries — not always the way the buyer hopes. Under apparent authority, a principal whose conduct led the counterparty reasonably to believe authority existed can be bound *even where actual authority was exceeded*; under ratification, a principal who accepts the benefits of an unauthorized act can be bound after the fact. Document 04 installed this doctrine as prior art (actual-versus-apparent *is* the live-mandate/valid-credential gap); here it lands with consequences: **the buyer can hold perfect proof that scope was exceeded and still lose.** Machine speed sharpens both horns — agents "hold out" other agents thousands of times a day, and benefits are accepted (goods booked, payments settled) before any human could ratify or repudiate.

**What breaks today, precisely.** The factual record is two private, incompatible archives: the buyer's application logs and agent configuration; the supplier's logs and its agent's account of the exchange. Neither side holds independent evidence of the *other's* authorization structure; the court weighs credibility across novel, unstandardized material; the proceeding is slow, expensive, and jurisdiction-lottery-dependent in its treatment of agent-mediated evidence.

**What substrate evidence genuinely changes — the factual layer.** With the mandate as a referenceable artifact (04), the acts as bound records (09), the chain preserved in audit evidence (12), and the accountable persons identified from the bind this document describes, the *factual* questions collapse: what scope was granted, when, by whom; what the counterparty's agent was shown or could verify; whether the disputed term fell inside or outside the recorded scope. Both parties and the court examine one record instead of two stories. That alone converts the proceeding's hardest phase from credibility contest to record inspection.

**What substrate evidence does not decide — the legal layer.** Who bears the risk of excess remains doctrine's question. And here the honest, sophisticated version of the claim appears: verifiable, substrate-visible scope doesn't merely *inform* the reasonable-reliance analysis — it potentially *shifts its baseline*. If the counterparty could have verified the published scope at commitment time and did not, its reliance argument weakens; if scope was verifiable and the term checked out, the principal's repudiation weakens. Whether courts and legislatures adopt that shifted baseline — making scope-verification the commercial norm the way signature-verification became one — is a recognition outcome, decided outside the substrate. The claim is confined to making the shift *possible*: evidence exists to shift onto.

**The cross-border extension, with the original's honesty kept.** Evidence designed for the properties digital-signature regimes already recognize is *portable* in a way application logs are not; enforcement across borders still requires treaty bases, mutual recognition of judgments, or arbitration clauses — jurisdictional variation rides on top of the mechanism, and the mechanism removes none of it.

**Open questions this use case exposes.** How agent-scope verification interacts with apparent-authority doctrine (a question for courts and commercial-law reform, flagged, not answered); whether machine-speed ratification needs a defined repudiation window (a legislative question); and which existing statute family — electronic-agent provisions, digital-signature regimes, or new instruments — is the recognition vehicle. All legal-space; the evidentiary failures stand regardless.

## Properties without a common definition

Framed as what courts, regulators, and counterparties must be able to establish — with the recognition caveat governing throughout:

- **The bind:** a durable, verifiable association between an agent (02's identifier) and an identifiable natural or legal person answerable for its in-scope actions — checkable by counterparties *before* commitment, not excavated after.
- **Scope visibility:** the granted scope (04's mandate) examinable by the counterparty and later by the court, so the factual layer of authority disputes is record inspection.
- **Chain-following accountability:** where delegation chains connect actor to principal (03/04), responsibility follows the chain within scope — with excess-of-scope allocation left to doctrine, explicitly.
- **Evidentiary properties recognition can attach to:** integrity, non-repudiation, and formality comparable to already-recognized electronic-signature evidence; clear scope; identifiable person — the property set, not the conferred status.
- **Transition semantics:** transfer of the bind across corporate sales, restructurings, and reassignments, and revocation of it, through mechanisms counterparties and courts can verify — with historical accountability defined, not orphaned. *(02's lifecycle and 08's succession, at the liability layer.)*
- **Segmentation in composition:** in multi-principal workflows, which accountable person stands behind which segment, establishable from the record. *(10's participation fabric, with persons attached.)*
- **Anchoring to legal-person infrastructure:** composition with the deployed legal-entity identifier systems, so the person the bind names is the person registries and regulators already know.

## Prior art, and what it constrains

**UETA's electronic-agent provisions (1999)** are the headline and the posture-changer: statutory recognition that software can form binding contracts for a principal predates this entire field. The constraint it imposes cuts favorably: proposals here are not asking law to accept automated commitment — that is settled — but to recognize *verification structure* (bind, scope, chain) the 1999 answer never needed because its world was bilateral and pre-negotiated.

**Agency doctrine** — actual and apparent authority, holding out, ratification, and **respondeat superior** (the employer-liability rule that gives document 02's agents-as-workforce frame its liability face) — is the deepest well and the discipline on every claim in this document: it supplies the target shape (in-scope acts are the principal's), the hard cases (excess, reliance, after-the-fact acceptance), and the warning that evidence resolves facts while doctrine allocates risk.

**The digital-signature regimes** are the recognition pathway and its honest price. The sequence — years of cryptographic capability in legal limbo, then statutes conferring weight — proves recognition is achievable and legislative. The eIDAS qualified tier proves something more: the strongest recognition arrived *bundled with apparatus* — accredited, supervised, liable trust service providers. Recognition, historically, comes with a regime attached; whoever proposes the bind should expect to propose, or accept, its supervisors.

**UCC Article 12 (2022)** shows commercial law actively building infrastructure for controllable electronic records — evidence that the legislative machinery for exactly this class of problem is currently running, and a vehicle candidate.

**LEI and the legal-entity identifier systems** are the deployed registry of accountable legal persons — built post-crisis by regulatory mandate, adopted at the pace mandates are enforced: both the anchoring target and an adoption lesson.

**The DAO cases** are the negative exhibit: accountability vacuums get filled by improvisation under incident pressure, with doctrines stretched to reach whoever is reachable. **Corporate personhood** is the deepest precedent of all — law *manufacturing* a new category of accountable person when commerce required one — and it names the road this document deliberately does not take: "electronic personhood" for agents has been floated and rejected, and this problem statement assumes accountability maps to *existing* natural and legal persons, disclaiming the new-category question as out of scope rather than settled.

The constraint the history imposes: automated binding is recognized (UETA); person registries exist (LEI); recognition pathways exist and cost legislation plus apparatus (eIDAS); the doctrine that will adjudicate the hard cases exists and is not fully on the substrate's side (agency law); and the alternative to designing the bind is watching courts improvise one (DAOs). The unmet piece is the bind itself — verifiable, scope-carrying, transition-surviving, counterparty-checkable — and the recognition it must then earn.

## The second governance problem: governing the substrate itself

Accountability binding is this document's subject. But the suite has accumulated a second, distinct governance problem, and the closing document is where it must at least be *collected*: every layer's hard residue turned out to be a regime question, and no document could answer it from inside its layer.

The backlog, gathered: **the operator question** in its five appearances — who runs the coordination substrate (01), the discovery index (06), session custody (08), the integration hub the tool economy currently rents (10), and the audit record stores (12); **the stewardship questions** — who plays ICAO for the coordination vocabulary and its extensions (09), who disciplines extension fragmentation before it becomes XEP/FHIR-profile history repeating (01, 09), who accredits and disciplines attestors (05); **the enforcement questions** — who mandates audit coverage, penalizes suppression and tampering, and licenses custodians, without which the evidence layer's every analogy fails (12); and **the collision questions** — how agent commerce is squared with strong-customer-authentication law, chargeback classification, and money-transmission rules (11).

This document does not solve these, and says so. They are named here as a distinct problem statement the suite exposes: the substrate's primitives require stewardship, accreditation, mandate, and enforcement functions that no protocol layer can supply for itself — the same conclusion audit reached for its records, generalized to the whole stack. Whether those functions emerge from standards bodies, regulatory mandate, industry consortia, or the incident-driven improvisation the DAO cases preview is the open question the suite ends on — deliberately visible, rather than buried in twelve dependency sections.

## Additional governance needs

Stated as accountability or evidentiary failures; ownership per the suite.

**Binding and verification**
- Establish, before committing, which natural or legal person stands behind a counterparty agent — as a lookup, not an investigation.
- Verify the granted scope behind a proffered commitment at commitment time. *(04's mandate, examined by strangers.)*
- Resolve an excess-of-authority dispute from a shared factual record, with the doctrinal allocation left to doctrine.

**Transitions**
- Transfer accountability across corporate sale, restructuring, or reassignment such that counterparties and courts can verify the current and historical answerable person.
- Revoke a bind with defined effect on ongoing sessions *(08)* and standing commitments, and defined survival of historical accountability.
- Detect that an agent's accountable person has ceased to exist. *(02's orphans, before the liability materializes.)*

**Jurisdiction and recognition**
- Produce authorization evidence whose properties existing recognition regimes can attach to, portable across the jurisdictions a workflow spans — with enforcement's treaty dependence acknowledged.
- Operate under jurisdiction-specific overlays for regulated sectors *(deferred as secondary, per scope discipline)*.

**Composition**
- Determine, in a multi-principal workflow, which accountable person answers for which segment of a joint outcome.
- Anchor binds to deployed legal-entity identifier infrastructure rather than a parallel registry of persons.

## What this document does not claim

- **Not** that recognition follows from design. Legislatures and courts confer it; history says they confer it slowly and with supervisory apparatus attached; this document describes what makes conferral possible and attractive, and no more.
- **Not** a position on how excess-of-authority risk should be allocated. Apparent authority and ratification are doctrine's territory; the claim is confined to making the factual layer inspectable and the reliance baseline *shiftable*.
- **Not** electronic personhood. Accountability here maps to existing natural and legal persons; the new-category road is named and declined, not foreclosed for others.
- **Not** a replacement for contract law, agency doctrine, corporate law, signature statutes, or AI regulation — composition with all, and a candidate evidentiary substrate for the last.
- **Not** an escape from enforcement's realities. Cross-border enforcement still runs on treaties, recognition of judgments, and arbitration; the bind makes evidence portable, not judgments.
- **Not** a solution to the substrate's own governance. The backlog above is collected and handed forward as the suite's open ending, on the stated conviction that a designed answer beats an improvised one.

The claim is: agent action at machine speed, under delegation, across organizations and jurisdictions, currently terminates in accountability archaeology — the answerable person excavated from records the excavated parties control, authority disputes fought as asymmetric-evidence credibility contests, transitions and compositions leaving responsibility undefined, and courts improvising when the anchor is absent; the properties that would change this — a counterparty-checkable bind to a legal person, examinable scope, chain-following responsibility, transition and segmentation semantics, evidence shaped for recognition regimes — exist nowhere in combination; and the law's own history (electronic agents recognized in 1999, signatures given weight by statute, personhood manufactured when commerce demanded it) shows both that the recognition pathway is real and that it is walked by legislation, which is precisely why the evidentiary substrate should be designed before the incidents that will otherwise design it.

## Forward and backward dependencies

- **Identity (02), backward:** the accountable/trustworthy distinction completes here — accountability requires the bind this document describes; orphan detection becomes liability-vacuum detection; retirement-with-history serves historical accountability.
- **Authorization (03) and Delegation (04), backward:** chains and mandates are the scope evidence courts inspect; agency doctrine received from 04 now does adversarial work; the purpose-decidability boundary holds — courts adjudicate purpose disputes, which is exactly 04's ex-post conclusion.
- **Trust (05), backward:** evidence framing governs (an agent's record informs counterparties; it does not substitute for an answerable person); attestor accreditation joins the backlog.
- **Session (08), Tools (10), Commerce (11), backward:** succession, participation, and the regulatory collisions land in transitions, segmentation, and the backlog respectively; 11's dual-held evidence is the commercial instance of the shared factual record.
- **Audit (12), backward:** the regime handoff is received as this document's spine — attribution's evidence acquires force only through the bind and the recognition described here; coverage mandates and custodian licensing join the backlog.
- **Naming (14), forward:** the remaining document; the terminological surface of the suite, outside this document's scope.
- **The suite, forward:** the substrate-governance backlog is the deliberate open ending — the one problem statement these thirteen documents jointly expose and none of them, including this one, resolves.

---

[← Audit](12-audit.md) &nbsp;·&nbsp; [README](README.md) &nbsp;·&nbsp; [Naming →](14-naming.md)
