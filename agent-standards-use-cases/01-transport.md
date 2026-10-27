# 1. Transport (TPT)

## Scope, status, and terminology

This document is a problem statement, not a solution proposal. Its subject is the **transfer layer itself**: the protocol layer that carries agent coordination, whatever that layer turns out to be. Its claim is that this layer's fundamental design questions — stateless or stateful, content methods or coordination methods, identity above the protocol or native to it, connection or session as the primary unit — have never been posed *as questions* for the agent case. Every current agent stack inherits HTTP's answers to all of them by default, and then rebuilds the opposite answers at the application layer, incompatibly. The document stops short of answering the questions it poses; posing them, with the evidence that the inherited answers are strained, is the problem statement.

This document also holds a distinctive position in the suite: the transfer layer is the **placement decision** for everything that follows. Each later document defines a primitive — identity, attribution, session context, coordination acts — and for each, there is a prior question this document owns: does that primitive live *in* the transfer protocol, where it is common by construction, or *above* it, where every stack defines it privately? The status quo answers "above, privately, per stack" for all of them — by inheritance, not examination.

Boundaries, stated so the axes below cannot be misread as land grabs: what a logical session *contains* — lifecycle, membership, continuity evidence — is document 08's; whether the wire should know state exists at all is this document's axis. What coordination acts *mean* is document 09's; whether the protocol's method surface should carry intent is this document's axis. What the agent identifier *is* belongs to document 02, and its resolution to document 06; whether the identifier lives above the protocol or is the protocol's addressable element is this document's axis. What the message envelope must make visible is document 07's; what the protocol beneath it makes native rather than conventional is this document's axis.

A note on the word "transport." The layer in question is not a transport in the OSI or TCP/QUIC sense; where precision matters, this document says *transfer layer* for the layer whose design is in question and *carriers* for TCP, QUIC, and their kin beneath it — the same relationship HTTP has to TCP, and SIP to UDP and TLS.

## The skeptical case

The question of whether agents need a dedicated coordination substrate has been asked implicitly through many recent standards proposals but rarely addressed directly. Every proposal for agent identity, session, discovery, or coordination assumes some substrate carries the primitive it defines. Yet the substrate itself has yet to be examined as a design choice. This section examines it, starting with the strongest available case against.

The honest skeptical case is stronger than it is usually given credit for, because modern HTTP is not the HTTP of 1999. HTTP/2 provides stream multiplexing, per-stream cancellation (RST_STREAM), and stream prioritization within a connection. HTTP/3 over QUIC adds independent stream delivery without head-of-line blocking, per-stream teardown (STOP_SENDING), 0-RTT session resumption, and connection migration across network changes. WebTransport exposes bidirectional streams and datagrams to applications directly. WebSockets and SSE cover long-lived bidirectional and server-push patterns. On top of this, the current agent ecosystem is a working proof of concept: MCP, A2A, and ACP all run on HTTP-family carriers, all have working implementations, and all have growing production use. JWT and OAuth carry authorization; MIME types have handled multi-modal content for decades; every runtime language has mature client libraries.

The skeptic's conclusion follows naturally: rather than invent a new layer that becomes an N+1th stack and fragments the ecosystem further, standardize conventions on top of what exists — define headers for attribution and delegation, adopt JSON schemas for coordination messages, publish an "agent coordination conventions" specification. The path of least fragmentation is to build on what already works.

This case must be answered on its merits, not by attacking an outdated version of HTTP. Much of what earlier critiques attributed to "HTTP" — no multiplexing, no in-session cancellation, no priority — was solved at the framing layer years ago. The unmet requirements are not at the wire.

## Where the mismatch actually lies

HTTP is not merely a protocol. It is a **bundle of architectural answers**, each correct for its design center — moving documents from servers to human-facing clients at web scale:

- **Stateless**, because statelessness bought cacheability, horizontal scale, and failure isolation for request/response document delivery.
- **Resource-oriented content methods** (GET, POST, PUT, DELETE), because the operations were operations *on content*.
- **No party in the protocol** — identity lives entirely above the wire (TLS, cookies, tokens), because the web's client was an anonymous-by-default user agent.
- **The connection as the primary unit**, because a document transfer begins and ends within one.
- **Content-typed payloads for rendering**, because a human was going to read the result.

Agents inherited the bundle whole — it was there, it worked, and every runtime had a client library. And the tell that the inherited answers are wrong-shaped is not any single missing feature; it is the **inversion pattern**: every agent stack, independently, rebuilds the *opposite* of each answer at the application layer.

- Stateless wire → session machinery reconstructed above it: context stores, resumption tokens, reconnection logic — per stack, incompatibly.
- Content methods → coordination intent reconstructed above them: tool schemas, task objects, prose to be inferred — per stack, incompatibly.
- Identity above the wire → attribution bolted on through headers, JWTs, and mTLS — attached to connections and requests, dissociating at every hop and relay — per stack, incompatibly.
- Connection as unit → the multi-participant, multi-carrier, multi-day logical session assembled above point-to-point connections — per stack, incompatibly. (Every rich carrier primitive — streams, prioritization, reset, migration, in HTTP/2–3, QUIC, WebTransport, and MOQ alike — is scoped to one connection between few endpoints; nothing at any layer cancels *a session's* work across participants on different carriers, orders a control message ahead of in-flight work flowing to a *different* participant, or gives the other participants a session-level fact to update when one member restarts or moves.)
- Content payloads → intent-plus-context and concurrent multi-modal *coordination signals* forced through content-delivery framing — per stack, incompatibly.

One inversion might indicate one stack's taste. Five inversions, performed by every stack, differently, are evidence about the layer underneath: the inherited answers do not fit the use, and the cost of the misfit is paid privately and repeatedly above the wire.

That reframes the problem statement as a set of **design questions that have never been posed as questions** — answered so far only by inheritance:

**Should an agent transfer protocol be stateless or stateful?** Statelessness was the right trade for cacheable document delivery. Agent coordination is conversational, long-lived, and pause-tolerant by nature; today that nature is expressed entirely in application-layer state bolted onto a wire that forgets everything between requests. Whether the transfer layer should *know that state exists* — sessions, subscriptions, presence as protocol facts rather than application reconstructions — is a genuine open question with real costs on both sides (stateful protocols pay in scale-out complexity and middlebox behavior; stateless ones pay the reconstruction tax now being paid five ways). What any session *contains* remains document 08's problem regardless of the answer.

**Should its methods be content operations or coordination operations?** GET and POST describe what happens to a resource; QUERY, PROPOSE, and CANCEL describe what a party is *doing*. Today the wire speaks the first vocabulary and every stack reconstructs the second above it. This axis is the one with controlled empirical evidence: intent-dense method naming measurably improves machine selection accuracy by 10–29 percentage points at frontier scale and survives documentation noise that collapses CRUD naming (see document 09, which owns the vocabulary and its meaning; this document's question is only whether the method surface of the transfer protocol should carry intent at all). SIP is the deployed precedent that it can: INVITE, CANCEL, and BYE are coordination methods at the wire.

**Should the acting party live above the protocol, or in it?** HTTP has no concept of a party; agent identity today is a header on a connection, minted per stack, dissociating at every relay. The alternative shape is protocol-native addressing: the agent identifier as the element the protocol routes and addresses by — the way an IP address or a phone number *is* the protocol element rather than an annotation on it. HIP demonstrated identity-native addressing at the network layer (and its adoption failure is a standing warning); every current stack demonstrates the above-the-wire answer's cost (documents 02 and 07 catalogue the dissociation). What the identifier must *be* is document 02's problem, and its resolution document 06's; whether it lives in the protocol is this one's.

**What is the protocol's primary unit — the connection, or the message and the session?** And is the protocol point-to-point, with multi-party coordination assembled above, or multi-party native? The group-communication tradition answered "multi-party native" inside single administrative domains three decades ago; every internet-scale protocol in current agent use answers "point-to-point" and leaves the assembly to applications.

**What is the payload model?** Content for rendering, or intent plus context between software peers, with voice, video, and structured data as concurrent coordination signals rather than deliverables — and with attribution at message granularity rather than connection granularity?

None of these questions has a settled answer, and this document does not settle them. The claim is narrower and harder to dismiss: **they have never been asked.** The current answers were inherited from a protocol designed for a different use, and the universal inversion pattern is what inheriting the wrong answers looks like from above.

The skeptic has a natural fallback at this point — *"then build a better session protocol on QUIC or WebTransport"* — and it deserves a direct answer, because it is half right. A purpose-built transfer layer over QUIC-family carriers is a perfectly plausible solution shape, and this document, being a problem statement, rules neither for nor against it. But observe what building it would require: taking positions on exactly the axes above — state model, method semantics, identity placement, primary unit, payload model — and agreeing common definitions for what each position implies. The fallback is not an objection to the problem statement; it is one candidate answer to it, and it cannot be designed until the questions this document poses are answered deliberately. The problem is prior to the layering choice.

The placement consequence completes the picture. Whatever the answers turn out to be, the transfer layer is where each of this suite's primitives either becomes **native and common** — defined once, present on every exchange — or remains **conventional and private** — rebuilt per stack above a wire that does not know it exists. The status quo places every primitive above, by inheritance; the rest of this suite documents, primitive by primitive, what that placement costs.

## What observably breaks today

The abstract mismatch has concrete, reproducible failure modes. Anyone assembling a multi-stack agent workflow today (MCP for tools, A2A for agent-to-agent, WebRTC for voice) encounters versions of the following:

**Cancellation races.** A cancellation issued by the principal must be translated into each stack's cancellation semantics separately. The cancellation path is stack-specific and frequently slower than the happy path it is chasing; the hotel API confirms the reservation before the cancellation arrives, and the workflow ends in a state no participant intended. There is no cross-stack ordering relationship between a control message and the operations it is meant to preempt.

**Post-restart divergence.** After a process restart of one participant, its peers may still believe streams are live that the restarted process can no longer identify. Each stack has its own (or no) session-resumption model, so a logical session that spans stacks resumes partially: the A2A leg reconnects, the tool leg re-authenticates from scratch, the voice leg drops. The participants now disagree about what session they are in.

**Audit fragmentation.** Reconstructing the causal chain of a single workflow requires joining three or four log formats across three or four systems, each with its own identity namespace and clock. The delegation lineage — who authorized which action on whose behalf — is not recoverable from the logs because it was never carried uniformly in the messages.

**Non-uniform revocation.** A principal revoking a specific sub-delegation mid-session cannot have the revocation enforced uniformly, because each stack has its own token, refresh, and caching model. The revocation takes effect at different times on different legs, and on some legs only at the next token refresh — after irreversible side effects may have occurred.

**No shared session identity at any layer.** A workflow spanning a WebRTC voice channel, an A2A exchange, and two HTTPS tool APIs has no identifier, at any layer of any of those protocols, that names the workflow. Correlation is reconstructed after the fact by convention, differently by everyone.

These are not hypothetical. They are the routine integration burden of every team building multi-stack agent systems today, and they are the specific evidence that the coordination primitives lack a common definition.

## Properties without a common definition

Restated as requirements the ecosystem demonstrably has — because every stack has built some version of each — rather than as features of a presupposed solution:

**Session continuity across pauses and restarts.** Coordination sessions span days, with pauses for human review and external events, and survive process restarts on either side. No carrier-layer resumption mechanism covers this; every stack rebuilds it with opaque tokens and bespoke reconnection logic.

**Peer-symmetric coordination.** Between agent peers there is no natural client-server asymmetry: either party may initiate, propose, or negotiate. Carrier protocols retain an initiator/responder frame even when bidirectional; each stack papers over it differently.

**Per-message attribution and delegation lineage.** The acting agent's delegation, authority scope, and identity context may vary between messages in one session; a tool invocation and a delegation transfer carry different attribution requirements. This requirement has real costs — signature computation, key management, replay protection — and any honest accounting must include them. The present problem is that each stack pays these costs in a different currency, with different envelope formats, and the lineage still dissociates at every cross-stack hop. (DIDComm demonstrates that per-message attribution can be defined carrier-independently; see prior art below.)

**Multi-modal streams as coordination signals.** A voice conversation with the principal is coordination context, not content delivery, and it runs concurrently with structured-data, chat, and tool streams that all belong to one logical session — across multiple carriers and participants, which is precisely the scope no carrier-layer multiplexing covers.

**Cross-participant control propagation with priority and partial teardown.** A cancellation must reach all affected participants ahead of the in-flight work it preempts, tear down some streams while others continue, and leave a determinate record of what was and was not stopped in time.

**Delegation-chain preservation across hops and relays.** Multi-hop workflows (principal → A → B → C → tool) require the full lineage to remain visible at every step for authorization and audit. Point-to-point tokens rebind at each hop; gateways and relays break end-to-end attribution unless the envelope is defined to survive them.

**Cross-organization session continuity.** Sessions that extend to a partner organization mid-flow must not depend on either organization's private session infrastructure. No existing stack's session state survives this transition cleanly.

**Delivery ordering and duplicate detection for control messages.** At-least-once delivery with deduplication, and defined ordering between control messages and the operations they govern, across participant restarts — currently provided by none of the stacks for cross-stack workflows, and differently by each within its own boundary.

## The fragmentation question, answered honestly

The integration cost of the status quo is quadratic in the obvious way: to add stack N, bridges must be built to each of the existing stacks, and every bridge is a place where cancellation, attribution, and session semantics are translated lossily. This is the same scaling problem HTTP itself solved for the web, one layer down.

But the quadratic argument, by itself, proves less than it appears to. *Any* common layer collapses N×M to N — including the skeptic's conventions specification over HTTP. A new substrate is, on day one, an N+1th stack. The problem statement therefore cannot rest on the scaling argument alone; it must locate the problem precisely enough that the solution space (conventions spec, profile of an existing protocol, new session layer) can be evaluated against it.

The precise formulation is this: **the coordination primitives themselves — session identity, attribution envelope, delegation lineage, control propagation, stream membership — have no common definition at any layer, so every stack defines them independently and incompatibly.** Where the common definition should live is a solution question with real trade-offs in both directions: conventions are cheap to publish but historically suffer optional adoption, partial implementation, and semantic drift (the current three-way divergence occurred *despite* all three stacks having identical HTTP machinery available — shared carrier, divergent semantics); a distinct layer is testable and enforceable but must overcome bootstrap adoption and avoid becoming the fragmentation it was meant to end. This document takes no position beyond requiring that any solution be judged against the failure modes above.

One consequence of the formulation deserves its own sentence, because it is the composition stake for the entire suite: whatever form the common definition takes, it is the **attachment point** the higher primitives currently lack. Identity binding, authorization chains, delegation lineage, session context, and audit correlation are today each re-anchored to HTTP requests and DNS names separately, per stack, because there is nothing else to anchor them to. A commonly defined coordination unit — however delivered — is what would let documents 02 through 12 stop being rebuilt against carriers that do not know their unit exists. That is a statement about the cost structure of the status quo, not about any solution's shape.

## Prior art, and what it constrains

Purpose-built coordination substrates have been attempted before, and a problem statement that ignores them will be dismissed by anyone who remembers them. Each solved a subset of the requirements above; none became the agent substrate; each carries a lesson that constrains the solution space.

Read against the design axes above, the prior art resolves into something more useful than a list: **each prior system is a different answer-set on the same questions.** SIP chose stateful, session-as-unit, and coordination methods at the wire; HIP chose identity-native addressing; MQTT and AMQP chose stateful with broker-held state; the group-communication tradition chose multi-party native; HTTP chose the bundle agents inherited. Each system's successes and failures are evidence about its answers — which is what makes the history a constraint on the solution space rather than trivia.

**SIP** is the closest structural analog and answers three axes at once: a stateful session layer composed over multiple carriers, with the session as its unit (mid-session modification via re-INVITE, session-scoped cancellation via CANCEL and BYE, forking to multiple participants, multi-modal negotiation via SDP) — and with *coordination methods at the wire*: INVITE, CANCEL, REGISTER, and BYE are intent-dense method names, deployed at global scale, decades before anyone measured why that matters. SIP demonstrates that the stateful, session-unit, semantic-method answer-set is standardizable and operable. It also demonstrates the costs: NAT and middlebox traversal consumed a decade of engineering, and B2BUAs in practice broke the end-to-end session semantics the protocol promised — a direct warning for any claim that attribution will survive relays because the spec says so.

**Group communication systems** — the virtual-synchrony tradition (ISIS, Spread, JGroups) — are the direct precedent for the multi-party session as a first-class unit: view-synchronous membership (all survivors agree on who is in the group after a join, leave, or crash) and totally or causally ordered multicast (control messages with defined ordering relative to other traffic) were solved problems by the mid-1990s. They are proof that the properties this document requires are buildable — and they were built for single administrative domains, LAN-era latencies, and closed memberships, and never crossed into the multi-organization, multi-carrier, pause-tolerant setting. The suite's recurring single-trust-domain pattern has its transport-layer instance here: the multi-party unit exists, inside one operator's walls.

**XMPP** delivered presence, federation, and long-lived sessions — and then recreated the conventions problem *inside* the substrate, as interoperability fractured across optional XEPs. Lesson: a common substrate with an open extension model can reproduce the very fragmentation it targets; the core primitive set must be mandatory and small.

**AMQP and MQTT** provide delivery guarantees, ordering, and deduplication — the control-message reliability requirements above — but are broker-centric, which imports an operator-model question (who runs the broker, especially across organizations?) that any agent substrate must answer explicitly rather than inherit silently.

**Matrix** is the closest existing answer to cross-organization session continuity: federated, replicated room state that survives any single server. Its cost is the complexity of state resolution across federation — a preview of what cross-org session state actually requires.

**DIDComm** defines per-message attribution and encryption as a carrier-independent envelope, demonstrating that the attribution requirement is satisfiable at message granularity without a new wire protocol — and, in its limited adoption, demonstrating how hard bootstrap is for an envelope standard with no killer transport.

The constraint this history imposes on the problem statement itself: the requirements above are *individually* met by prior systems. What no system provides is their *combination* under one session model — SIP's session semantics, DIDComm's attribution, Matrix's cross-org state, AMQP's delivery guarantees — and any proposed solution must explain specifically why it will avoid the adoption and fragmentation fates of these predecessors.

## The core problem case: a live multi-party session under mid-session change

This case is what the inherited answers cost at run time. Strip away everything the rest of the suite owns, grant the skeptic's best world — excellent QUIC or WebTransport connections between every pair of endpoints that has one — and watch the stateless, connection-unit, identity-above, content-method answers fail together.

A coordination session is live across four participants. Two are directly reachable; one sits behind a relay; one is reached over a different carrier entirely — a media session rather than an HTTP-family connection. The session has run for two days, spans two organizations, and carries concurrent streams: structured data, a media channel, control traffic. Mid-session, in quick succession:

- One participant's process restarts, and another's network path changes — new address, new connection, same participant.
- A high-priority control act — a cancellation and a revocation — must reach every live participant and preempt specific in-flight work, including a bulk transfer already in progress to one of them.
- One subset of the session's streams is torn down while the remainder continues, across participants on different carriers.
- Throughout, the logical session identity, the control state (what has been cancelled, what survives), and the attribution of every message must remain consistent for the surviving participants — without a full group re-handshake, which would itself destroy the in-flight state the session exists to protect.

**What breaks today.** Each behavior is implemented differently per framework and per carrier, or not at all. The restart severs one connection and, with it, everything that framework kept connection-scoped; the survivors' view of the session now diverges from the restarted participant's, and there is no shared object to reconcile against. The path change is handled *within* one QUIC connection (migration) and is meaningless *across* the session — the other participants have no session-level fact to update, because no session-level fact exists. The preempting control act has no cross-participant, cross-carrier ordering: it is expressible, at best, as N separate per-connection messages bearing no defined relationship to the in-flight work on any connection but their own — so it races the bulk transfer and the race has no rules. Partial teardown exists per stream, per connection, and nowhere as a session operation. The finding, compressed: **no common session identity across carriers, no common propagation and preemption semantics for control messages, no common partial-teardown visibility — at any layer.**

**What any adequate answer must make possible — at this layer.** A session identity that outlives every connection and carrier that serves it, updateable in every participant's view when a member restarts or moves; delivery of one control act to all participants across their respective carriers, with defined ordering ahead of the in-flight work it preempts and with per-participant confirmation or known failure; partial teardown as a session operation spanning carriers; membership change — join, leave, restart, address change — without a group re-handshake; and attribution that remains verifiable across hops and carrier changes (the binding itself per documents 02 and 07).

**What no answer can promise, and this document does not.** Preemption racing irreversible effects is two-generals-adjacent: no substrate makes "the cancel arrives before the work completes" atomic across independent organizations, and the achievable property is *defined* race semantics — ordered, tracked, confirmed propagation with determinate outcomes, so every participant knows whether the cancel won, what completed, and what requires compensation (the compensation itself belongs to document 08). Nor can any answer promise a perfectly consistent membership view under network partition — the group-communication literature settled the costs of view agreement decades ago, and cross-organization operation pays them at higher latency and lower trust; the requirement is a defined view-change semantics, not an impossible instantaneous one.

**Open question this case exposes.** The operator model. Is the substrate an embedded library all parties speak (SIP-style), a broker (AMQP-style), or a federation (Matrix-style)? Membership semantics, control propagation, and cross-organization continuity have different shapes under each. This is a solution-space question, flagged because the case cannot be fully evaluated without it.

## A concrete illustration: the trip booking

The abstract case above is what the following, more accessible scenario looks like in practice — and this scenario is deliberately the suite's shared scenario, reused by later documents.

An assistant agent is booking a business trip for its principal. The workflow involves four coordination targets: the principal (through voice conversation), a partner organization's travel-service agent (through agent-to-agent coordination), an airline API (through tool invocation), and a hotel booking API (through tool invocation). All four coordinate within a single logical session on the assistant's behalf, under a single delegation from the principal.

The coordination is inherently multi-modal. The voice channel with the principal is active. Structured data flows between the assistant and the travel-service agent (flight options, hotel comparisons, pricing). Tool invocations exchange JSON payloads with the airline and hotel APIs. Chat messages summarize progress. All four streams are concurrent; all four contribute to the same coordination.

Mid-way through the workflow, the principal says "cancel the hotel, keep the flight." The cancellation must propagate across four participants. The voice channel stays active and is used to confirm the cancellation. The bulk transfer of hotel comparison data must be aborted mid-transfer. The travel-service agent that received hotel preferences must revoke the shared context. The hotel API that has reserved a room must release it. If the airline transaction has already charged the corporate card, it proceeds; if not, its state must be tracked.

**This document claims only the scenario's carriage slice.** The workflow recurs through later documents, each owning what it owns: whether the cancellation was authorized, and how the sub-delegation's revocation takes effect, belong to documents 03 and 04; what compensates when an irreversible charge wins the race belongs to document 08's partial-failure semantics; the joined record afterward belongs to document 12; the identities behind every leg belong to document 02. What remains when all of that is subtracted is the abstract case above, instantiated: **one cancellation act delivered to four participants across three carrier protocols — WebRTC, HTTP-based A2A, plain HTTPS — ordered ahead of the in-flight bulk transfer it preempts, tearing down the hotel streams while their siblings on other carriers continue, with delivery confirmed or its failure known.** Everything else the scenario needs presupposes that this is possible.

**What breaks today.** The assistant maintains three or four protocol stacks: WebRTC for voice, A2A for agent coordination, MCP for tools. Each of the failure modes catalogued above appears in this one workflow. The cancellation is translated into each stack's semantics manually, and races the hotel confirmation on a slower path (cancellation race). If the assistant restarts mid-workflow, the travel-service agent still holds live stream state the assistant can no longer name (post-restart divergence). The audit trail spans four log formats and four identity namespaces (audit fragmentation). Revoking the hotel-related sub-delegation takes effect at different times on different legs (non-uniform revocation). And no identifier anywhere names the trip-booking workflow as a whole (no shared session identity). Of these, the post-restart, audit, revocation, and session-identity failures find their logical-layer resolutions in later documents (08, 12, 03/04, and 08 respectively) — and every one of those resolutions rides on the carriage question: a session model cannot recover what was never deliverable, and a revocation semantics cannot bind what cannot be ordered.

The requirements, limits, and open questions this illustration exposes are those of the primary case above, and are not restated.

## Additional coordination needs

Each item below is stated as an observable coordination need or present-day failure, not as a feature of a presupposed solution. Items whose primary ownership lies in a later document in this series are tagged.

**Session lifecycle**
- Resume a coordination session intact after a network outage, a process restart on either side, or an intentional multi-day pause for human review or an awaited event — today each stack resumes (or fails to) independently, and cross-stack sessions resume partially.
- Hand off a live multi-stream session from Organization A's agent to Organization B's agent without losing cancellation capability, attribution, or stream identity — today this requires teardown and re-establishment, losing in-flight state.
- Admit an ephemeral specialist agent into a session for one decision and remove it cleanly, without forcing remaining participants to re-establish state.
- Correlate related sessions across time into one logical workflow for audit and continuation. *(Shared ownership with the audit document.)*

**Control propagation**
- Propagate one cancellation to all in-flight streams and participants of a session, ordered ahead of the work it preempts — today cancellation is per-stack and races the happy path.
- Deliver a high-priority control message (cancellation, change-of-plan) ahead of an in-progress bulk or media transfer to a *different* participant — carrier-level priority is per-connection and cannot express this.
- Cancel one stream of a session while the others continue, with a determinate record of what stopped and what completed.
- Enforce a mid-session revocation of a specific sub-delegation across all participants before further side effects, or determinately record where enforcement arrived late — today enforcement timing varies per stack's token model. *(Shared ownership with the identity document.)*
- Barge in on an active stream with a new request without tearing down the session.

**Attribution and audit**
- Carry acting-agent identity and full delegation lineage at message granularity, varying within a session as authority context changes. *(Requirements owned jointly with the identity document.)*
- Preserve end-to-end attribution through gateways and relays — the specific failure SIP's B2BUA history warns about.
- Reconstruct a single causal trace and ownership chain for a workflow that today spans four log formats and identity namespaces. *(Shared ownership with the audit document.)*

**Multi-modal and multi-participant**
- Treat concurrent voice, video, structured-data, and chat streams as members of one coordination session spanning multiple participants and carriers.
- Route coordination between agents in different organizations without dependence on either organization's private session infrastructure.
- Federate across substrate deployments while preserving attribution and session identity. *(Dependent on the operator-model question above.)*

**Delivery semantics**
- At-least-once delivery with deduplication for control messages across a participant restart while other participants remain live.
- Defined ordering between control messages and the operations they govern, within a session that spans multiple carrier connections.
- Recover a coherent, mutually agreed session state after partial failure of any participant.

Two bullets from earlier drafts are removed rather than restated: "carry semantic intent as a substrate primitive" (a solution feature, not an observable need; the need it gestured at is covered by the message-semantics document) and "advertise participant availability ambiently" (a discovery-layer concern; moved to the discovery document).

## What this document does not claim

- **Not** that HTTP-family or QUIC-family protocols cannot carry the bytes. They demonstrably can; every current stack proves it. The claim is a design-center mismatch: their answers — stateless, content-method, identity-above, connection-unit — were right for document delivery, and inheriting them for coordination makes coordination's properties expensive to build, non-interoperable when built, and rebuilt separately by every stack.
- **Not** that the questions' answers are foregone. This document poses the axes — stateless or stateful, content or coordination methods, identity above or native, connection or session as unit — and does not decide them. The evidence it offers is that the *inherited defaults* are strained: the universal five-way inversion pattern, and the measured method-semantics result. Strain warrants deliberate examination; it does not pre-select stateful, semantic, or native as the verdicts, and honest costs sit on every side of every axis.
- **Not** that existing stacks are non-functional. They work, within their own boundaries; the failures catalogued here occur between and across them, and in the per-stack reconstruction cost.
- **Not** that a new wire protocol is the necessary answer. Whether the deliberate answers take the form of a conventions profile, an envelope standard, or a purpose-built transfer layer over QUIC-family carriers is left open — the last of these being the skeptic's own fallback, answered above as a candidate solution rather than an objection.
- **Not** that the individual requirements are novel. Each axis has been answered non-default somewhere in prior art — stateful sessions and semantic methods in SIP, identity-native addressing in HIP, multi-party units in group communication — inside bounded domains. The unmet need is a deliberate, common answer-set for the agent case, across organizations and carriers, with an adoption story that survives the fates of SIP, XMPP, HIP, and DIDComm.

The claim is: the transfer layer that carries agent coordination has design axes — state model, method semantics, identity placement, primary unit, payload model — that have never been posed as questions for the agent case; every current stack inherits HTTP's answers, correct for document delivery, and then inverts all five above the wire, privately and incompatibly; the inversion pattern is the observable evidence of misfit, the failures catalogued above are its running cost, and because the transfer layer is the placement decision for every primitive in this suite, the inherited placement — everything above the wire, per stack — is why documents 02 through 12 each find their primitive rebuilt against carriers that do not know it exists.

## Forward dependencies

Later documents in this series lean on specific claims made here; contradictions across the set should be checked against this list. The recurring shape of every entry is the placement split declared in the scope section: this document owns *where* a primitive lives relative to the wire; the named document owns *what* the primitive is.

- **Identity (02):** owns what the agent identifier must be; this document owns the placement axis (above the protocol versus protocol-native addressing) and establishes that per-message attribution and hop-surviving lineage are transfer-layer-shaped requirements — if identity proposes token-based lineage, it must answer the hop-dissociation failure described here.
- **Discovery (06):** owns identifier resolution, and receives the "ambient availability" concern removed from this document's use-case list; the identity-placement axis determines whether resolution resolves an annotation or a protocol element.
- **Communication (07):** owns the message envelope; this document owns what the protocol beneath it makes native versus conventional, and the payload-model axis (intent-plus-context, multi-modal coordination signals) sets the envelope's operating assumptions.
- **Session (08):** owns the logical session object — lifecycle, membership, continuity evidence — and receives the session-identity gap established here; this document owns the prior axis of whether the wire knows state exists at all, and the carriage-level facts (deliverability, ordering, membership-change mechanics) the session object is defined over.
- **Semantics (09):** owns the coordination-act vocabulary and its meaning; this document owns the method-surface axis — whether the transfer protocol's methods carry intent — for which 09 now holds the controlled empirical evidence.
- **Audit (12):** assumes the audit-fragmentation failure mode and the "single causal trace" requirement established here.
- **All solution-oriented documents:** are constrained by the prior-art section read as answer-sets on the design axes (SIP's stateful/semantic-method answers and their middlebox costs, HIP's native-identity answer and its adoption fate, group communication's multi-party answer and its domain bound, XMPP's mandatory-core lesson, AMQP/Matrix's operator-model question) and by the non-claims above.

---

[README](README.md) &nbsp;·&nbsp; [Identity →](02-identity.md)
