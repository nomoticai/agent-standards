# 10. Tools (TOL)

## Scope, status, and terminology

This document is a problem statement, not a solution proposal — and it differs structurally from the nine before it, which each argued that a primitive lacked a common definition. This document argues no new primitive into existence. Its claim is about the **participant model** of the substrate the previous documents describe: that tools must be first-class participants in the same identity, discovery, authorization, delegation, trust, communication, session, and semantics fabric as agents — because the tool/agent partition, wherever it is enforced at the coordination layer, is exactly where each of those primitives currently breaks. It is, in effect, the suite's first composition test. It stops short of prescribing an invocation protocol, an attestation scheme, or a sandboxing model.

This framing is not new to the suite; it is the formalization of an assumption made since its first page. Document 01's core scenario — the trip booking — treated the airline and hotel APIs as full session participants alongside the principal and a partner agent, subject to the same cancellation propagation, the same attribution, the same audit trail. This document owns the claim that scenario presupposed.

**Terminology.** "Tool" here names a *usage profile*, not a protocol category: typically narrower capability, atomic invocation, deterministic execution, tighter containment. The profile is real and worth expressing; the claim is that it does not warrant a separate protocol universe. **Boundary with Semantics (09):** that document handed this one capability and function *description* — what a tool or agent can do — as distinct from the coordination *acts* agents perform toward each other; the handoff is received below, with a security consequence attached that makes description ownership more than bookkeeping.

## The skeptical case

The skeptic here holds the strongest deployed position in the suite. MCP is the dominant tool protocol, supported by every major model provider, with new tools wrapped daily; function calling is native to every serious framework; plugin architectures, REST, and automation platforms (Zapier and kin) cover the rest. Tool invocation is the fastest-moving, best-served layer of the entire agent ecosystem. Combine MCP for description and invocation with OAuth at each service boundary and existing platform credentials, and the tool problem is not merely solved — it is the one part of the agent stack that demonstrably works at scale today. Adding substrate ceremony to it risks breaking the ecosystem's one success.

A second skeptic argues from the other side: tools and agents are *genuinely different* — tools are atomic, deterministic, narrowly scoped, and sandboxable; agents are stateful, stochastic, and broad — and collapsing the distinction is category error, not simplification.

Both skeptics are partly right, and the document's claim threads between them.

## Where the mismatch actually lies

To the second skeptic: conceded, in full, at the profile level. Atomicity, determinism, and containment expectations are real asymmetries, and they *survive* shared participation — a deterministic executor and a stochastic interpreter legitimately carry different kinds of trust evidence (supply-chain attestation for one, behavioral history for the other, per document 05's evidence framing), different lifecycle shapes, different containment postures. The claim is not that these differences vanish; it is that they are *profile attributes the fabric must express*, not grounds for a protocol partition. At the coordination layer, both tools and agents are software entities receiving structured requests and returning structured results, and the category boundary between them is famously unstable anyway — a maps service, a translation service, a booking service each sit wherever the describer's framing puts them.

To the first skeptic: MCP's success is real and is not contested — it solved description and invocation *within a specific trust posture*: a user or operator configures servers they have decided to trust, inside one deployment's boundary. The failures appear precisely at this suite's concerns, where that posture runs out:

**The partition breaks each primitive at the boundary.** A workflow that crosses from agent coordination into tool invocation crosses identity models (OAuth against one provider, vendor credentials at another, A2A conventions at a third), so document 02's attribution collapses to service credentials exactly at the tool step. Delegation lineage — documents 03 and 04 — dies at the invocation boundary: the tool sees a service credential, not the principal, the mandate, or the chain, and downstream calls from the tool start a fresh causal history. Discovery forks: a capability satisfiable by either a narrow tool or a general agent requires querying two regimes and reconciling the result sets by hand. Audit fragments at the same seam: the investigator of a mixed workflow correlates tool-platform logs and agent-stack logs by timestamp and hope. Each of these is an earlier document's failure mode, localized to the same architectural line.

**The partition multiplies bridges.** Every combination of tool protocol and agent protocol in a workflow is a translation surface, and the integration tax grows with the product — the suite's recurring quadratic, here at its most concrete, since mixed workflows are not the exotic case but the normal one. (The honest footnote from document 01 applies: any common layer collapses the product to a sum — including a hub operator, which is what Zapier already is; the prior-art section takes up what that answer costs.)

**And the boundary is the active attack surface.** This is the failure the earlier drafts missed and the ecosystem is currently living through. Tool *results* are attacker-influenced content that agents ingest as trusted input — the deployed instance of document 07's in-band problem, and the inbound half of document 03's confused deputy: most real injection attacks on deployed agents arrive *through* a tool return. Tool *descriptions* are an injection surface of their own — instructions embedded in the description text the model reads (description poisoning), trusted servers whose tools silently change after approval (rug pulls), lookalike and shadowing servers. The public MCP security literature of the past year is a catalogue of exactly these. The partition makes this worse, not better: integrity checking, such as it exists, runs through a different evidence path at every stack, so a spoofed email tool, a hijacked calendar integration, and a fraudulent partner agent — equivalent compromises in consequence — are checked by three unrelated mechanisms under three sets of assumptions, or not at all.

## What observably breaks today

**The tool step is where the principal disappears.** A chain that begins with a human principal, passes through an assistant agent, and invokes a tool delivers, at the tool, a service credential — no principal, no purpose, no lineage. Everything documents 02–04 require of the chain is severed at its most common hop.

**Tool results are the injection channel.** Content returned by tools — web pages, documents, records, another service's text — is treated as trusted input by the invoking agent, and carries attacker instructions in practice. No deployed tool protocol marks the provenance or trust class of a result in a way the substrate can act on.

**Descriptions are unverified and mutable.** The text that determines whether and how an agent invokes a tool is supplied by the tool's operator, readable by the model, unsigned in practice, and changeable after approval — poisoning and rug-pull attacks follow directly.

**Invocation-time integrity does not exist.** Supply-chain attestation for software artifacts is deployed (signing, provenance, reproducible builds) and is not consulted at the moment an agent invokes an endpoint claiming to be that software; "is this the expected implementation" is unanswerable in-band.

**Discovery is forked; audit is forked.** Capability search runs separately over tool registries and agent directories, reconciled by the requester; workflow reconstruction runs separately over per-stack logs, reconciled by the investigator — with no common correlation identifier across the tool/agent line. *(Documents 06 and 01's failures, localized.)*

**Standing tool credentials are the over-provisioning norm.** Because per-invocation scoped authority is expensive under current stacks, tools hold broad long-lived credentials — document 03's over-provisioning failure, concentrated at the layer with the largest attack surface.

## Properties without a common definition

Framed as what the participant model must make answerable, uniformly across the tool/agent line:

- **Who am I invoking:** the invoked entity's identity, verifiable before access, independent of its hosting — the identifier/locator split of document 02, applied at invocation.
- **Is it what it claims to be:** implementation integrity at invocation time, connecting deployed artifact-attestation practice to the moment of use, with the evidence type appropriate to the profile (attestation for deterministic executors; document 05's behavioral evidence for adaptive ones).
- **Whose authority does it act under:** the delegation chain arriving *at* the tool intact — principal, mandate, scope — and continuing *through* it for downstream calls. *(Documents 03/04, through the boundary.)*
- **What did it say it can do, and who vouches for the saying:** description as an owned, attributable, integrity-protected object — the seam received from document 09, now carrying its security weight.
- **What trust class is its output:** result provenance expressible at the substrate, so a tool return is not silently promoted to trusted input — the tool-boundary application of document 07's instruction/content separation, with that document's honest limit inherited: separation removes the structural confusion, not the possibility of persuasive content.
- **Where does it appear in search:** one capability query, one result set, tools and agents together, filterable by profile attributes. *(Document 06.)*
- **How is it contained:** the profile's containment expectations (sandboxing, atomicity, statelessness) expressible as attributes counterparties can read, rather than assumptions they cannot check.
- **How does the workflow read afterward:** one correlated record across every tool and agent step. *(Documents 01/08's session and audit fabric, spanning the boundary.)*

## Prior art, and what it constrains

**Unix** remains the best illustration of the payoff and now carries its two honest limits. Uniformity — every tool a process, stdin to stdout, composed by pipes — is why pipelines exist; and it holds because everything runs under *one kernel, one administrator, one trust domain*: the single-operator condition, again (document 08's durable-execution echo). And pipes achieve their uniformity by stripping everything but bytes — no attribution, no lineage, no authority crosses a pipe. The analogy licenses uniform *interface*; it is silent on precisely the properties this suite requires the interface to carry.

**The actor model** (Erlang/OTP and descendants) is the theory-clean version of the claim — everything a uniformly addressable message-receiving entity — proven within single runtimes, never across organizational boundaries. **Object-capability systems** are the theory-clean answer to authority-through-invocation — the capability travels with the call — and their perennial non-adoption outside research systems is the suite's standard bootstrap warning, here applied to the delegation-through-tools requirement.

**CORBA and COM** are the graveyard headline: uniform object invocation across vendors and languages, tried at full industry scale, dead of complexity, vendor politics, and interface-versioning misery — the elder sibling of document 07's WS-\* corpse. Any "one invocation model for everything" proposal answers to CORBA by name.

**MCP** is treated as what it is: the deployed, adopted solution to description and invocation within a configured-trust posture — and the most direct evidence for this document, because its known gaps and its incident literature (description poisoning, rug pulls, server spoofing, absent lineage, per-server credential sprawl) map one-to-one onto the failure modes above. The realistic path runs through MCP's installed base, not around it, which constrains solutions toward extension and profile rather than replacement.

**Supply-chain attestation** — signing infrastructure, provenance frameworks, reproducible builds — is the constructive deployed answer to implementation integrity, currently terminating at the artifact registry rather than the invocation; the gap is wiring, not invention. **Browser and mobile permission models** are deployed per-tool containment and scoped grants at consumer scale, with permission fatigue as the standing lesson — document 03's over-provisioning dynamic reproduced in every "Allow" dialog. **Zapier and the integration-hub pattern** prove the tax can be collapsed to a sum today — through a central operator who becomes the ecosystem's chokepoint, pricing authority, and single point of failure: the operator question's fourth appearance in the suite, and the strongest evidence that the sum is worth something, since a business of that size is built on providing it.

The constraint the history imposes: interface uniformity is proven within trust domains (Unix, actors) and failed across them at maximal ambition (CORBA); authority-carrying invocation is solved in theory (ocap) and unadopted; integrity attestation exists and is unwired; the sum-not-product economics are proven and currently purchasable only from a hub. The unmet combination is uniform *participation* — identity, lineage, evidence, audit — across the tool/agent line and across organizations, achieved without a mandatory central operator.

## The core use case: composing tools and an external agent in one workflow

An assistant agent schedules a meeting for its principal with a partner at another organization. Three coordination targets: a calendar tool in the principal's environment, the partner's agent across the organizational boundary, and an email tool for confirmations. All three act under the same principal and the same delegation; the outcome should be one workflow with one intact chain and one audit trail.

**What breaks today.** Three protocol stacks (MCP or REST for the calendar, A2A or convention for the partner's agent, an SDK for email); three identity models to verify and rotate; three renderings of the principal's delegation — OAuth scopes in one, A2A attribution in another, nothing at all in the third — so the lineage exists nowhere end-to-end. When the partner's agent proposes new times, the assistant hand-translates between the agent-message world and the calendar-API world, and every crossing is a surface for drift and lost attribution. If any participant is compromised — spoofed email service, hijacked calendar integration, fraudulent partner agent — each is checked through a different evidence path under different assumptions, and the calendar tool's *results* (availability data that will steer the whole negotiation) enter the assistant's context as unmarked trusted input. Afterward, reconstruction means correlating three log formats by timestamp. Reviewer's note made concrete: the tool steps are where the workflow is most opaque, most credentialed, and least attributed — simultaneously.

**What any adequate answer must make possible.** Each participant — tool or agent — verifiable as an identified entity before engagement, through evidence appropriate to its profile; the delegation chain arriving at and passing through every step, so the calendar tool sees the principal and scope, not a service key; results carrying provenance the substrate can act on; one capability-discovery surface and one correlated record across all three participants; and a fourth participant added on the same terms rather than through a fourth bridge.

**What no answer can promise, and this document does not claim.** That uniform participation ends injection: a provenance-marked tool result can still contain persuasive falsehood — document 07's limit, restated at its most active channel; the structural gain is that untrusted output stops being silently promoted to trusted input, and no more. That the profile differences dissolve: atomicity, determinism, and containment remain, expressed rather than erased. That the tax collapses without an operator answer: sum-not-product must be earned against the hub alternative, which exists, works, and charges for itself.

**Open questions this use case exposes.** The profile vocabulary (which tool/agent asymmetries the fabric must express); where invocation-time integrity evidence is anchored and by whom; and the migration path through MCP's installed base — all solution-space; the failures stand regardless.

## Additional participation needs

Stated as observable needs or failures; primitive ownership is as tagged throughout the suite.

**Identity and integrity**
- Verify what is being invoked, as an identified entity, independent of where it is hosted. *(02.)*
- Verify at invocation time that an endpoint is the attested implementation it claims — connecting existing artifact attestation to the moment of use.
- Detect that a previously approved tool's description or behavior has changed since approval.

**Authority through the boundary**
- Deliver the principal, mandate, and scoped authority *to* a tool, and carry the chain *through* it for downstream calls — ending the service-credential collapse. *(03/04.)*
- Scope tool authority per invocation or per task where warranted, against the standing-credential norm. *(03's over-provisioning.)*

**Description and results**
- Treat capability descriptions as owned, attributable, integrity-protected objects. *(Seam from 09.)*
- Mark the provenance and trust class of tool results so the substrate — not habit — decides their standing as input. *(07's separation, at the tool channel.)*

**Discovery and composition**
- Return tools and agents in one capability result set, filterable by profile. *(06.)*
- Compose multiple tools and agents in one workflow without per-pair bridges, with the workflow's session and audit spanning every step. *(01/08.)*
- Express containment and atomicity expectations as readable profile attributes.

**Ecosystem**
- Share tools across organizations with attribution intact. *(Commercial terms forward to 11.)*
- Handle tool versioning such that counterparties can detect a version change as a fact, not an incident.

## What this document does not claim

- **Not** a tenth primitive. The claim is a participant-model requirement across the nine that exist; if the substrate's primitives cannot uniformly cover a calendar API, they were never the substrate the earlier documents described.
- **Not** the erasure of the tool/agent distinction. Profile asymmetries — atomicity, determinism, containment, evidence type — are real, load-bearing, and to be *expressed* by the fabric; the claim is confined to ending the protocol partition.
- **Not** that uniformity solves injection. The tool channel remains the active attack surface; structural separation of provenance from content removes silent promotion of untrusted output, and inherits document 07's stated limit past that point.
- **Not** MCP's replacement. MCP is the installed base and the adoption path; the claim is about what it and its peers do not yet carry across the boundary — identity, lineage, integrity-at-invocation, provenance, unified discovery, unified audit.
- **Not** trust scores for tools. Document 05's evidence framing governs; the tool profile changes the *type* of evidence, not the retired-score model.

The claim is: the tool/agent partition at the coordination layer is where every earlier primitive currently fails — attribution collapses to service credentials, lineage dies at invocation, discovery and audit fork, integrity is checked inconsistently or not at all, and the tool channel carries the ecosystem's live injection attacks — while mixed tool-and-agent workflows are the normal case, not the edge; and the participation of tools in the same identity, discovery, authorization, delegation, trust, communication, session, and semantic fabric as agents is therefore not a tenth requirement but the condition under which the first nine mean anything in practice.

## Forward and backward dependencies

- **Transport (01) and Session (08), backward:** the trip-booking scenario's treatment of APIs as session participants is formalized here; sessions and cancellation propagation span tool steps; the audit-fragmentation failure localizes to the tool/agent line.
- **Identity (02), backward:** invoked-entity verification, hosting-independent identity, and instance distinction apply to tools verbatim.
- **Authorization (03) and Delegation (04), backward:** the confused deputy's inbound channel is the tool result; over-provisioning concentrates at tool credentials; lineage-through-invocation is the boundary form of chain preservation; mandate scope arrives at the tool or nowhere.
- **Trust (05), backward:** evidence framing governs, with profile-appropriate evidence types (attestation vs. behavioral history) as this document's contribution to it.
- **Discovery (06), backward:** the unified result set is the boundary form of capability search.
- **Communication (07), backward:** result provenance is the tool-channel application of instruction/content separation, limits included.
- **Semantics (09), backward:** the description seam is received, with description integrity attached; invocation acts use the act layer without presuming its vocabulary.
- **Commerce (11), forward:** receives cross-organization tool sharing's commercial dimension — metering, attribution of usage, and revenue terms ride on the participation fabric claimed here.
- **Audit, forward:** the single correlated record across tool and agent steps is jointly owned; invocation-time integrity evidence is audit substrate.

---

[← Semantics](09-semantics.md) &nbsp;·&nbsp; [README](README.md) &nbsp;·&nbsp; [Commerce →](11-commerce.md)
