# 3. Authorization (AZN)

## Scope, status, and terminology

This document is a problem statement, not a solution proposal. It argues that agent workflows create authorization requirements — verifiable causal lineage across organizational boundaries, effect-time validity, quantitative bounds that survive multi-hop chains, and resistance to a threat model in which the authorized actor itself can be manipulated — that no deployed system meets in combination, and that the absence of a common model produces observable failures today. It stops short of prescribing a token format, chain encoding, or enforcement architecture.

**Boundary with Delegation (04).** This document and the next divide one territory, and the split is stated here so both can be checked against it. *Authorization* owns the receiver's decision at the point of action: is this actor authorized for this specific action, under what constraints and quantitative bounds, on what provenance, is the authority valid at the moment of effect, and can the check be performed locally. *Delegation* owns the transfer mechanism: how authority is created, attenuated, bound to principals, and moved hop to hop — including the construction of the chains this document requires receivers to verify. Chain *verification* is authorization; chain *formation* is delegation. If document 04 is scoped differently, this seam must be renegotiated explicitly rather than left overlapping.

Two term distinctions carry weight below. A **bearer token** is valid in isolation and says nothing about its ancestry; a **capability chain** is a verifiable attenuation of a specific parent authority. **Issuance-time validity** (the authority was good when granted) and **effect-time validity** (the authority is still good at the moment an irreversible effect occurs) are different properties, and the gap between them is where several of the failures below live.

## The skeptical case

The internet has deep authorization infrastructure, and the strong skeptic knows all of it. OAuth 2.0 handles user-to-service authorization at massive scale, and OAuth token exchange (RFC 8693) explicitly supports delegation and impersonation patterns within it. Enterprise IAM provides role- and attribute-based control; SPIFFE anchors workload authorization; Cedar, OPA, and Zanzibar evaluate rich policy at scale.

More pointedly, the specific property this series might claim as novel — chained, attenuable authority verifiable offline at the receiver — has been built repeatedly. SPKI/SDSI defined authorization certificate chains with attenuation in the 1990s. X.509 proxy certificates (RFC 3820) carried multi-hop, multi-organization delegation chains in production grid-computing deployments two decades ago. Macaroons provide caveat-based attenuation verifiable without callbacks; Biscuit tokens embed attenuable authority with an evaluable policy language; UCAN and ZCAP-LD define delegation chains aimed at exactly the decentralized, cross-party case agents present. Beneath all of it sits the older pattern: organizations already install, permission, and monitor software through OS permission models, code signing, and endpoint detection, and already govern employee authority through IAM and approval workflows.

The skeptic's conclusion: compose what exists. RFC 8693 for delegation across services, Biscuit or Macaroons where offline attenuation matters, Cedar or OPA for policy, existing IAM at each organization's edge. Agents are software actors; the toolbox is full; no new primitive is required.

At small scale — a single agent, single owner, single organization, presenting a single token to a receiver that trusts the issuer — this case simply holds, and this document does not contest it.

## Where the mismatch actually lies

The mismatch is not that chained verifiable authority is unknown. It is that (a) the known constructions have never been commonly adopted, so real deployments run on bearer tokens and standing credentials that sever lineage at every hop, and (b) even the known constructions do not address the requirements that are specifically agent-shaped. Both halves matter, and they are different kinds of problem.

**The adoption half.** Every hop in today's multi-stack agent workflows presents a freshly issued token, locally valid, causally disconnected from its parent authority. Existing IAM was designed for a single organization's actors; receivers across a boundary can verify the immediate issuer or call back to an authority, and neither scales to multi-hop chains at machine speed. The causal chain — who originally authorized this, under what constraints — exists nowhere on the wire. This is the same finding as the transport and identity documents: the primitive has been defined many times, incompatibly or not at all in deployed practice, and no common definition spans stacks and organizations.

**The agent-shaped half.** Four requirements fall outside what even the capability literature delivers:

- **Effect-time validity at machine speed.** Agent chains execute in seconds and produce irreversible effects (payments, commitments, state changes). The gap between issuance-time and effect-time — during which a parent authority may have been revoked or a budget exhausted — is where the money moves.
- **Quantitative, consumable bounds across hops.** Budgets, rate limits, and quantity caps are not static predicates; they are shared mutable state consumed across multiple vendors and chain branches. No offline-verifiable credential can enforce them alone (developed below).
- **Action-instance binding.** The same evidence must not authorize a replay of the same action or a different action, including under concurrent attempts from parallel chain branches.
- **A confused-deputy threat model.** Traditional software executes fixed logic under its permissions. An agent's control flow is influenced at runtime by content it processes, so an agent holding a perfectly valid credential can be manipulated — prompt injection being the current dominant vector — into exercising legitimate authority for an attacker's purpose. Token verification cannot catch this: the token is genuine and the action is in scope. This is the sharpest delta between agent authorization and everything the skeptic listed, and it is the strongest standing argument for narrow, task-scoped, attenuated authority over broad ambient permission.

One correction to a claim made in earlier drafts: it is not true that human authority never chains across organizations. Corporate travel booked through managed travel services does exactly this today — an employee's authority reaches a vendor's systems and onward to hotels and airlines. The existing solution is contractual scoping plus after-the-fact reconciliation (expense reports, statements, disputes) at human speed and human frequency. Agent chains break that solution on every axis it depends on: hop depth, transaction frequency, decision latency, and the absence of a human filing the reconstruction. The problem is not that the pattern is new; it is that its existing resolution mechanism does not survive the operating regime.

## What observably breaks today

**Over-provisioning as standard practice.** Because fine-grained, per-task authority is operationally expensive to mint and manage, real deployments grant agents broad standing credentials — long-lived API keys, wide IAM roles, org-level OAuth grants. Fleet-wide, this makes every agent a high-value target and makes the blast radius of any single compromise the full width of the standing grant.

**Confused deputies with valid credentials.** Injection attacks against deployed agents cause actions that are fully authorized in the credential sense and entirely unintended in the authority sense. Post-incident, the audit question "was this action authorized" answers *yes* while the question that mattered — "was this action within the authority the principal meant to grant for this task" — was never representable.

**Severed chains and manual reconstruction.** When authority crosses an organizational boundary, the receiving side holds a token from the immediately preceding hop and nothing else. Reconstructing the chain for audit or dispute means email, phone calls, and days of cross-vendor reconciliation — the core use case below walks through it.

**Revocation that outruns nothing.** A mid-chain revocation propagates through each hop's independent token model at each hop's pace, or not at all; later hops act on authority that no longer exists upstream. This is the authorization-layer face of the transport document's non-uniform-revocation failure, and today it has no defined semantics anywhere: nothing specifies what revoking a mid-chain grant does to sub-delegations already issued from it, or with what timing guarantee.

**Cumulative bounds enforced nowhere.** A cross-vendor budget cap is enforced, at best, at the original issuer at issuance time. No downstream receiver can know what the rest of the chain has already consumed; concurrent branches can each be individually within bounds while jointly exceeding them.

**Replay and double-spend ambiguity.** Whether a presented authority has already been consumed — by a retry, a concurrent branch, or an adversary replaying captured evidence — is undefined across stacks, and each receiver improvises idempotency locally if at all.

## Properties without a common definition

Framed as the receiver's questions at the point of action:

- **Lineage: who authorized this, through what chain, from what root?** Requires chain carriage and local verifiability across organizational boundaries — receiving the identity document's cross-org verifiability as a precondition, and the transport document's per-message attribution as its carrier.
- **Scope: is this specific action within the granted authority?** Requires attenuation to be *verifiable* by the receiver (its construction belongs to delegation), and requires the grant's subject to be a specific agent instance — the identity document's instance-distinction property — not a shared credential.
- **Bounds: do quantitative constraints still hold?** Requires defined semantics for consumable authority. An honest statement of the constraint: offline verifiability, cumulative cross-party bounds, and absence of shared state cannot all be had at once. The available resolutions — pre-partitioned budget allocations carried in the chain, an online check against a designated authority with defined latency, or post-hoc detection with a defined exposure window — have different failure profiles, and today *none of them is even specifiable*, because no common model names the choice.
- **Currency: is the authority valid now, at effect time?** Requires revocation-propagation semantics along chains. The same honesty applies: offline verification and instant revocation are the two horns of the oldest dilemma in credential systems (CRLs, OCSP, and short-lived tokens are all points on its tradeoff curve). The problem is not that the dilemma exists; it is that agent chains currently sit at no defined point on the curve.
- **Uniqueness: has this authority already been consumed, and is it bound to this action instance?** Requires instance binding and defined idempotency/consumption semantics across chain branches.

## Prior art, and what it constrains

**SPKI/SDSI** proved authorization chains with attenuation three decades ago and never achieved deployment — the first bootstrap warning.

**X.509 proxy certificates and grid computing** are the load-bearing precedent: real multi-organization delegation chains, deployed at scale, for autonomous computational work — the closest historical system to agent chains. The grid experience is also the cautionary tale: proxy-certificate management proved operationally miserable (short lifetimes fought long jobs, renewal and revocation were chronic pain, middleware complexity metastasized), and the ecosystem largely retreated to simpler federated tokens. Any proposal here must explain specifically why it does not replay this history.

**Macaroons** demonstrated offline attenuation via caveats, including third-party caveats as a principled hybrid of offline structure and online checks — directly relevant to the bounds and currency questions above — and remained largely confined to single-service deployments. **Biscuit** adds an embedded, evaluable policy language to offline attenuation and is young. **UCAN and ZCAP-LD** target the decentralized cross-party case most directly, and their adoption is niche; UCAN's dependence on DID-family identity ties its bootstrap problem to the one the identity document already catalogues.

**OAuth token exchange (RFC 8693)** handles delegation *within* an issuer-trust topology: lineage exists as issuer-side knowledge, not as receiver-verifiable chain structure. It is the strongest evidence for the skeptic's composition argument and the clearest illustration of its limit.

**Cedar, OPA, and Zanzibar** evaluate policy over facts presented to them; they neither carry facts across organizational boundaries nor verify lineage. They are complements to whatever solves this problem, not competitors — worth stating because "use OPA" is a predictable review response.

**Purchase-order and approval-chain systems** are the institutional model the core use case invokes: every charge walks back to a parent approval, at human speed, inside one organization's records. The unmet need is this property at machine speed across organizational boundaries.

The constraint the history imposes: lineage, attenuation, and offline verification have each been solved several times over. What no system provides is their combination with quantitative effect-time constraints, action-instance binding, and an injection-aware threat model — under an adoption story that escapes the fates of SPKI, proxy certificates, and UCAN. A proposal that cannot state its delta against Biscuit and UCAN in one paragraph should be presumed to be one of them.

## The core use case: bounded authority with a paper trail across organizations

An organization grants an assistant agent authority to spend up to $500 per month on travel for a specific employee. The best package requires booking through a travel service operated by a second company, which reserves a room from a hotel chain (a third) and a flight from an airline (a fourth). The trip proceeds. A month later, the employee's finance team reconciles the corporate card and sees a hotel charge for $237. Their job: verify the charge was authorized, connect it to the delegation that authorized it, and confirm the monthly total remains within the cap.

**What breaks today.** The statement shows a merchant and an amount. Nothing connects the charge to the assistant, the assistant to the delegation, or the delegation to the $500 ceiling. The hotel's records show a valid token from the travel service, scoped to reserving rooms — carrying nothing about whose budget it draws from or what parent authority scoped it. Reconstruction means requesting the booking record from the travel service, checking the assistant's grant in the internal agent-management system, and aggregating that month's charges across every vendor the assistant touched. Each hop's token was valid in isolation; the chain between them exists in no system. One question becomes three phone calls and several days.

**The two problems in this scenario are different problems, and conflating them is the classic error.** *After-the-fact reconstruction* — the finance team's backward walk from charge to root authority — is solved by verifiable chain lineage: if each hop's authority is a recorded, verifiable attenuation of its parent, reconciliation is a query against records that already join. This is the tractable half, and the capability literature shows the construction. *At-decision-time enforcement of the cumulative cap* is not solvable by any offline-verifiable credential, because the cap is shared mutable state: the hotel cannot learn from any token what the airline charged this month, and concurrent bookings can each be individually compliant while jointly over budget. Any honest model must choose where cap enforcement lives — partitioned allocations minted into the chain, an online authority consulted with defined latency, or post-hoc detection with a defined exposure window — and make the choice explicit rather than implying the credential alone enforces it.

**What any adequate solution must provide.** Receiver-local verification of the full chain from charge back to root grant; the grant bound to a specific agent identity (not a shared credential); action-instance binding so the same authority cannot fund a second reservation; defined revocation and expiry semantics along the chain; and an explicit, named enforcement point for the quantitative cap with its exposure characteristics stated.

**What no solution can promise, and this document does not claim.** Simultaneous offline verification, instant chain-wide revocation, and credential-enforced cumulative bounds. The achievable requirement is *defined position* on each tradeoff — so that when the finance team, or an incident responder, asks what guarantee held at the moment of charge, the answer is a property of the model rather than an archaeology project.

**What lineage does not establish.** A verifiable chain proves who authorized the action and through what attenuations. It does not prove the root principal is creditworthy, the intermediary competent, or the request wise — the accountable/trustworthy distinction the identity document draws applies verbatim, and the trustworthiness question routes to the trust document.

## Additional authorization needs

Stated as the receiver's problems at the point of action; construction-side counterparts belong to delegation (04).

**Verification**
- Verify the full chain of authority behind a request locally, without callbacks to any prior hop's organization — including under partial connectivity or latency budgets that preclude synchronous checks.
- Verify that a presented authority's scope covers this specific action, where the scope is an attenuation of a parent authority granted in a different organization's system.
- Verify that the acting agent is the grant's subject — a specific instance, not any holder of a shared credential. *(Depends on identity's instance distinction.)*

**Quantitative bounds**
- Determine whether a request is within a budget that other vendors and concurrent chain branches are simultaneously consuming, under an explicitly defined enforcement model.
- When two attenuated authorities derived from the same parent arrive at one receiver, determine their relationship and the remaining joint capacity.

**Validity over time**
- Determine that authority valid at issuance is still valid at effect time — that no upstream revocation or exhaustion has occurred within the model's defined exposure window.
- Detect mid-chain revocation from the receiving end while later hops are in flight, without synchronous callbacks to every prior hop. *(Propagation mechanism shared with transport's control-propagation requirement; revocation semantics along the chain shared with delegation.)*

**Instance and consumption**
- Bind an authorization to a single action instance such that replay — accidental retry or adversarial reuse — is detectable at the receiver.
- Define consumption semantics for single-use and countable authorities across concurrent branches.

Items moved to delegation (04) under the scope split: *delegate authority for a specific type of action*, *delegate a narrower authority than the delegator holds*, and the construction side of *revoke an agent's authority* (the receiver-side detection of revocation remains above).

## What this document does not claim

- **Not** that chained, attenuable, offline-verifiable authority is novel. It has been designed repeatedly and deployed at least once at scale; the prior-art section is the constraint, not a garnish.
- **Not** that offline verification, cumulative bounds, and instant revocation can be jointly achieved. The claim is that agent authorization currently sits at *no defined point* on these tradeoff curves, not that the curves can be escaped.
- **Not** that a verifiable chain implies a trustworthy root, or that lineage substitutes for the trust document's problem.
- **Not** any particular token format, chain encoding, or policy language.
- **Not** that OAuth, IAM, SPIFFE, or policy engines are displaced. They are components and boundary conditions of any plausible solution; the missing thing is the common cross-organization chain model they do not provide.

The claim is: authorization for agent chains — receiver-local lineage verification, effect-time validity, quantitative consumable bounds, action-instance binding, under a confused-deputy threat model — has no common definition across stacks or organizations; deployed practice runs on bearer tokens and over-provisioned standing credentials that sever lineage and widen blast radius; and the resulting failures — manual cross-org reconstruction, undefined revocation exposure, unenforced cumulative caps, injection-driven misuse of valid credentials — are observable today and scale with chain depth, transaction frequency, and fleet size.

## Forward and backward dependencies

- **Transport (01), backward:** the non-uniform-revocation failure mode lands here as the effect-time validity problem; per-message attribution is the carrier on which chain evidence rides; control-message propagation is the mechanism revocation notices would need.
- **Identity (02), backward:** the identifier is the subject of every grant; instance distinction makes grants attach to one sibling rather than all; role enforcement, moved out of identity, lands here as scope verification; cross-org verifiability of the identifier is a precondition for cross-org lineage.
- **Delegation (04), forward:** receives chain construction, attenuation mechanics, principal binding (routed from identity), and the grant-side semantics of revocation, under the scope split declared above — to be verified against that document.
- **Trust, forward:** receives the root-worthiness question — whether a verifiably authorized chain is *worth transacting with*.
- **Audit, forward:** the chain records this document requires are the audit document's raw material; the finance-reconciliation query is jointly owned.
- **Discovery, forward:** capability claims advertised at discovery time are unverified assertions until checked against the lineage model described here; the pairing between discovery claims and authorization verification (asserted in passing in earlier drafts) belongs to that document's seam list.

---

[← Identity](02-identity.md) &nbsp;·&nbsp; [README](README.md) &nbsp;·&nbsp; [Delegation →](04-delegation.md)
