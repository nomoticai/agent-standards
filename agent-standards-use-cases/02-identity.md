# 2. Identity (IDY)

## Scope, status, and terminology

This document is a problem statement, not a solution proposal. It argues that agent deployments have identity requirements that existing workload-identity and organizational-identity systems do not meet in combination, and that the absence of any common definition for what an agent identity names, how it persists, and how it is verified across organizational boundaries produces observable failures today. It stops short of prescribing an identifier syntax, an issuance model, or a trust architecture.

Three distinct things travel under the word "identity" and this document scopes them explicitly:

- **An identifier** — a stable name for an agent (the SKU-like reference).
- **Authentication** — the ability to verify that a party presenting an identifier is the agent it names (without this, an identifier is a label anyone can wear).
- **Attribution** — the binding of actions and messages to the authenticated identifier (the property the transport document's per-message requirement depends on).

All three are in scope here as *requirements*; richer identity attributes (roles, capabilities, reputation) are owned by the authorization and trust documents and are referenced only where the identifier must anchor them.

## The skeptical case

The naive skeptical case — "agents are software, and servers already have IP addresses" — is not worth answering; nobody managing production infrastructure identifies software by the host it runs on today. The serious skeptical case is much stronger: **workload identity is a largely solved problem.**

SPIFFE and SPIRE exist precisely to give software workloads stable, verifiable identities that are independent of node, IP, and process — issued through platform attestation, carried in short-lived SVIDs, usable as the reference for authorization and audit. Kubernetes separates service identity from pod identity so the logical service persists while its instances churn. Cloud IAM service accounts, Active Directory service principals, and OAuth client credentials all name software actors independent of their hosting. Host Identity Protocol solved the identifier/locator split at the network layer two decades ago. mTLS with workload certificates is routine.

The skeptic's conclusion: an agent is a workload. Give it a SPIFFE ID or a service account like any other workload, and stop proposing new identity layers — the industry has seen enough of those, and every additional identity namespace is an integration burden and an attack surface.

This case must be answered on its merits. Much of what an "agent identity" proposal might claim as novel — host-independence, multi-tenancy on shared infrastructure, survival across infrastructure changes — is delivered by existing workload identity within a single trust domain. The unmet requirements lie elsewhere.

## Where the mismatch actually lies

Workload identity names a *deployment artifact within one organization's trust domain*. Agent coordination requires naming an *acting entity, on behalf of a principal, verifiable across organizational boundaries, at message granularity, across a lifecycle that includes reconstitution, forking, and ephemeral spawning*. The gap has five specific components:

**No common answer to what the identity names.** Is the subject the model, the scaffold, the configuration, the deployed instance, or the version? When an agent is updated in place, forked, or reconstituted from checkpoint on different infrastructure, is it the same agent? When an agent spawns twelve ephemeral sub-agents for one task, does each carry a first-class identity, and what happens to those identities afterward? Workload identity has a clean answer for its domain (the attested workload as deployed); no common answer exists for agents, and every stack currently improvises one. This is the direct analog of the transport document's core finding: the primitive lacks a common definition, so each stack defines it incompatibly.

**No principal binding.** An agent acts on behalf of a human or organizational principal, frequently under delegation that changes mid-session. A SPIFFE SVID names the workload; it says nothing about whom the workload is acting for, and was never designed to. The identity primitive agents need must anchor the delegation context that the transport document requires at message granularity — not carry the full delegation mechanism (that is the delegation document's problem), but provide the stable subject to which delegation attaches and travels.

**No cross-organization verification model.** SPIFFE federation exists but is pairwise, configuration-heavy, and rare in practice; cloud IAM and service accounts stop at the organization's boundary entirely. When organization B receives a message from organization A's agent, B can verify — at best — A's TLS certificate: the *organization*, not the agent. Every claim below the organization level (which agent, which version, which delegation) is unverifiable assertion. The web PKI solved organization-level identity for servers; nothing widely deployed extends verification to the individual acting agent across a boundary between parties with no shared IAM.

**Lifecycle dynamics outside workload-identity assumptions.** Agent populations spawn, fork, migrate, pause for days, and retire at cardinalities and rates that service-account lifecycle tooling was not built for — and retirement must preserve attributed history, while revocation must have defined semantics for what happens to that history and to the agent's outstanding sessions.

**Message-granularity binding.** The transport document establishes per-message attribution as a substrate requirement and warns that point-to-point tokens dissociate lineage at each hop. The identity layer must therefore provide an identifier that can be bound to individual messages and survive relays — a requirement connection-scoped workload certificates do not address.

## What observably breaks today

**Attribution collapses to shared credentials.** In most current deployments, "which agent did this" resolves to a shared API key, a shared service account, or a model-provider organization ID. After an incident, the audit log names the credential, not the agent; investigation reconstructs the actor from surrounding context, or fails to.

**Orphaned agents.** Agents continue running under the credentials of a departed employee, a deleted project, or a decommissioned pipeline, because nothing binds the agent's lifecycle to an identity with an owner. Fleet inventory — which agents exist, who deployed each, which are still supposed to be active — is unanswerable in any organization past trivial scale.

**Sibling indistinguishability.** Near-identical agents (same model, same role configuration, same tools, same image, often the same host) run concurrently. When one misbehaves, the operator cannot attribute the behavior to a specific instance, and cannot revoke or quarantine that instance without affecting its siblings, because nothing below the shared image and shared credential distinguishes them.

**Cross-stack identifier mismatch.** The same logical agent carries different identifiers in MCP, A2A, and internal logging contexts. The transport document's audit-fragmentation failure is partly an identity failure: the causal trace cannot be joined because no common identifier spans the stacks.

**Reconstitution breaks references.** An agent reconstituted on new infrastructure mid-task presents new instance-level identifiers; outstanding sessions, grants, and audit references point at identifiers that now name nothing. Counterparties cannot distinguish "the same agent, moved" from "a different agent claiming continuity" — and today have no basis for either conclusion.

**Cross-organization claims are unverifiable.** A receiving organization cannot distinguish a legitimate partner agent from any process presenting plausible JSON over a valid organizational TLS connection. Agent-level authentication across a boundary does not exist to be checked.

## Properties without a common definition

- **Subject definition.** What the identifier names, and the continuity rules across update, fork, checkpoint/reconstitution, and migration.
- **Stability.** The identifier survives relocation, multi-homing, process restarts, and multi-day pauses — the identifier/locator split, applied to agents.
- **Instance distinction.** Individual instances are distinguishable among arbitrarily similar siblings.
- **Verifiability.** A counterparty can authenticate that the presenter of an identifier is the agent it names — including across organizational boundaries and through the relays and gateways the transport document warns about. (Impersonation resistance is this property, not a separate feature.)
- **Principal and provenance anchoring.** The identifier is the stable subject to which the acting principal, the deploying party, and the delegation context attach. *(Mechanism owned by the delegation and authorization documents.)*
- **Uniqueness scope and namespace management.** Whether identifiers are organization-scoped or global, and how collisions are prevented when many independent parties mint identifiers for agents that later interact.
- **Lifecycle semantics.** Creation, versioning, retirement that preserves attributed history, and revocation with defined effects on outstanding sessions and past attribution.
- **Message-granularity binding.** The identifier can be cryptographically bound to individual messages, satisfying the transport document's per-message attribution requirement, with the costs (keys, signatures, replay windows) that requirement already acknowledges.

## Prior art, and what it constrains

**SPIFFE/SPIRE** is the strongest parallel and the mandatory point of departure: attested, host-independent workload identity with short-lived credentials. Any agent-identity proposal that cannot state precisely what it needs beyond SPIFFE will be — and should be — collapsed into it. The specific deltas are the five gaps above: subject definition for agents, principal binding, practical cross-domain verification, agent lifecycle semantics, and message-granularity binding. SPIFFE's federation experience is also the cautionary lesson for the cross-org requirement: pairwise trust-domain federation exists and is barely used, because the operational cost lands on every pair.

**Cloud IAM, service accounts, and directory service principals** demonstrate mature intra-organizational software identity — and their signature failure, the shared service account behind which all actors blur, is precisely the attribution collapse described above, now inherited by agents at higher cardinality.

**Host Identity Protocol** proved the identifier/locator separation cleanly at the network layer, and its adoption failure is a lesson about bootstrapping an identity layer that requires everyone to move at once.

**DIDs and Verifiable Credentials** target exactly the cross-boundary verification gap — decentralized identifiers with cryptographic verification and no shared IAM — and their limited adoption outside niche deployments is the second bootstrap warning. DIDComm, already cited in the transport document as proof that per-message attribution envelopes are definable, sits in this family; the identity and transport requirements meet there.

**The web PKI** solved cross-organization identity at the organization/domain level, and demonstrates both the value of a common verification root and the governance weight it carries. It also marks the exact boundary of the solved region: verification stops at the organization, above the acting agent.

**Employee and badge systems** supply the management framework the fleet-inventory case needs — onboarding, role assignment, attribution, separation — and nothing more: they are intra-organizational and non-cryptographic. They support the inventory argument; they cannot carry the interop argument, and this document does not lean the employee analogy further than the management requirements it names. (The framing is deliberately not a claim that agents are employees; it is the observation that when software performs a function whose human performer required an identifier for accountability, the accountability requirement remains when the performer changes.)

The constraint this history imposes: each required property exists somewhere — host-independence in SPIFFE, cross-boundary verification in DIDs/PKI, lifecycle management in IAM, identifier/locator split in HIP. No system provides their combination, and none answers the subject-definition question for agents at all. Any solution must state its deltas against SPIFFE and the PKI explicitly, and must explain why its adoption path escapes the fates of HIP and DIDs.

## The core use case: cross-organization attribution

This use case continues the transport document's trip-booking workflow, at the seam where that document hands off: the travel-service agent belongs to a *partner organization*.

Organization A's assistant agent coordinates with organization B's travel-service agent. B's agent receives messages claiming to come from "A's assistant agent, acting under a delegation from principal P, with authority to book travel up to a limit." For B to act on this safely, B must be able to answer: Which agent of A's is this, specifically? Is it the same agent B coordinated with last week — including after A migrated its infrastructure over the weekend? Is the delegation claim anchored to this agent, or merely asserted alongside it? If the booking is later disputed, can B's records identify the specific acting agent, in terms A will recognize in its own records?

**What breaks today.** B can verify A's organizational TLS certificate and nothing below it. The "assistant agent" is a JSON field. Continuity with last week's counterpart is unverifiable — after A's migration, the instance-level identifiers changed, and B cannot distinguish legitimate continuity from impersonation by another of A's processes, or by anything that has obtained A's connection credentials. The delegation claim floats free of any verifiable subject. In a dispute, B's logs name a connection and a self-declared label; A's logs name an internal service account; the records do not join.

**What any adequate solution must provide.** An agent-level identifier verifiable by B without shared IAM with A; continuity of that identifier across A's infrastructure changes, with B able to detect discontinuity; the identifier as the anchoring subject for the delegation claim; and attribution records on both sides that join on the same identifier.

**What identity alone does not provide, and this document does not claim.** Verifying *which agent this is* establishes nothing about whether the agent is competent, honest, or safe to transact with — that is the trust document's problem, and conflating the two is the oldest identity-system mistake. Nor does identity alone validate the delegation's scope — that is authorization's problem. Identity is the subject line, not the sentence.

**Open questions this use case exposes.** The subject-definition question (what, exactly, must remain "the same agent" across A's migration for B's continuity check to be meaningful) and the issuance model (who vouches for the binding between identifier and agent — A alone? A under an auditable scheme? a third party?) are unresolved and belong to the solution space. The failure modes above stand regardless of how they are resolved.

## Supporting use case: fleet inventory and lifecycle at scale

An organization deploys a thousand agents across customer service, procurement, security monitoring, and research. Some spawn sub-agents; some are updated weekly; some run for months across infrastructure that is replaced under them; some must be retired with their history preserved for compliance.

The operator needs to answer, continuously: which agents exist; who deployed each, and under what ownership; what is each supposed to do; which agent performed a specific action; which are still active, and which are orphans running under stale credentials. Host names, process IDs, container instance IDs, and IP addresses all change with infrastructure and cannot anchor these answers; shared service accounts erase the per-agent resolution the questions require.

This case is deliberately demoted from "core": it is intra-organizational, and an organization could in principle satisfy it with an internal scheme and a database — it motivates *an* identifier, not a *common* one. Its force in this problem statement is threefold: the scale that makes improvisation fail is already present; the internal schemes organizations are improvising are exactly the incompatible per-stack definitions the cross-organization case then inherits; and the lifecycle requirements it surfaces (retirement with history, revocation, orphan detection) constrain the same primitive the cross-org case needs. The SKU analogy applies to this case and only this case: a SKU is an unverified label, sufficient for internal inventory, and categorically insufficient for the cross-organization case, where verifiability is the point.

## Additional identity needs

Stated as observable needs or present-day failures; items whose primary ownership lies elsewhere in the series are tagged.

**Stability and continuity**
- An agent is live-migrated or reconstituted on new infrastructure mid-task; outstanding sessions, grants, and audit records must continue to refer to the same agent without manual re-binding, and counterparties must be able to verify the continuity.
- A long-lived agent's containers, VMs, and cloud accounts are replaced over months while its logical identity and attributed history remain continuous.
- An agent pauses for days (the transport document's session-pause pattern) and resumes with its identity intact on both sides.

**Distinction and containment**
- Distinguish which of several near-identical concurrent instances performed a given action.
- Revoke or quarantine one specific agent without affecting siblings sharing its image, host, and credentials.

**Cross-organization**
- Recognize a previously encountered agent as the same agent after the counterpart organization's infrastructure changes — or detect that it is not.
- Attribute a received message to a specific agent of a specific organization, verifiably, with no shared identity infrastructure.

**Namespace and issuance**
- Prevent and resolve identifier collisions when many independent parties mint agent identifiers that later interact.
- Define uniqueness scope (organizational vs. global) and the resolution path from identifier to verification material.

**Lifecycle**
- Retire an agent while preserving the integrity and queryability of its attributed history.
- Revoke an agent's identity with defined semantics for its outstanding sessions and for the standing of its past attributions.
- Detect orphaned agents whose owning principal or project no longer exists.

Items moved out of this document, following its scoping: *enforce the agent's declared role* → authorization (roles attach to the identifier; enforcement is an authorization function). *Verify which organization created an agent* → shared with trust (provenance attaches to the identifier; its evidentiary weight is a trust question). *Enable one agent to introduce another* → discovery. *Prevent impersonation* remains, restated above as the verifiability property of the identifier itself rather than a separate feature.

## What this document does not claim

- **Not** that any particular issuance or trust architecture is required — not self-sovereign identity, not blockchain anchoring, not a single global root. The claim is the requirement set, not an architecture.
- **Not** that workload identity is inadequate for its domain. SPIFFE-class identity is the presumptive substrate for agent identity *within* a trust domain; the claim is that the five gaps above are real and unmet on top of it.
- **Not** that identity establishes trust. An authenticated identifier makes an agent *accountable*, not *trustworthy*; the distinction is load-bearing for the trust document.
- **Not** that the requirements are individually novel. Each exists in prior art; the combination — agent-appropriate subject definition, principal anchoring, cross-organization verifiability, agent lifecycle semantics, message-granularity binding — does not.

The claim is: agent identity currently has no common definition of subject, continuity, verification, or lifecycle; each stack and organization improvises one; the improvisations do not compose across stacks or organizational boundaries; and the resulting failures — attribution collapse, orphaned agents, sibling indistinguishability, unverifiable cross-organization claims, broken references after reconstitution — are observable today and grow with fleet size and cross-organization coordination.

## Forward and backward dependencies

- **Transport (01), backward:** this document accepts the seam handed to it — per-message attribution and hop-surviving lineage are requirements on the identity primitive (message-granularity binding, relay-surviving verifiability). Any token-based identity proposal must answer the hop-dissociation failure transport describes.
- **Authorization (03):** receives the identifier as the subject of grants, and receives the *role enforcement* concern moved out of this document. Assumes instance distinction (a grant to one sibling, not all).
- **Delegation:** receives the principal-anchoring requirement; defines the mechanism by which delegation attaches to and travels with the identifier.
- **Trust:** receives the provenance concern; assumes the accountable/trustworthy distinction drawn here.
- **Audit:** assumes attribution records join on a common identifier across stacks and organizations — the identity-layer half of transport's audit-fragmentation failure.
- **Discovery:** receives the introduction concern; assumes identifier-to-verification-material resolution exists in some form.

---

[← Transport](01-transport.md) &nbsp;·&nbsp; [README](README.md) &nbsp;·&nbsp; [Authorization →](03-authorization.md)
