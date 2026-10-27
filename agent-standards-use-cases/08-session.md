# 8. Session (SSN)

## Scope, status, and terminology

This document is a problem statement, not a solution proposal. Its subject is the **logical coordination session**: the durable, multi-participant context within which a long-running agent workflow coheres — as distinct from any connection that carries it and any single message within it. It argues that this object has no common definition anywhere, that its absence is observable in how long-running agent work fails today, and it stops short of prescribing a state model, a storage architecture, or a continuity mechanism.

**Boundary declarations.** *Transport (01)* owns connections and streams: their multiplexing, priority, teardown, and behavior under failure and migration. This document owns what must *outlive* any of them. Where earlier drafts spoke of "surviving transport failure and process restarts," the carriage half of that sentence belongs to transport; what belongs here is that the logical session is not identical to any connection and must remain coherent when every connection it ever used is gone. *Communication (07)* owns individual messages and hands this document its declared seam: messages carry a context *reference*; the context *object* — its lifecycle, membership, state, and continuity — is defined here. Transport also hands this document its oldest debt: document 01's finding that *no identifier at any layer names the workflow*, with the session model to be defined here.

This document receives, from identity (02), the dependence of participant continuity on identifier continuity; from authorization and delegation (03/04), the fact that delegations bound into a session have their own liveness, distinct from the session's; and from delegation specifically, mandate windows that span the pauses described here.

## The skeptical case

The weak skeptic assembles web-session machinery: cookies for state, TLS resumption for reconnection, WebSockets for real-time, OAuth refresh tokens for authorization persistence, server-side session tables for everything else. Mature, universal, sufficient.

The strong skeptic points at a shipping product category. **Durable execution** — Temporal, AWS Step Functions, Airflow, Camunda and the BPMN world — exists precisely for workflows that run for weeks, survive process crashes and redeployments, pause for human approval, and resume with complete state. "A seven-day research task with human checkpoints" is not a gap in the ecosystem; it is the demo on the Temporal homepage. Event sourcing supplies durable, replayable, append-only workflow state as a routine architectural pattern. The skeptic's conclusion: long-running stateful coordination is a solved problem with multiple mature implementations; use one.

Both skeptics deserve their due, and the weak one is dispatched by his own tools' assumptions: cookies, resumption tickets, and refresh tokens were designed for one human, one service, minutes to hours, state at one or two endpoints, transport roughly continuous. The strong skeptic requires a real answer.

## Where the mismatch actually lies

Durable execution solves the *single-operator* version of this problem, and solves it well. A Temporal workflow's state lives in one organization's cluster; its participants are activities and workers that operator deploys; its history is readable by that operator and no one else needs proofs, because everyone who matters trusts the cluster. Every property this document describes exists there — inside one trust domain, under one administrator, with continuity guaranteed by the operator's database rather than demonstrated to outsiders.

The agent case that runs through this suite removes every one of those conditions. The session's participants belong to different organizations; no single cluster is acceptable as the home of shared state; counterparties need continuity *demonstrated*, not operator-vouched; and the participants themselves are agents whose identities, delegations, and lifetimes are governed by documents 02–04 rather than by a deployment manifest. Six specific assumptions break, and they are the structural spine of this document:

**Duration.** Coordination spans days to months, with intentional pauses for review, co-signers, court schedules, external data. The wait is a designed state of the workflow, not a timeout to be survived.

**Locus of state.** The coherent context spans a delegation chain — assistant, research agent, data agent — across organizations. Traditional session state lives at one endpoint; durable-execution state lives at one operator. Neither names a context that three organizations share.

**Independence from carriage.** Every connection the session ever used may be gone — processes restarted, containers migrated, nothing transported for days — while the logical session persists. (How any individual connection behaves under failure is transport's territory; that the session is not any of its connections is this document's.)

**Demonstrable continuity.** For high-consequence coordination, a receiver evaluating today's message needs evidence that it belongs to the same logical session established three weeks ago — evidence verifiable without extending full trust to whichever store held the state in between. Operator-vouched continuity is exactly what cross-organization counterparties cannot accept.

**Defined partial-failure semantics.** A workflow that commits effects across multiple participants and then loses one leg needs a defined story for what unwinds, what compensates, and what stands. (What that story can and cannot promise is treated honestly below; earlier drafts' "commit-or-rollback" overpromised.)

**Suspend and resume as first-class states.** Pausing for human input or an awaited event, and resuming with membership, delegation context, and state intact — without re-authentication and re-establishment from scratch — is the workflow's normal rhythm, not an edge case.

Three analogies from earlier drafts survive, each now carrying its honest limit. The **video-game save-state** captures intentional pause and exact resumption — and is a single-player property: an MMO cannot save one player's world, and the multi-*party* save is precisely the hard part. The **bank account** captures state persisting across time, devices, and staff — and is institution-held state: the operator question answered by fiat, which is the answer cross-organization sessions cannot use. The **medical record** captures long-lived, multi-party, appendable context — and is *aspirational*: cross-provider record portability is the famous, decades-running failure of HL7 and FHIR, which testifies to this problem's difficulty, not its solvedness.

## What observably breaks today

**Long-running agent work is serialized improvisation.** Each application persists coordination state to its own store in its own schema at each pause, and reloads at each resumption. Every serialization boundary is an integrity risk; every schema is private; nothing about the persisted context is portable to, or verifiable by, any other participant.

**Resumption is re-establishment.** Reattaching after a pause means re-authenticating each participant, re-verifying identities, and re-deriving delegation context — friction at every boundary, and a fresh opportunity at each one for the coordination to fail to resume or to resume differently than it paused.

**Participants disagree about the session.** Document 01's post-restart divergence, at the logical layer: after one participant restarts, its counterparts hold state and stream identities for a session the restarted party can no longer name. There is no shared object to reconcile against, because the "session" was always three private approximations of one.

**Continuity is asserted, not evidenced.** Nothing available lets a receiver verify that today's message belongs to the workflow established weeks ago. The claim rides on bearer artifacts and the counterparty's records — the counterparty being exactly the party whose continuity is in question.

**Dormancy hides staleness.** A session resumed after a long pause carries the authority context it paused with. Delegations revoked in the interim, principals departed, budgets exhausted — none of it is surfaced by resumption, because resumption checks continuity at most, and continuity is not currency.

**Partial failure is handled ad hoc, per participant.** When one leg of a multi-participant commitment fails, each participant consults its own logs and improvises compensation. There is no shared record of what reached what state, so the unwinding is manual reconciliation — document 03's finance archaeology, generalized.

**Cross-organization sessions do not exist as objects.** Two organizations in one long-running workflow have, between them, no thing that *is* the workflow: no shared identifier (transport's finding), no shared membership list, no shared state, no agreed view of what each side may see.

## Properties without a common definition

Framed as what participants and receivers must be able to establish about a logical session:

- **Identity:** a stable identifier naming the workflow itself — the gap document 01 established — distinct from every connection, process, and participant identifier, valid across all of them.
- **Membership:** who the participants are, under which identities (02) and mandates (04); how participants join, leave, are succeeded, or are retired mid-session without the session losing coherence. *(Succession collides with identity's continuity rules; jointly owned.)*
- **Reattachment:** how a participant resumes after dormancy or restart with context intact — and what, exactly, is preserved: state, membership view, delegation bindings.
- **Continuity evidence:** what a receiver can verify about a resumed session's relation to the original — decomposed honestly, because the candidates come apart: custody of session material, hash-linked state history, and participant attestation are different continuities, and a resumer can hold one without the others.
- **Currency at resumption:** which bindings are re-checked when a session wakes — the dormancy face of authorization's effect-time problem: delegations, budgets, and identities may all have changed while nothing moved.
- **State coherence and visibility:** what "the current session state" means when participants span organizations — with the consistency costs stated, not assumed away (document 01 already cited Matrix state resolution as the preview) — and per-participant visibility rules over that state.
- **Partial-failure semantics:** the defined record of what committed, what failed, and what compensates — compensation as a session-level vocabulary rather than per-participant improvisation.
- **Suspension:** dormancy as a defined state with defined properties — including its security properties, since a dormant session is stored attack surface.
- **Custody:** where session state lives during dormancy and between participants — replicated, held, or carried — with the operator question (documents 01 and 06) answered explicitly rather than inherited silently.

## Prior art, and what it constrains

**Durable execution (Temporal, Step Functions, BPMN engines)** is the headline and the standard the delta must be stated against: long-duration, restart-surviving, pausable workflows with complete state — inside one trust domain, under one operator, with continuity by operator fiat. Any proposal here that does not clearly state what it adds beyond "Temporal, but multi-organization" should be presumed to be reinventing it; and "multi-organization Temporal" is precisely where every hard property above begins.

**Distributed transactions** constrain the partial-failure section with three decades of hard-won retreat: two-phase commit blocks, and blocking across trust boundaries is unacceptable, which is why the industry moved to **sagas and compensating actions** (Garcia-Molina and Salem, 1987) for exactly the multi-step, long-lived workflows at issue. **WS-Coordination, WS-AtomicTransaction, and WS-BusinessActivity** were the standards-era attempt at cross-organization transactional context — document 07's WS-\* graveyard, transactional wing — and are the obligatory confrontation for any "commit-or-rollback" language: this suite's earlier drafts contained exactly that language, and it is withdrawn below.

**Matrix rooms** are the closest deployed artifact to the multi-party session object: long-lived, cross-organization, replicated state with member join/leave — already in document 01's prior art, recurring here with its lesson intact: federated state resolution is the price of shared state without a single operator, and it is expensive.

**SIP dialogs** contribute the vocabulary distinction between the long-lived session and the transactions within it, and demonstrate session-layer identity over changing transports — with document 01's B2BUA caveat about what intermediaries do to end-to-end session semantics.

**Event sourcing and hash-linked logs** (with Certificate Transparency as the deployed large-scale instance) are the constructive prior art for continuity evidence: an append-only, hash-chained session history is verifiable without trusting its custodian. **Token binding (RFC 8471)** and channel binding are the cautionary counterpart — deployed cryptographic session-continuity mechanisms, removed from the major browser for lack of adoption: continuity machinery has failed in the market before, and the bootstrap warning from documents 02 and 03 applies.

**HL7 and FHIR** are the reality check on the medical-record analogy: the most heavily funded, longest-running attempt at portable multi-party context in any industry, still incomplete after decades — a difficulty calibration, and a warning about schema-first approaches to shared state.

The constraint the history imposes: every property exists somewhere — durability in Temporal, multi-party replication in Matrix, continuity evidence in CT-style logs, compensation in saga practice. Nothing provides their combination across organizational boundaries, and the graveyards (WS-\* transactions, token binding, cross-provider health records) mark where the combinations have previously been attempted and lost.

## The core use case: a research task spanning a week with human review

An assistant agent prepares a market research report for its principal. Day 1: work begins; data-gathering is delegated to a research agent operated by a subscription intelligence service; preliminary observations go to the principal. Day 2: the principal redirects scope (enterprise segment, competitor pricing); the assistant re-delegates with new criteria. Day 3: data collection completes; analysis begins; two questions go to the principal; the coordination pauses. Day 4: the principal travels; the session waits all day; answers arrive at night. Day 5: resumption; draft produced. Day 6: markup returned; revisions. Day 7: delivery.

One logical workflow; six overnight pauses with every connection dropped and processes restarted; one full-day intentional dormancy; a sub-delegation to another organization; possibly an infrastructure migration mid-week. The week is the session.

**What breaks today.** The assistant's application serializes its private notion of the context nightly and reloads it each morning — every boundary an integrity risk, every schema its own. Each morning is re-establishment: re-authentication, re-verification of the research agent, re-derivation of the delegation — and the research agent's participation is tracked in a *separate* store under a *separate* notion of the engagement, so the two organizations hold two diverging approximations of one workflow, with nothing to reconcile against if they drift (and after any restart, they drift). When the day-5 draft arrives at the principal's systems, nothing lets them verify it belongs to the workflow authorized on day 1 rather than to anything else wearing the same tokens. And if the principal had revoked the pricing-analysis sub-delegation during the day-4 dormancy, day-5's resumption would proceed on the paused authority picture — continuity intact, currency silently gone.

**What any adequate answer must make possible.** The workflow has an identity all participants and the principal can reference across the week. The research agent's organization participates in *the same* session object, under defined visibility into its state. Reattachment after each pause preserves membership and delegation context rather than reconstructing them. A receiver of the day-5 draft can verify its continuity with day-1's establishment on evidence, not on trust in either party's store — and resumption re-checks the currency of the bindings the pause preserved.

**What no answer can promise, and this document does not claim.** Atomicity across organizations: the retreat from cross-domain 2PC stands, and if the workflow had committed partial purchases across participants, the achievable property is saga-shaped — a shared, coherent record of what committed and defined compensation semantics — not rollback. Continuity as a proxy for anything else: continuity evidence establishes the session's chain of custody, not that a dormant month didn't include a key compromise, and not that the authorities bound at pause survive to resumption — currency and compromise are separate checks that continuity must not be allowed to impersonate. Free coherence: shared state across organizations pays consensus and resolution costs (Matrix is the preview), and per-participant visibility raises them.

**Open questions this use case exposes.** Custody of state through the day-4 dormancy (replicated, held, carried); the granularity of continuity evidence (per message, per resumption, per epoch); and which bindings resumption re-verifies by default — all solution-space; the failures stand regardless.

## Additional session needs

Stated as observable needs or failures; items owned elsewhere are tagged.

**Lifecycle**
- Sustain one logical coordination context across days-to-months of activity and intentional dormancy, referenceable by all participants throughout.
- Suspend for human input or an awaited event and resume with membership, state, and delegation context intact — with resumption's re-verification obligations defined. *(Currency checks joint with 03/04.)*
- Recover a coherent, mutually agreed session after any participant's restart, ending the private-approximation divergence. *(The logical-layer counterpart of transport's recovery requirement.)*

**Membership**
- Admit a participant from another organization into the same session object, under defined visibility.
- Replace or retire a participant mid-session with defined succession — what the successor inherits, what the departed's contributions remain attributed to. *(Joint with identity's continuity and retirement rules.)*
- Let an ephemeral specialist join for one decision and leave without destabilizing remaining participants. *(Carriage-side counterpart in transport.)*

**Continuity and evidence**
- Demonstrate to a receiver that a message or reattachment belongs to the session established earlier, without full trust in any single state custodian.
- Detect, at resumption after dormancy, that authorizing delegations were revoked, principals departed, or budgets exhausted in the interim.
- Bound what a compromise during dormancy can silently inherit — dormant sessions as stored attack surface, acknowledged and constrained.

**Partial failure**
- Maintain a shared record of which effects committed at which participants when a multi-leg workflow partially fails, so compensation works from one account of reality rather than several. *(Compensation for irreversible effects: joint with transport's cancellation honesty and authorization's effect-time problem.)*
- Express compensation as session-level semantics rather than per-participant improvisation.

**State**
- Define what "current session state" means across organizations, with its consistency model and costs explicit.
- Control which participants see which parts of session state, without forfeiting continuity verifiability for any of them.
- Correlate related sessions into one logical workflow across time. *(Received from transport's use-case list; joint with audit.)*

## What this document does not claim

- **Not** cross-organization atomicity. "Commit-or-rollback" language from earlier drafts is withdrawn; the saga retreat and the WS-AtomicTransaction graveyard are accepted as constraints, and the claim is limited to shared partial-state records and defined compensation semantics.
- **Not** that continuity evidence establishes currency or non-compromise. It establishes custody of the session's thread, and no more; treating it as more recreates the valid-credential/live-mandate confusion of document 04 at the session layer.
- **Not** a state model, storage architecture, consistency protocol, or continuity mechanism — and not an escape from the operator question, which recurs here (custody during dormancy) and must be answered by any solution explicitly.
- **Not** that durable execution is inadequate for its domain. Single-operator workflow engines are the presumptive substrate *within* a trust domain; the claim is confined to the cross-organization, multi-party, evidence-requiring deltas stated against them.
- **Not** carriage. Connection behavior under failure belongs to transport; this document's object outlives connections by definition.

The claim is: the logical coordination session — a durable, multi-participant, cross-organization context with verifiable continuity, defined resumption, defined partial-failure semantics, and defined state visibility — exists today only as per-application improvisation inside single trust domains; nothing names the workflow, nothing lets counterparties verify its continuity, resumption silently launders staleness, partial failure is reconciled by archaeology, and every one of these failures is routine in exactly the long-running, delegated, cross-organization work the rest of this suite describes agents doing.

## Forward and backward dependencies

- **Transport (01), backward:** discharges the debt declared there — the session-identity gap is received and the session model's required contents are defined above; the logical/carriage boundary is enforced in both directions; post-restart divergence and ephemeral-participant needs are received at the logical layer.
- **Identity (02), backward:** participant membership rides on identifier continuity; succession and retirement are jointly owned; instance distinction determines who, exactly, is a member.
- **Authorization (03) and Delegation (04), backward:** bindings bound into a session retain their own liveness; resumption-time currency is the dormancy face of effect-time validity; mandate windows span session pauses by design.
- **Communication (07), backward:** the context-reference field defined there points at the object defined here; exchange-level multi-party state is received.
- **Semantics (09), forward:** suspension, resumption, compensation, and succession need expressible vocabulary; the session states defined by need here constrain what the semantic layer must be able to say.
- **Audit, forward:** the session record — membership, state transitions, partial-failure accounts — is primary audit substrate; workflow correlation is jointly owned.

---

[← Communication](07-communication.md) &nbsp;·&nbsp; [README](README.md) &nbsp;·&nbsp; [Semantics →](09-semantics.md)
