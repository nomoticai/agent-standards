# 6. Discovery (DSC)

## Scope, status, and terminology

This document is a problem statement, not a solution proposal. It argues that the dominant operational patterns of internet discovery — name-centric, publishing-centric, domain-anchored, static-endpoint — leave the discovery patterns agents most need poorly served, and that this gap is observable in how agents are actually found today. It stops short of prescribing a registry architecture, an index model, or a metadata format.

Two terminology notes. First, following RFC 6763's narrower usage: *search* here means an intentional query by a requester; *discover* is broader, including structural detection — one agent becoming aware of another through composition, delegation, referral, or presence, with no query issued. This document uses the broad sense, with one flag: **presence** is a provisional primitive elsewhere in this suite, so structural discovery via presence is noted where relevant but not developed here. Second, discovery in this suite sits between nothing and everything: it produces *candidates and claims*; the trust document owns evaluating them, and authorization owns acting on them. Claims surfaced at discovery time are unverified assertions until those layers do their work — a seam declared in documents 03 and 05 and accepted here.

This document also receives two relocated concerns: transport's *ambient availability advertisement* (surfacing candidates without active queries) and identity's *introduction* (one agent making another discoverable to a third), both folded into the structural-discovery material below. Identity's requirement that an identifier resolve to current verification material lands here as identity-first resolution.

## The skeptical case

The internet has one dominant discovery mechanism, and it works. DNS is universally deployed, and its extensions carry far more than name-to-address: SVCB/HTTPS records for service binding, TXT records and well-known URIs for structured metadata, DNS-SD for browsing services by capability type (deployed on billions of devices), DNSSEC and DANE for integrity and trust anchoring, NAPTR/DDDS for rule-based resolution. Where DNS ends, deployed alternatives begin: search engines answer "I have a need, not a name" for the entire web; marketplaces and app stores provide curated capability catalogs with review gates; LDAP directories filter on attributes at enterprise scale; service registries (Consul, Kubernetes) resolve dynamic endpoints continuously inside organizations. The current agent ecosystem already has working conventions on this foundation — curated MCP registries, A2A agent cards served from well-known URIs under a domain.

The skeptic's conclusion: publish agent metadata under well-known paths, extend DNS-SD service types for agent capabilities, let registries and search fill the rest. The pattern is proven; nothing new required.

## Where the mismatch actually lies

The mismatch is not that DNS "cannot" carry discovery data — its extensions demonstrably can, and the claim that DNS is incapable is both wrong and unnecessary. The defensible claim is about the *dominant patterns and their assumptions*, which leave three question shapes poorly served:

**Capability-based lookup.** "Given a task and constraints, find agents that can do it." The requester has a need, not a name — and DNS's entry point is a name. DNS-SD browses capability *types*, but within a domain the requester must already know; the no-starting-domain case (find legal-translation agents *anywhere*) has no DNS entry point at all. The deployed answer is web search, which imports the web's adversarial ranking dynamics (developed below).

**Attribute-based filtering.** "Within that capability, filter by region, specialization, price, availability, evidentiary standing." Multi-attribute directory query is LDAP's native model and a bolted-on afterthought in DNS-first patterns; today it happens after discovery, manually, per candidate.

**Identity-first resolution.** "Given a stable agent identifier that is not a domain name, find the current endpoint and verification material — including after the agent changed hosting or organizations." This is the identifier/locator split of the identity document (HIP's territory) surfacing as a resolution problem. Under domain-anchored patterns, the domain *is* the identity: an agent that moves domains is, to every receiver, a new entity, and its history — the trust document's evidence, the identity document's continuity — severs at the move. ENUM demonstrated the shape of non-domain-identifier resolution over DNS and also demonstrated its public-deployment failure.

Behind the three questions sits one structural issue the earlier drafts framed as a paradigm inversion and stated three times; once is enough, stated honestly. DNS-first discovery is a *publishing* model: nothing is visible until a domain owner configures records, and what is visible is what was published, under the publisher's domain. Earlier drafts opposed this to an "intrinsic metadata" model in which agents carry their descriptors and become discoverable automatically — but the filesystem and music-library images that motivated it describe *local, single-administrator* indexes scanning objects they already hold. There is no global filesystem. At ecosystem scale, metadata "traveling with the agent" does not eliminate the index; it relocates the unresolved questions: **who operates the index, who controls inclusion, who bears the freshness cost, and how the index's claims are verified.** Every known architecture answers these differently and pays differently: centralized registries concentrate control and become chokepoints (and marketplaces demonstrate both the value and the capture); federation fragments query reach (the XMPP lesson from document 01); DHTs achieve identity-first resolution at internet scale and inherit the sybil and eclipse attack literature; crawling builds the index passively and inherits the SEO arms race. This is the transport document's operator-model question recurring at the discovery layer, and this problem statement requires a defined position on it from any solution — not an escape from it, because none exists.

The coupling with identity, retained from earlier drafts because it is correct: a discovery result that is only an endpoint forces a second lookup to establish who was found; every non-trivial filter (organization, prior relationship, evidentiary standing) anchors in identity; and both directions of change — endpoint moves under a stable identity, identity changes behind a stable endpoint — are only detectable if identity is an input to the discovery response rather than an afterthought. Discovery and identity are separable primitives with a functional dependency, and the integration surface between them is where the composition fails today.

## What observably breaks today

**Finding agents is web search plus word of mouth.** The general-purpose answer to "I need an agent that can do X" is a search engine — returning SEO-shaped results in which visibility correlates with marketing investment, not capability, and in which comparison requires visiting each candidate and manually normalizing incompatible self-descriptions.

**The current agent stacks are domain-anchored publishing, which is the limitation in deployed form.** An A2A agent card lives at a well-known URI under a domain: the requester must already know the domain, the domain owner must have published, and the identity is the domain's. MCP server discovery runs through curated lists and registries: visible if listed, invisible otherwise, with listing criteria set per registry. Both work — and both instantiate exactly the assumptions whose limits this document describes.

**No cross-organization capability query exists.** Each marketplace, registry, and platform is a silo with its own schema; "all agents anywhere matching these criteria" is not a queryable question against anything deployed.

**Moves sever history.** An agent that changes hosting organization is, to prior counterparties resolving by domain, gone — or worse, silently replaced. The identity document's reconstitution failure and the trust document's evidence-portability failure both surface at the discovery layer as: the lookup that should have found *the same agent* found nothing, or found a stranger at the old address.

**Relay-mediated agents are invisible.** Agents reachable only through relays, gateways, or rendezvous points — the transport document's composition patterns — have no endpoint to publish, and endpoint-centric resolution has no way to return them.

**Claims at discovery time are free and indistinguishable.** Capability claims in results are self-asserted; established-brand impersonation, capability stuffing, and manufactured standing render identically to legitimate entries. The trust document's decision-time failure begins here, at result time — and any *open* index inherits the email-spam dynamic: open registration plus free claims equals a permanent adversarial arms race. Earlier drafts offered the spam filter as the reassuring analogy ("the trust score filters most of it"); the analogy is kept, inverted — four decades of escalating adversarial investment is the *forecast*, not the fix, and the score model it leaned on was retired in document 05. What filtering can consume is that document's evidence layer, under each relying party's policy; what it cannot promise is a solved arms race.

**Freshness is undefined.** Capability, pricing, and availability change on agent timescales; published records go stale with no staleness bound, and no deployed pattern defines who pays to keep the index current.

## Properties without a common definition

Framed as the questions a requester must be able to get answered, not as features of a registry:

- **Need-to-candidates:** from a capability need plus constraints — no name, no starting domain — obtain candidates across organizational boundaries, under a defined index model whose operator, inclusion rules, and coverage are inspectable.
- **Attribute filtering at query time:** apply multi-dimensional constraints (specialization, region, price, availability, evidentiary standing per document 05) before committing attention, not after.
- **Identity-first resolution:** from a stable agent identifier, obtain current endpoints and verification material, across infrastructure and organizational change, with staleness detectable in both directions (moved agent; replaced endpoint). *(Joint with identity's continuity and resolution requirements.)*
- **Relay-mediated reachability:** obtain a way to reach agents that have no direct endpoint, without the index leaking what the relay exists to protect.
- **Claim provenance in results:** distinguish, at result time, attested claims from self-asserted ones — consuming the trust document's evidence layer rather than reinventing it.
- **Freshness bounds:** know the age of what the index returned, with change-propagation responsibilities defined.
- **Structural discovery:** become aware of candidates through composition, referral, introduction, and (provisionally) presence — with such encounters carrying the same identity and provenance properties as queried ones, so that ambient awareness is not an unverified side channel. *(Receives transport's ambient-availability and identity's introduction concerns.)*
- **Two-phase coordination:** coarse capability-class resolution producing candidates, then bilateral entity-level negotiation of detailed capabilities and parameters — distinct phases with distinct properties, today conflated or unserved. *(Negotiation itself belongs to the communication document.)*

## Prior art, and what it constrains

**UDDI** is the headline and the obligatory confrontation: a universal registry for capability-based service discovery — taxonomy-indexed, query-filtered, backed by IBM, Microsoft, and SAP — built for the web-services ecosystem, and dead; the public registries shut down in 2006 without ever carrying meaningful traffic. It is the closest historical artifact to what this document's earlier drafts implied, and any solution here must state specifically why it does not replay UDDI: what changed about the demand, the operator model, or the claim-verification story.

**DNS-SD/mDNS** proves capability-type browsing at massive deployment — scoped to local networks and known domains, which is precisely the scope the agent cases exceed. **ENUM/NAPTR** proves non-domain-identifier resolution over DNS is buildable, and its public failure is the adoption warning for identity-first resolution schemes. **LDAP and X.500** prove attribute-filtered directory query at enterprise scale, and X.500's global-directory ambition is the standing warning about universal directories. **DHTs** (Kademlia-family, content-addressed networks) prove identity-first lookup at internet scale with no central operator — and carry the sybil and eclipse attack literature as the cost of openness, binding this document's index integrity to the identity document's issuance-cost question exactly as document 05's contamination bounds were bound. **Tor rendezvous** proves relay-mediated discovery deployably. **Service registries** (Consul, Kubernetes, Eureka) prove continuous dynamic-endpoint resolution with health and metadata — inside one operator's boundary. **Search engines** are the deployed answer to no-starting-point discovery, and the SEO economy is the deployed demonstration of what open, free-to-claim indexes become under adversarial pressure. **App stores and marketplaces** prove curated capability discovery with claim gates, and demonstrate the capture, rent, and chokepoint costs of the centralized answer to the operator question. **MCP registries and A2A agent cards** are the ecosystem's current conventions and the most direct evidence for this document: working, adopted, and structurally bound to curation-or-domain patterns.

The constraint the history imposes: every property above exists somewhere — capability browsing, attribute filtering, identity-first resolution, relay mediation, dynamic freshness, curation. No system provides their combination across organizational boundaries under an operator model that survived contact with either adoption (UDDI, ENUM, X.500) or adversaries (open indexes). The unmet need is the combination; the graveyard is the proof of difficulty, not a reason the need is unreal.

## The core use case: finding capable agents in an open market

An individual needs a legal document translated from Japanese to English with jurisdiction-specific terminology. They have a clear need and clear criteria — language pair, legal specialization, price ceiling, delivery timeline — and nothing else: no agent in mind, no starting domain, no prior relationship in this domain.

**What breaks today.** The path is web search. Results are ranked by the SEO economy, not the criteria; each candidate is a website with self-described capabilities in its own vocabulary; comparison means visiting each in turn while holding the criteria in memory; trust signals differ per site (one shows platform reviews, another certification badges, a third neither) with no way to distinguish attested from asserted; multi-attribute filtering across organizations — the actual question — is not expressible against anything. The individual commits attention *before* filtering, which is backwards, and the commitment is the cost the adversarial candidates are farming.

The evaluation the individual needs to perform is familiar and multi-dimensional — the restaurant problem: 500 reviews averaging 3 stars is different evidence than 10 reviews averaging 5; price interacts with both; no single axis decides. Humans run this evaluation instinctively when the dimensions are presented side by side. Nothing presents agent candidates side by side across organizations, in comparable terms, with evidence provenance attached.

The corporate analog is B2B procurement: a capability need plus constraints, queried against directories, associations, and RFP networks, producing qualified candidates evaluated on evidence before any relationship exists. That machinery operates at human cadence with human diligence; the agent case needs the same shape at query speed — which is precisely what makes its adversarial exposure worse, since the human diligence that catches fraud in procurement is the step being removed.

**What any adequate answer must make possible.** Query by need and constraints with no starting name or domain; results spanning organizational boundaries, identified by stable agent identifiers rather than domains; per-candidate metadata in comparable form with claim provenance distinguishable (attested vs. asserted, per document 05); filtering on the requester's criteria — including evidentiary standing under the requester's own policy — before attention is committed; and continuity, so that a candidate selected today remains resolvable as the same agent after it changes infrastructure.

**What no answer can promise, and this document does not claim.** That the index escapes the operator question — centralized, federated, distributed, and crawled models all pay known costs, and a solution owes a position, not a transcendence. That adversarial pressure is filtered away — open claims guarantee an arms race; the achievable property is that evidence and provenance are present at result time so filtering has something to consume. That ranking is neutral — any ordering embeds a policy, and the achievable property is that the policy be inspectable and overridable by requester criteria rather than silently imposed. Yellow Pages against White Pages remains the right image for the *need* — capability lookup alongside name lookup, complements not replacements — while UDDI remains the right image for the *difficulty*.

**Open questions this use case exposes.** The index operator model and its inclusion economics; who verifies capability claims at ingestion versus at query versus never; and how discovery-time evidence consumption composes with document 05's relying-party policies — all solution-space; the failures stand regardless.

## Additional discovery needs

Stated as observable needs or failures; items owned elsewhere are tagged.

**Lookup patterns**
- Resolve a stable agent identifier to current endpoints and verification material after infrastructure or organizational change. *(Joint with identity.)*
- Find candidates for a capability need across organizational boundaries with no starting name or domain.
- Filter candidates on multiple attributes — specialization, region, price, availability, certifications, evidentiary standing — at query time, in one place, rather than per-candidate afterwards.
- Look up an agent by name or domain where those are known — the solved case, which must remain served alongside the unsolved ones.
- Find tools by capability, and agents by the tools they can invoke.

**Reachability**
- Surface agents reachable only through relays or rendezvous points, without exposing what the mediation protects.
- Detect that a previously resolved endpoint no longer belongs to the previously resolved agent (replacement staleness), and that a previously resolved agent has moved (relocation staleness).

**Result integrity**
- Distinguish attested from self-asserted claims at result time. *(Consumes document 05's evidence layer; jointly owned.)*
- Bound the influence of manufactured entries — capability stuffing, brand impersonation, sybil flooding — on result composition, within the limits the sybil literature imposes. *(Bound to identity's issuance-cost question.)*
- Know the freshness of returned metadata, with change-propagation responsibility defined.

**Structural discovery**
- Become aware of candidates through composition and referral without an active query, with identity and provenance intact on the encounter. *(Receives transport's ambient-availability concern.)*
- Be introduced: one agent makes a second discoverable to a third, with the introduction's provenance carried. *(Receives identity's introduction concern.)*
- Encounter candidates through presence overlays. *(Provisional; developed if presence is retained in the suite.)*

**Two-phase coordination**
- Resolve a capability class to candidates (coarse), then negotiate detailed capabilities bilaterally with a selected candidate (fine) — with the boundary between the phases defined. *(Negotiation mechanics belong to communication.)*

## What this document does not claim

- **Not** that DNS is incapable of carrying discovery data. Its extensions carry a great deal; the claim is about dominant operational patterns — name-centric, publishing-centric, domain-anchored, static-endpoint — and the question shapes those patterns leave poorly served.
- **Not** an "intrinsic metadata" architecture. Earlier drafts opposed publishing to intrinsic metadata; the opposition does not survive the operator question, which no architecture escapes and every solution must answer explicitly.
- **Not** a registry design — centralized, federated, distributed, or crawled — nor a metadata schema, nor a ranking policy.
- **Not** that adversarial pressure on open indexes can be filtered away, that ranking can be neutral, or that sybil resistance is available over free identities. The email-spam history is the forecast; the sybil floor from document 05 applies; the achievable claims are evidence-at-result-time and inspectable policy.
- **Not** that discovery evaluates or authorizes. It produces candidates and claims; documents 05 and 03 own what happens to them.

The claim is: the discovery patterns agents most need — need-based candidate finding across organizations, query-time multi-attribute filtering with claim provenance, identity-first resolution that survives moves, relay-mediated reachability, provenance-carrying structural encounters — have no common definition and no deployed instance in combination; today's practice (web search, curated silos, domain-anchored cards) instantiates the limiting assumptions rather than escaping them; and the resulting failures — SEO-shaped selection, severed history on moves, invisible mediated agents, free indistinguishable claims, undefined freshness — are observable now and compound as agent populations and cross-organization coordination grow.

## Forward and backward dependencies

- **Transport (01), backward:** receives the relocated ambient-availability concern into structural discovery; the operator-model question flagged there recurs here as the index-operator question; relay-mediated reachability corresponds to transport's gateway and relay patterns.
- **Identity (02), backward:** identity-first resolution is the identifier/locator split surfacing as lookup; receives identity's introduction concern and its identifier-to-verification-material resolution requirement; move-survival depends on identity's continuity rules; index integrity inherits the issuance-cost question.
- **Authorization (03), backward:** the seam declared there is honored — capability claims in results are unverified until checked against the lineage model; discovery grants nothing.
- **Trust (05), backward:** the evaluation layer between results and decisions is document 05's subject; evidence-based filtering at result time and contamination of results by manufactured standing are jointly owned, and the retired score model is not resurrected here.
- **Communication (07), forward:** receives entity-level capability negotiation — the fine-grained bilateral phase after candidates are found — and the connection-establishment consequences of relay-mediated results.
- **Presence (provisional), forward:** structural discovery via presence is held open pending that document's disposition.

---

[← Trust](05-trust.md) &nbsp;·&nbsp; [README](README.md) &nbsp;·&nbsp; [Communication →](07-communication.md)
