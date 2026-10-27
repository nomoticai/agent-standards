# 15. Transactions (TXN)

## Scope, status, and terminology

This document is a problem statement, not a solution proposal. Its subject is **the accountable unit of agent interaction**: the engagement two or more agents undertake together (the transaction), the individual acts that compose it (actions), and the identifiers and receipt that name them so that every participant, and any stranger who later inspects the interaction, references the same unit. It argues that no deployed identifier scheme names cross-organization units of agent work in a way all parties and outside verifiers can resolve, and it stops short of prescribing formats, minting mechanisms, or cryptographic constructions — stable shared reference, not UUID grammar; a receipt, not a signature suite; the properties, with the constructions left to the solution space.

Terminology, for this document: a **transaction** is a bounded engagement between agents that does something — a purchase, a booking, a completed exchange of work. An **action** is an individual act within a transaction — a quote requested, a reservation placed, a payment submitted. A **receipt** is the attributable statement that a named action occurred within a named transaction, held by the participants in equivalent form.

One boundary governs the whole document: **the standard provides the receipt; it never renders the verdict.** Whether an action should have happened belongs to authorization (03); whether the authority behind it was properly conferred belongs to delegation (04); whether the record of it can be trusted belongs to audit (12); what consequences follow belongs to governance (13). This layer names the unit and issues the receipt. The receipt outlives the decision.

**Relationship to communication and session, stated plainly because the confusion is common.** Communication (07) is the conversation: how messages move, in what modalities, with what reliability. Session (08) is continuity: how coordination state persists across time and reconnection. Transaction is neither. It is the unit of account for what the conversation did. Agents may communicate at length and transact nothing — browsing, negotiation that fails, discovery chatter. One transaction may span several sessions: negotiated Monday, fulfilled Thursday, across disconnects and process restarts. A transaction may even span channels. The analogy that holds: the phone call is communication; the purchase order number is the transaction. Nobody confuses the call with the PO, and no one should confuse carriage or continuity with the unit of account.

## The skeptical case

Correlation identifiers are everywhere. W3C trace-context standardized a propagated trace and span pair, and every observability vendor builds on it. Every framework mints request IDs. Every merchant issues order numbers, every airline confirmation codes, every database transaction IDs. Email solved decentralized per-message identity decades ago with Message-ID, and threads assemble across organizations from References chains. And the IETF is chartering an interaction reference for agent workflows right now. The skeptic's composition: trace-context for propagation, application request IDs for correlation, business confirmation numbers for commerce, and the incoming interaction reference for whatever remains. Naming units of work looks solved several times over.

## Where the mismatch actually lies

Every deployed scheme is a single-namespace scheme. Trace-context propagates identifiers faithfully and leaves their semantics per-deployment: sampling, retention, and meaning are the operator's, and attribution lands on services rather than agents. An order number is meaningful exactly against the issuing merchant's database; the customer holds a string that resolves nowhere else. Database transactions are single-system by definition. Message-ID comes closest to the decentralized ideal and carries no attribution, no integrity, and no receipt semantics: anyone can forge one, and a thread proves nothing.

What agent interaction specifically stresses: engagements form between strangers across organizational boundaries, at machine speed, composing many actions under delegated authority, and the questions that matter arrive later, from parties who were not present — an auditor, a counterparty's counsel, a payment network, a regulator. Under those conditions, the deployed schemes deliver a different name for the unit in every participant's records, joined afterward by timestamp correlation — which is where document 12's archaeology begins. The join keys were never shared; that is this document's problem, stated in one sentence.

A second mismatch is subtler and runs through the current proposal landscape: **the unit keeps getting bundled with the verdict.** Proposals that touch this layer tend to arrive as governance frameworks — the identifier, plus evaluation, plus policy, plus post-hoc review. The bundling is why thirty audit proposals and a governance industry each redefine the unit privately: no neutral layer owns it, so every opinionated layer invents it. A unit-naming layer only works as a standard if it is verdict-neutral, which is precisely what makes it thin enough to standardize.

## What observably breaks today

**Same engagement, N names.** The assistant's framework logs request IDs, the merchant logs an order number, the processor logs its own reference; the engagement they shared has no name they share. Every cross-organization question begins by reconstructing which records are even about the same thing.

**Actions and transactions blur.** Frameworks log requests; nothing marks which requests compose one engagement. The boundary of the unit — where the transaction began, which acts belong to it, whether it completed — is reconstructed by inference, differently by each party.

**Receipts are unilateral.** A confirmation number is the issuer's database key, graciously shared. The counterparty holds evidence that resolves only through the issuer's continued existence and cooperation — the dependence document 12 identifies, beginning one layer earlier than audit can fix it.

**Disputes lack a referent.** "Which transaction are we discussing" precedes every cross-organization dispute, and answering it consumes the discovery budget before the substantive question is reached.

**Failed engagements vanish.** A transaction that aborts is still a unit someone later asks about — the payment that half-completed, the booking that errored after the charge. Unnamed, it exists only as unjoined fragments in separate logs, which is exactly when naming matters most.

## Properties without a common definition

Framed as what participants and later strangers must be able to establish, mechanisms deliberately unspecified:

- **Unit naming:** a transaction identifier minted when the engagement forms, carried by every participant, resolving to the same unit in each participant's records and to a stranger reading any of them.
- **Action naming within the unit:** each act named and bound to its transaction, with relational structure — actions reference their transaction, sub-units reference their parent — rather than one identifier spanning everything.
- **The receipt:** an attributable statement, held in equivalent form by the participants, that a named action occurred within a named transaction: who (02's identifier), what act, which unit, when, attested. Dual-held, in the double-entry sense document 11 already reaches for.
- **Verdict neutrality:** identifiers and receipts carry no judgment. The receipt that an action occurred is not approval that it should have; nothing in this layer evaluates, permits, or reviews.
- **No bearer power:** possession of a transaction or action identifier confers no authority. Correlation and authorization stay separate, or the identifier becomes an attack surface.
- **Propagation:** identifiers ride the exchanges that compose the unit, whatever carries them (01, 07), surviving session boundaries (08) and multi-hop delegation (04).
- **Named regardless of outcome:** completed, aborted, and failed transactions are equally referenceable units.
- **Unlinkability across units:** unrelated transactions yield unrelated identifiers; the parties to one engagement gain no handle on another. Severing is a property, not an accident.

## Prior art, and what it constrains

**W3C trace-context** is the deployed proof that propagated correlation pairs work at scale, and its limits define this document's gap: single trust domain, service-level attribution, no integrity, no receipt.

**Message-ID and References (RFC 5322)** prove decentralized minting and cross-organization assembly — email threads are multi-party units built from identifiers no authority issued — and prove equally that identifiers without attribution or receipts settle nothing.

**The payment network reference chain** (authorization codes, acquirer reference numbers) is the closest deployed cross-organization precedent: issuer, acquirer, network, and merchant all resolve the same transaction from their own records, and disputes proceed against a common referent. Its lesson cuts both ways: it works, and it works because a central network operator mints the namespace — the centralized mint is the constraint an open agent ecosystem cannot import.

**Double-entry bookkeeping** is the ancestral dual-held receipt; document 11 already claims it for commerce evidence, and this layer is where the shared reference that double entries join on gets named.

**The chartered interaction reference** in current IETF agent work is the strongest possible validation that this layer is real: an entire working group scope reduced to a propagated identifier plus proof of binding. This document states the problem that work answers, and states it wider: binding and propagation are necessary; the unit semantics, the action structure, the receipt, and verdict neutrality are the rest of the problem.

## The core use case: the disputed purchase

An assistant agent and a merchant agent negotiate an equipment purchase: quote requested, terms negotiated, order placed, payment submitted, delivery scheduled — five actions, one transaction, spanning two sessions across three days. Two weeks later the buyer's finance team disputes the charge: the amount differs from the quoted terms. The dispute involves the buyer, the merchant, the payment processor, and eventually an auditor — four parties, two of them strangers to the original exchange.

**What breaks today.** The assistant's framework holds request IDs; the merchant holds an order number; the processor holds its reference; the quote and the payment live in different systems on both sides. Establishing that the disputed charge belongs to this engagement, under these negotiated terms, is timestamp-and-amount correlation across four namespaces — and the negotiation session and the fulfillment session share no key, so even one party's own records need archaeology before the cross-party work starts.

**What any adequate answer must make possible.** The transaction was named when the engagement formed, and every action — the quote, the terms, the order, the payment, the scheduling — carries that name and its own, in both parties' records. The receipt for each action is dual-held: the buyer's copy and the merchant's copy attest the same act in the same unit. The processor's reference maps to the named transaction because the payment action carried it. The auditor, a stranger, resolves the entire engagement from either party's records alone, walks quote to payment inside one named unit, and the question "does the charge match the terms" becomes a comparison of two receipted actions rather than a reconstruction.

**What no answer can promise, and this document does not claim.** The receipt proves the exchange, never its wisdom: a named, receipted, perfectly referenced transaction can still be a bad deal, an unauthorized act, or a fraud — those determinations belong to authorization, delegation, trust, and governance, consuming the receipt as input. Fulfillment stays off-substrate: whether delivered goods matched remains document 11's residue. And naming forces nothing: an interaction conducted off-substrate is unnamed, and completeness of naming, like completeness of records, is a regime outcome (12, 13) rather than a property this layer can supply.

**Open questions this use case exposes.** Who mints the transaction identifier when the engagement forms between strangers — initiator, responder, both, or derivation; how multi-party units (three or more agents) name themselves; and how long-running units (weeks, standing relationships) bound themselves — solution-space, all; the failures stand regardless.

## Additional transaction needs

Stated as reference or accountability failures; ownership tags per the suite.

**Naming and structure**

- Group many actions under one engagement across sessions, stacks, and organizations. *(01, 08, 10.)*
- Name sub-units within larger engagements, with child referencing parent. *(04's chains, structurally parallel.)*
- Name aborted and failed engagements as referenceable units.

**Evidence and joins**

- Supply the shared join keys audit's cross-system correlation currently lacks. *(12's inbound requirement, discharged here.)*
- Provide the dual-held receipt commerce claims as transaction evidence. *(11.)*
- Anchor delegation context to the unit it authorized. *(04.)*

**Boundaries and neutrality**

- Keep identifiers verdict-free and bearer-powerless, so the unit layer never becomes a policy engine or an attack surface. *(03, 13.)*
- Keep unrelated units unlinkable, so transaction history is not an ambient surveillance graph. *(12's dual-use warning, one layer earlier.)*

## What this document does not claim

- **Not** any construction. Identifier formats, minting protocols, and signature schemes are solution space; the claims are the properties.
- **Not** evaluation. The receipt is not approval; this layer renders no verdicts and reviews nothing.
- **Not** audit. Document 12 owns evidence integrity, custody, disclosure, and coverage; this document owns the unit and its names, which those records reference.
- **Not** communication or session. Carriage is 07; continuity is 08; the unit of account is here.
- **Not** completeness. Off-substrate work stays unnamed; naming everything is a regime outcome, not a data structure.

The claim is: agent engagements form across organizational boundaries at machine speed, compose many actions under delegated authority, and generate later questions from parties who were not present; today every participant names the shared unit privately, receipts are unilateral database keys, failed units vanish, and every cross-organization question starts with archaeology to establish which records describe the same thing; the required properties — shared unit naming, action structure, dual-held verdict-neutral receipts, propagation, unlinkability — exist piecewise in trace-context, Message-ID, payment references, and double-entry practice, and nowhere in combination; and this layer is where the suite's identifiers stop being each document's assumption and become someone's deliverable.

## Forward and backward dependencies

- **Backward:** 01 and 07 carry the identifiers; 02 supplies the who of every receipt; 04's chains attach to the units they authorize; 08's sessions are spans the unit crosses.
- **Forward:** 11 receives the dual-held receipt as its transaction evidence; 12 receives the join keys its investigations require and makes the receipts trustworthy over time; 13 receives the referent its decisions, disputes, and enforcement cite. The receipt is what the regime acts on; without the name, there is nothing to adjudicate.

---

[← Naming](14-naming.md) · [README](README.md) · 
