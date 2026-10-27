# IETF AI Agent Standards Landscape

**Author**: Chris Hood, <chris@chrishood.com>, <chris@nomotic.ai>
**Contributors**: See below
**Version**: v.01 - 2026-07-19 (based on IETF Datatracker, mailing lists, BoF schedules for IETF 126 Vienna, and cross-referenced drafts)
**Purpose**: Living document to track venues and work items for AI agent and agentic standards.

---

## How to read this document

This is a living document. It is a catalog for awareness of current drafts and work items across the agent standards space, rather than an architectural reflection, comparative analysis, or a source of technical accuracy claims about the drafts it lists. Listings are descriptive and consistent across all entries. Technical claims about specific drafts belong in those drafts. Comparative claims between drafts are outside the scope of this catalog.

The full author list for each draft is available on the IETF Datatracker page linked from each entry. Author names appear in this document only where they form part of a draft's filename identifier (which is the standard IETF draft naming convention).

Each category uses three standard subsections, with bullets listed alphabetically:

- **IETF venues** — WGs, BoFs, RGs, mailing lists, side meetings where the topic lives.
- **IETF work items** — Internet-Drafts, WG documents, and proposed contributions in or targeting the IETF/IRTF stream.
- **External protocols and industry** — non-IETF specs, consortia, and vendor efforts adjacent to the IETF conversation.

All Internet-Draft references link to the Datatracker document page and will resolve to the current version. Additions and corrections are welcome as the space evolves.

---

## 1. Agent Identity, Auth, and Attestation

**IETF venues**

- AI Agent Security side meeting — IETF 125 (Shenzhen, March 2026), agenda and slides at github.com/liuchunchi/IETF125-AI-Agent-Security-Side-Meeting.
- DISPATCH — [draft-klrc-aiagent-auth](https://datatracker.ietf.org/doc/draft-klrc-aiagent-auth/) was dispatched at IETF 125.
- OAUTH WG (<oauth@ietf.org>) — venue for agent delegation extensions.
- RATS WG (<rats@ietf.org>) — venue hook for agent attestation (EAT profiles).
- Related: WEBBOTAUTH WG (bot/crawler auth — see Category 13).
- SCIM WG (<scim@ietf.org>) — agent identity provisioning and lifecycle extensions.
- SPICE WG (<spice@ietf.org>) — venue for the actor-chain and intent-chain drafts (draft-mw-spice-*).
- WIMSE WG (<wimse@ietf.org>) — agent-identity applicability discussions.

**IETF work items**

- [draft-aap-oauth-profile](https://datatracker.ietf.org/doc/draft-aap-oauth-profile/) — Agent Authorization Profile for OAuth 2.0.
- [draft-abbey-scim-agent-extension](https://datatracker.ietf.org/doc/draft-abbey-scim-agent-extension/) — SCIM "Agent" resource type and schemas for provisioning and deprovisioning agents and agentic applications across domains.
- [draft-aip-agent-identity-protocol](https://datatracker.ietf.org/doc/draft-aip-agent-identity-protocol/) — Agent Identity Protocol: agentic authentication and authorized policy enforcement.
- [draft-araut-oauth-transaction-tokens-for-agents](https://datatracker.ietf.org/doc/draft-araut-oauth-transaction-tokens-for-agents/) — extension to OAuth Transaction Tokens defining the agentic_ctx claim for agent context propagation.
- [draft-barney-caam](https://datatracker.ietf.org/doc/draft-barney-caam/) — Contextual Agent Authorization Mesh.
- [draft-beyer-agent-identity-architecture](https://datatracker.ietf.org/doc/draft-beyer-agent-identity-architecture/) — architectural model for human-anchored agent identity.
- [draft-beyer-agent-identity-problem-statement](https://datatracker.ietf.org/doc/draft-beyer-agent-identity-problem-statement/) — problem statement for human-anchored identity.
- [draft-bondar-wca](https://datatracker.ietf.org/doc/draft-bondar-wca/) — Warrant Certificate Authorities: auditable data provenance for AI-agent tool-call chains.
- [draft-chen-agent-decoupled-authorization-model](https://datatracker.ietf.org/doc/draft-chen-agent-decoupled-authorization-model/) — decoupled authorization model for agent-to-agent interactions.
- [draft-chen-oauth-rar-agent-extensions](https://datatracker.ietf.org/doc/draft-chen-oauth-rar-agent-extensions/) — policy, lifecycle, and intent extensions for OAuth Rich Authorization Requests.
- [draft-chen-oauth-scope-agent-extensions](https://datatracker.ietf.org/doc/draft-chen-oauth-scope-agent-extensions/) — structured and constraint extensions for OAuth scopes for agents.
- [draft-diaconu-agents-authz-info-sharing](https://datatracker.ietf.org/doc/draft-diaconu-agents-authz-info-sharing/) — cross-domain authorization information sharing for agents.
- [draft-drake-agent-identity-registry](https://datatracker.ietf.org/doc/draft-drake-agent-identity-registry/) — federated registry with hardware-anchored identity.
- [draft-ferro-dnsop-apertoid](https://datatracker.ietf.org/doc/draft-ferro-dnsop-apertoid/) — ApertoID: DNS-based agent identity declaration protocol.
- [draft-ferro-httpbis-apertoid-sig](https://datatracker.ietf.org/doc/draft-ferro-httpbis-apertoid-sig/) — ApertoID-Signature: HTTP request signing for AI agent identity.
- [draft-gaikwad-south-authorization](https://datatracker.ietf.org/doc/draft-gaikwad-south-authorization/) — SOUTH stochastic authorization protocol.
- [draft-goswami-agentic-jwt](https://datatracker.ietf.org/doc/draft-goswami-agentic-jwt/) — Secure Intent Protocol: JWT-compatible agentic identity and workflow management.
- [draft-gudlab-agentid-protocol](https://datatracker.ietf.org/doc/draft-gudlab-agentid-protocol/) — AgentID: identity protocol for AI agents.
- [draft-hartman-credential-broker-4-agents](https://datatracker.ietf.org/doc/draft-hartman-credential-broker-4-agents/) — CB4A: credential broker for agents.
- [draft-bubblefish-npamp](https://datatracker.ietf.org/doc/draft-bubblefish-npamp/) — N-PAMP (Native Post-Quantum Agent Messaging Protocol): binary multi-channel transport substrate for agent-to-agent traffic; one fixed-size frame, multiplexed channels, and three security profiles (Standard, High, Sovereign) that hold the wire format constant while escalating the cryptography; hybrid X25519 plus ML-KEM key establishment, authenticated encryption, forward-secure key schedule; runs over QUIC or TLS 1.3 with ALPN n-pamp/2. Independent Submission.
- [draft-hood-independent-agtp](https://datatracker.ietf.org/doc/draft-hood-independent-agtp/), [draft-hood-agtp-agent-cert](https://datatracker.ietf.org/doc/draft-hood-agtp-agent-cert/), [draft-hood-agtp-identifiers](https://datatracker.ietf.org/doc/draft-hood-agtp-identifiers/), [draft-hood-agtp-trust](https://datatracker.ietf.org/doc/draft-hood-agtp-trust/) — AGTP substrate identity: X.509 v3 Agent Certificates with Canonical Agent-ID and Owner-ID, trust score model, wire-layer attribution.
- [draft-ietf-wimse-arch](https://datatracker.ietf.org/doc/draft-ietf-wimse-arch/), [draft-ietf-wimse-identifier](https://datatracker.ietf.org/doc/draft-ietf-wimse-identifier/), [draft-ietf-wimse-workload-creds](https://datatracker.ietf.org/doc/draft-ietf-wimse-workload-creds/), [draft-ietf-wimse-wpt](https://datatracker.ietf.org/doc/draft-ietf-wimse-wpt/), [draft-ietf-wimse-http-signature](https://datatracker.ietf.org/doc/draft-ietf-wimse-http-signature/) — WIMSE working group deliverables.
- [draft-ihsanullah-dnsid](https://datatracker.ietf.org/doc/draft-ihsanullah-dnsid/) — DNSID: DNS-anchored agent identity; a name the agent can prove it controls, bound to an accountable entity, with lifecycle including rotation, revocation, retirement, and transfer; binding and key references for authentication.
- [draft-jia-oauth-scope-aggregation](https://datatracker.ietf.org/doc/draft-jia-oauth-scope-aggregation/) — OAuth 2.0 scope aggregation for multi-step AI agent workflows.
- [draft-kavian-agent-enrollment-protocol](https://datatracker.ietf.org/doc/draft-kavian-agent-enrollment-protocol/) — agent enrollment; part of a kavian draft family including AEP session-credential and did-web identity-method documents.
- [draft-kiliram-agent-trust-auth-framework](https://datatracker.ietf.org/doc/draft-kiliram-agent-trust-auth-framework/) — architectural framework for a cross-domain trust substrate covering verifiable agent identity, credentialing, cross-domain authorization, delegation, revocation, and auditability.
- [draft-klrc-aiagent-auth](https://datatracker.ietf.org/doc/draft-klrc-aiagent-auth/) — AIMS (Agent Identity Management System): composes WIMSE, SPIFFE, OAuth 2.0, and OpenID SSF/CAEP; delegation with user and system context preserved in tokens and audit trails.
- [draft-liu-oauth-a2a-profile](https://datatracker.ietf.org/doc/draft-liu-oauth-a2a-profile/) — OAuth profile for agent-to-agent interactions.
- [draft-messous-eat-ai](https://datatracker.ietf.org/doc/draft-messous-eat-ai/) — Entity Attestation Token profile for AI agents; claims for agent integrity, training provenance, runtime authorization.
- [draft-mishra-oauth-agent-grants](https://datatracker.ietf.org/doc/draft-mishra-oauth-agent-grants/) — DAAP: Delegated Agent Authorization Protocol.
- [draft-morrison-identity-accord](https://datatracker.ietf.org/doc/draft-morrison-identity-accord/) — identity accord specification for agents.
- [draft-morrison-identity-attributed-commits](https://datatracker.ietf.org/doc/draft-morrison-identity-attributed-commits/) — identity-attributed commits for source version control.
- [draft-morrison-identity-pronouns](https://datatracker.ietf.org/doc/draft-morrison-identity-pronouns/) — reference-axis pronoun grammar for handle identity.
- [draft-mw-oauth-actor-chain](https://datatracker.ietf.org/doc/draft-mw-oauth-actor-chain/) and [draft-mcguinness-oauth-actor-profile](https://datatracker.ietf.org/doc/draft-mcguinness-oauth-actor-profile/) — OAuth delegation-chain claims (nested act per RFC 8693; acti and actc candidates) and an actor-profile vocabulary for AI Agent, Sub-Agent, Tool, Service, and Human.
- [draft-mw-spice-actor-chain](https://datatracker.ietf.org/doc/draft-mw-spice-actor-chain/), [draft-mw-spice-intent-chain](https://datatracker.ietf.org/doc/draft-mw-spice-intent-chain/), and [draft-mw-spice-inference-chain](https://datatracker.ietf.org/doc/draft-mw-spice-inference-chain/) — actor chain (delegation provenance), intent chain (content provenance), inference chain (computational provenance); Merkle-rooted in the OAuth token.
- [draft-nandakumar-agent-sd-jwt](https://datatracker.ietf.org/doc/draft-nandakumar-agent-sd-jwt/) — SD Agent: selective disclosure for agent discovery and identity management.
- [draft-nennemann-wimse-ect](https://datatracker.ietf.org/doc/draft-nennemann-wimse-ect/) — ECT: execution context tokens for distributed agentic workflows.
- [draft-ni-wimse-ai-agent-identity](https://datatracker.ietf.org/doc/draft-ni-wimse-ai-agent-identity/) — WIMSE applicability to agentic AI; independent agent identities and credentials distinct from users and devices; dual-identity model.
- [draft-niyikiza-oauth-attenuating-agent-tokens](https://datatracker.ietf.org/doc/draft-niyikiza-oauth-attenuating-agent-tokens/) — attenuating authorization tokens for agentic delegation chains.
- [draft-oauth-ai-agents-on-behalf-of-user](https://datatracker.ietf.org/doc/draft-oauth-ai-agents-on-behalf-of-user/) — OAuth 2.0 on-behalf-of extension; authorization-endpoint agent identification and a new grant type; resulting token records user, agent, and client identity.
- [draft-pidlisnyi-aps](https://datatracker.ietf.org/doc/draft-pidlisnyi-aps/) — APS: Agent Passport System; cryptographic identity, faceted authority attenuation, and governance.
- [draft-prakash-aip](https://datatracker.ietf.org/doc/draft-prakash-aip/) — AIP: Agent Identity Protocol; verifiable delegation for AI agent systems.
- [draft-ravikiran-clawdentity-protocol](https://datatracker.ietf.org/doc/draft-ravikiran-clawdentity-protocol/) — Clawdentity: cryptographic identity and trust protocol for AI agent communication.
- [draft-rosenberg-oauth-aauth](https://datatracker.ietf.org/doc/draft-rosenberg-oauth-aauth/) — AAuth: OAuth 2.1 extension for agents obtaining tokens on behalf of users reached over PSTN and texting channels; anti-impersonation controls.
- [draft-shang-campus-agent-scope-down](https://datatracker.ietf.org/doc/draft-shang-campus-agent-scope-down/) — campus agent identification and scope-down access control.
- [draft-sharif-agent-identity-framework](https://datatracker.ietf.org/doc/draft-sharif-agent-identity-framework/) — five-layer model (identity, authorization, attestation, evidence, trust).
- [draft-sharif-openid-agent-identity](https://datatracker.ietf.org/doc/draft-sharif-openid-agent-identity/) — OpenID Connect agent identity claims.
- [draft-singla-agent-identity-protocol](https://datatracker.ietf.org/doc/draft-singla-agent-identity-protocol/) — did:aip DIDs with delegation chains.
- [draft-somoza-dmsc-atn-agent-trust-negotiation](https://datatracker.ietf.org/doc/draft-somoza-dmsc-atn-agent-trust-negotiation/) — Agent Trust Negotiation: Capability, Delegation, and Provenance Binding for AI Agents.
- [draft-song-oauth-ai-agent-collaborate-authz](https://datatracker.ietf.org/doc/draft-song-oauth-ai-agent-collaborate-authz/) — OAuth 2.0 extension for multi-AI agent collaboration.
- [draft-vandemeent-jis-identity](https://datatracker.ietf.org/doc/draft-vandemeent-jis-identity/) — JIS: JTel Identity Standard for identity and trust establishment for agents.
- [draft-wahl-scim-agent-schema](https://datatracker.ietf.org/doc/draft-wahl-scim-agent-schema/) and [draft-wzdk-scim-agent-resource](https://datatracker.ietf.org/doc/draft-wzdk-scim-agent-resource/) — platform-neutral SCIM "AgenticIdentity" schema for representing and lifecycle-managing AI agent identities.
- [draft-williams-intent-token](https://datatracker.ietf.org/doc/draft-williams-intent-token/) — Intent Token: cryptographic authorization primitive for agents.
- [draft-yakung-oauth-agent-attestation](https://datatracker.ietf.org/doc/draft-yakung-oauth-agent-attestation/) — ACAP: Agent Credential Attestation Protocol.

**External protocols and industry**

- NIST — NCCoE concept paper on AI agent identity and authorization; AI Agent Standards Initiative.
- OpenID Foundation — "Identity Management for Agentic AI" whitepaper; SSF/CAEP for monitoring.
- SPIFFE/SVID (CNCF) — workload identity primitives.
- W3C DIDs — identity layer used by ANP (see Categories 2 and 9).

---

## 2. Agent Discovery, Naming, and Directories

**IETF venues**

- agentproto and agent2agent discussions (discovery threads).
- DAWN BoF (WG-forming; 21 July 2026, IETF 126) and <dawn@ietf.org> (created April 2026). Scope: discovery of entities generally — tasks, workloads, endpoints, services, AI agents.
- DNSOP (<dnsop@ietf.org>) — venue for DNS-AID and DNS-based discovery mechanisms.

**IETF work items**

- [draft-aiendpoint-ai-discovery](https://datatracker.ietf.org/doc/draft-aiendpoint-ai-discovery/) — well-known unauthenticated HTTP "AI Discovery Document" for service discovery and capability exposure.
- [draft-cui-ai-agent-discovery-invocation](https://datatracker.ietf.org/doc/draft-cui-ai-agent-discovery-invocation/) — AIDIP: unified agent metadata schema, two-mode discovery, semantic resolution, unified invocation API, security framework.
- [draft-cui-dns-native-agent-naming-resolution](https://datatracker.ietf.org/doc/draft-cui-dns-native-agent-naming-resolution/) — DNS-Native AI Agent Naming and Resolution.
- [draft-gaikwad-woa](https://datatracker.ietf.org/doc/draft-gaikwad-woa/) — Web of Agents host-level agent manifest primitive.
- [draft-hood-agtp-discovery](https://datatracker.ietf.org/doc/draft-hood-agtp-discovery/) and [draft-hood-agtp-presence](https://datatracker.ietf.org/doc/draft-hood-agtp-presence/) — AGTP substrate discovery: the DISCOVER method, the Agent Name Service (ANS), and AGTP-Presence ambient substrate visibility; covers agent, tool, resource, and API discovery.
- [draft-jakab-dawn-agent-discovery-mdns](https://datatracker.ietf.org/doc/draft-jakab-dawn-agent-discovery-mdns/) — local-link agent discovery using mDNS and DNS-SD zero-configuration machinery.
- [draft-kay-dawn-use-cases](https://datatracker.ietf.org/doc/draft-kay-dawn-use-cases/) — DAWN use cases for agents, workloads, and named entities.
- [draft-king-dawn-requirements](https://datatracker.ietf.org/doc/draft-king-dawn-requirements/) — companion DAWN requirements draft.
- [draft-liu-agent-metadata-sync-protocol](https://datatracker.ietf.org/doc/draft-liu-agent-metadata-sync-protocol/) — agent metadata synchronization protocol.
- [draft-morrison-mcp-dns-discovery](https://datatracker.ietf.org/doc/draft-morrison-mcp-dns-discovery/) — domain-scoped unicast DNS TXT records for MCP server discovery.
- [draft-moussa-dawn-gap-analysis](https://datatracker.ietf.org/doc/draft-moussa-dawn-gap-analysis/) — gap analysis of existing discovery mechanisms against DAWN requirements.
- [draft-mozley-aidiscovery](https://datatracker.ietf.org/doc/draft-mozley-aidiscovery/) — AID problem statement; requirements for context-aware discovery, capability schemas, versioning and lifecycle, trust in the discovery process, organizational control over advertising agents.
- [draft-mozleywilliams-dnsop-dnsaid](https://datatracker.ietf.org/doc/draft-mozleywilliams-dnsop-dnsaid/) — DNS SVCB and HTTPS records for federated capability discovery.
- [draft-mp-agntcy-ads](https://datatracker.ietf.org/doc/draft-mp-agntcy-ads/) — Agntcy Agent Directory Service.
- [draft-narajala-courtney-ansv2](https://datatracker.ietf.org/doc/draft-narajala-courtney-ansv2/) — successor to the expired original ANS draft; DNS-anchored identity providing stable cross-domain identifiers referenced by delegation chains.
- [draft-narvaneni-agent-uri](https://datatracker.ietf.org/doc/draft-narvaneni-agent-uri/) — the agent:// Protocol: URI-based framework for interoperable agents.
- [draft-nemethi-aid-agent-identity-discovery](https://datatracker.ietf.org/doc/draft-nemethi-aid-agent-identity-discovery/) — DNS TXT records under _agent for agent identity discovery.
- [draft-ni-agent-entity-discovery](https://datatracker.ietf.org/doc/draft-ni-agent-entity-discovery/) — DNS and DANE-style entity-level discovery with credential bindings.
- [draft-pioli-agent-discovery](https://datatracker.ietf.org/doc/draft-pioli-agent-discovery/) — ARDP: Agent Registration and Discovery Protocol.
- [draft-raskar-agentic-web-federated-resolution](https://datatracker.ietf.org/doc/draft-raskar-agentic-web-federated-resolution/) — registry-assisted discovery for agents and workloads without a usable DNS Discovery Anchor.
- [draft-rehfeld-apix-core](https://datatracker.ietf.org/doc/draft-rehfeld-apix-core/) — APIX: HATEOAS-based machine-native service index for agents; governance model, three-dimensional trust model, APIX Manifest (APM), Index API; profile documents [draft-rehfeld-apix-services](https://datatracker.ietf.org/doc/draft-rehfeld-apix-services/) (web APIs and bots) and [draft-rehfeld-apix-iot](https://datatracker.ietf.org/doc/draft-rehfeld-apix-iot/) (IoT devices).
- [draft-song-anp-ans](https://datatracker.ietf.org/doc/draft-song-anp-ans/) (Agent Name System) and [draft-song-anp-adp](https://datatracker.ietf.org/doc/draft-song-anp-adp/) (Agent Description Protocol) — naming and description and discovery members of the ANP suite; agent:// URIs mapped to cryptographic peer identities with DHT and GossipSub dissemination.
- [draft-vandemeent-ains-discovery](https://datatracker.ietf.org/doc/draft-vandemeent-ains-discovery/) — AINS: AInternet Name Service; agent discovery and trust resolution protocol.
- [draft-ye-problems-and-requirements-of-dns-for-ioa](https://datatracker.ietf.org/doc/draft-ye-problems-and-requirements-of-dns-for-ioa/) — problem statement and requirements analysis of DNS for Internet of Agents.
- [draft-seethiraju-dawn-dan](https://datatracker.ietf.org/doc/draft-seethiraju-dawn-dan/) — DNS-Based Agent Naming (DAN): AIDISCA and AIINDEX Resource Records for AI Agent Discovery.

**External protocols and industry**

- A2A Agent Cards and MCP server discovery conventions — de facto discovery metadata formats.
- Agntcy (Linux Foundation) directory service — upstream of draft-mp-agntcy-ads.

---

## 3. Agent-to-Agent Communication (Core Protocols)

**IETF venues**

- agentproto BoF (WG-forming; 23 July 2026, IETF 126) and <agent2agent@ietf.org> (created April 2025).
- CATALIST coordination (BoF held at IETF 125; see Category 15).
- IETF 124 side meeting that produced the strawman charter.

**IETF work items**

- [draft-chang-agent-context-interaction](https://datatracker.ietf.org/doc/draft-chang-agent-context-interaction/) — agent context interaction optimizations.
- [draft-cowles-aee](https://datatracker.ietf.org/doc/draft-cowles-aee/) — AEE: Agent Envelope Exchange; minimal JSON envelope format for inter-agent communication.
- [draft-cowles-aocl](https://datatracker.ietf.org/doc/draft-cowles-aocl/) — AOCL: Agent Orchestration Control Layers protocol.
- [draft-eckert-catalist-acip-framework](https://datatracker.ietf.org/doc/draft-eckert-catalist-acip-framework/) — ACIP: Agent Communications Internet Protocol; framework for agent-aware networks.
- [draft-fu-nmop-agent-communication-framework](https://datatracker.ietf.org/doc/draft-fu-nmop-agent-communication-framework/) — agent communication framework for Network AIOps.
- [draft-han-agent-comm-enterprise](https://datatracker.ietf.org/doc/draft-han-agent-comm-enterprise/) — considerations for AI agent communication and networking in enterprise.
- [draft-han-rtgwg-agent-gateway-intercomm-framework](https://datatracker.ietf.org/doc/draft-han-rtgwg-agent-gateway-intercomm-framework/) — agent gateway intercommunication framework.
- [draft-hood-independent-agtp](https://datatracker.ietf.org/doc/draft-hood-independent-agtp/), [draft-hood-agtp-api](https://datatracker.ietf.org/doc/draft-hood-agtp-api/), [draft-hood-agtp-session](https://datatracker.ietf.org/doc/draft-hood-agtp-session/), [draft-hood-agtp-communication](https://datatracker.ietf.org/doc/draft-hood-agtp-communication/) — Runtime Contract Negotiation Substrate (RCNS); semantic methods including QUERY, DISCOVER, DELEGATE, EXECUTE, COLLABORATE, PURCHASE.
- [draft-hw-protocol-agent](https://datatracker.ietf.org/doc/draft-hw-protocol-agent/) — AI agent protocols for multi-modality.
- [draft-jeskey-anml](https://datatracker.ietf.org/doc/draft-jeskey-anml/) — ANML: Agent Native Messaging Language; semantic vocabulary and message envelope for agent-to-agent messaging.
- [draft-jesske-ai-enablement-interface](https://datatracker.ietf.org/doc/draft-jesske-ai-enablement-interface/) — AI enablement interface between telco communication platforms and AI service providers.
- [draft-jurkovikj-httpapi-agentic-state](https://datatracker.ietf.org/doc/draft-jurkovikj-httpapi-agentic-state/) — HTTP profile for synchronized resource state (agentic state transfer).
- [draft-mallick-muacp](https://datatracker.ietf.org/doc/draft-mallick-muacp/) — uACP: Micro Agent Communication Protocol.
- [draft-nederveld-adl](https://datatracker.ietf.org/doc/draft-nederveld-adl/) — ADL: Agent Definition Language.
- [draft-rosenberg-aiproto-framework](https://datatracker.ietf.org/doc/draft-rosenberg-aiproto-framework/) — framework, use cases, and requirements for AI agent protocols.
- [draft-teodor-pilot-protocol](https://datatracker.ietf.org/doc/draft-teodor-pilot-protocol/) — Pilot Protocol: overlay network for agent communication.
- [draft-zyyhl-agent-networks-framework](https://datatracker.ietf.org/doc/draft-zyyhl-agent-networks-framework/) — framework for AI agent networks based on ANP.

**External protocols and industry**

- A2A (Agent2Agent — Google, now Linux Foundation).
- AConP (Agent Connect Protocol — Cisco/Agntcy) and AITP (NEAR).
- ACP (Agent Communication Protocol).
- ANP (Agent Network Protocol — open-source community).
- MCP (Model Context Protocol — Anthropic and AAIF).

---

## 4. Agent-to-Tool and API Invocation

**IETF venues**

- agentproto BoF and agent2agent list (tool-invocation building blocks are in scope of the charter discussion).

**IETF work items**

- [draft-cui-ai-agent-discovery-invocation](https://datatracker.ietf.org/doc/draft-cui-ai-agent-discovery-invocation/) — AIDIP; unified invocation API (also see Category 2).
- [draft-hood-agtp-discovery](https://datatracker.ietf.org/doc/draft-hood-agtp-discovery/) and [draft-hood-agtp-api](https://datatracker.ietf.org/doc/draft-hood-agtp-api/) — AGTP substrate handles tool, resource, and API discovery and invocation within the same substrate as agent-to-agent communication, using the semantic methods (QUERY, EXECUTE, DELEGATE).
- [draft-pelov-bounded-agent-capabilities](https://datatracker.ietf.org/doc/draft-pelov-bounded-agent-capabilities/) — bounded agent capabilities: problem statement.
- [draft-pelov-rich-architecture](https://datatracker.ietf.org/doc/draft-pelov-rich-architecture/) — companion architecture for bounded agent capabilities.
- [draft-rosenberg-aiproto-a2t](https://datatracker.ietf.org/doc/draft-rosenberg-aiproto-a2t/) — A2T: Agent-to-Tool Protocol; OpenAPI-style enumeration and invocation of third-party tools by enterprise agents.
- [draft-rosenberg-aiproto-nact](https://datatracker.ietf.org/doc/draft-rosenberg-aiproto-nact/) — N-ACT: Normalized API for AI Agents Calling Tools.

**External protocols and industry**

- MCP — tool-invocation protocol.
- OpenAPI and JSON-RPC ecosystems as substrate.

---

## 5. Semantic Intent, Verb Taxonomies, and Action Description Layers

**IETF venues**

- No dedicated venue. Embedded in Communication (agentproto) and Collaboration (DMSC) discussions; potential dedicated semantic and interop threads on agent2agent.

**IETF work items**

- Agent description schemas (ADP in the ANP suite, AIDIP metadata, A2A-style capability cards mirrored in discovery drafts).
- [draft-hood-agtp-api](https://datatracker.ietf.org/doc/draft-hood-agtp-api/) — AGTP intent-based verb taxonomy with categories including ACQUIRE, COMPUTE, TRANSACT, ORCHESTRATE, NOTIFY, QUERY; machine-readable intent expression; semantic methods (QUERY, DISCOVER, DELEGATE, EXECUTE, COLLABORATE, PURCHASE).
- [draft-jeskey-anml](https://datatracker.ietf.org/doc/draft-jeskey-anml/) — ANML semantic vocabulary; also in Category 3.
- [draft-sz-dmsc-iaip](https://datatracker.ietf.org/doc/draft-sz-dmsc-iaip/) — IAIP: Intent-based Agent Interconnection Protocol at Agent Gateway; routing semantics at the gateway boundary (see Category 6).
- [draft-verma-dmsc-nlip-notes](https://datatracker.ietf.org/doc/draft-verma-dmsc-nlip-notes/) — using natural language for universal coordination in multi-agent systems (NLIP notes; DMSC-tagged).
- [draft-yang-dmsc-gateway-semantic-layer](https://datatracker.ietf.org/doc/draft-yang-dmsc-gateway-semantic-layer/) — Gateway Mediation Layer for AI Agent Collaboration; semantic translation at the gateway boundary.
- [draft-zhang-dmsc-ioa-semantic-interaction](https://datatracker.ietf.org/doc/draft-zhang-dmsc-ioa-semantic-interaction/) — semantic interaction for the Internet of Agents.

**External protocols and industry**

- Academic literature on semantic views of agent communication protocols.
- MCP, A2A, and related protocols with structured actions and tools.

---

## 6. Multi-Agent Collaboration and Gateways

**IETF venues**

- agent2agent and CATALIST (DMSC presented in the CATALIST BoF at IETF 125).
- agentproto (new mailing list as of July 2026 to discuss agent based session layer IETF 126).
- DMSC BoF (non-WG-forming; 22 July 2026, IETF 126) — AI Agent Gateway-mediated collaboration: capability exposure, request forwarding, coordination, synchronization, policy control, observability, secure communication. A GitHub org (ietf-dmsc) tracks meeting materials.
- Mailing lists — <dmsc@ietf.org> exists (archive at mailarchive.ietf.org/arch/browse/dmsc/); IETF 126 BoF announcement additionally directs discussion to the DAWN list (<dawn@ietf.org>).

**IETF work items**

- 6G agent use-case drafts: [draft-sarischo-6gip-aiagent-requirements](https://datatracker.ietf.org/doc/draft-sarischo-6gip-aiagent-requirements/), [draft-stephan-ai-agent-6g](https://datatracker.ietf.org/doc/draft-stephan-ai-agent-6g/), and the "AI Network for Training, Inference, and Agentic Interactions" draft.
- [draft-agent-gw](https://datatracker.ietf.org/doc/draft-agent-gw/) — agent communication gateway for semantic routing and working memory.
- [draft-cui-ai-agent-task](https://datatracker.ietf.org/doc/draft-cui-ai-agent-task/) — task-oriented coordination requirements for AI agent protocols.
- [draft-cui-dmsc-agent-cdi](https://datatracker.ietf.org/doc/draft-cui-dmsc-agent-cdi/) — cross-domain interoperability framework for AI agent collaboration.
- [draft-dunbar-agent-attachment](https://datatracker.ietf.org/doc/draft-dunbar-agent-attachment/) — Agent Attachment Protocol.
- [draft-dunbar-dmsc-gw-scenarios-gap-analysis](https://datatracker.ietf.org/doc/draft-dunbar-dmsc-gw-scenarios-gap-analysis/) — seven gateway properties and gap analysis for the DMSC gateway proposal.
- [draft-hood-agtp-session](https://datatracker.ietf.org/doc/draft-hood-agtp-session/) — AGTP session substrate for multi-agent orchestration via sessions, transfer, and intent routing.
- [draft-li-dmsc-inf-architecture](https://datatracker.ietf.org/doc/draft-li-dmsc-inf-architecture/) — DMSC infrastructure architecture.
- [draft-li-dmsc-macp](https://datatracker.ietf.org/doc/draft-li-dmsc-macp/) — Multi-agent Collaboration Protocol Suite; Agent Gateways handle registration, authentication, capability management.
- [draft-liu-dmsc-acps-arc](https://datatracker.ietf.org/doc/draft-liu-dmsc-acps-arc/) — agent collaboration protocols architecture for the Internet of Agents.
- [draft-liu-dmsc-gw-requirements](https://datatracker.ietf.org/doc/draft-liu-dmsc-gw-requirements/) — agent gateway requirements.
- [draft-mapmw-task-discovery](https://datatracker.ietf.org/doc/draft-mapmw-task-discovery/) — task discovery in agentic networks.
- [draft-morrison-agent-channel-fan-out](https://datatracker.ietf.org/doc/draft-morrison-agent-channel-fan-out/) — application-layer delivery frame for intra-principal and intra-organisation fan-out over a per-handle stream.
- [draft-song-dmsc-problem-statement](https://datatracker.ietf.org/doc/draft-song-dmsc-problem-statement/) — problem statement and requirements for DMSC; gateway layer offloading secured communication, cross-domain connectivity, multi-tenant policy enforcement, and collaboration assistance.
- [draft-sun-zhang-iaip](https://datatracker.ietf.org/doc/draft-sun-zhang-iaip/) and [draft-sz-dmsc-iaip](https://datatracker.ietf.org/doc/draft-sz-dmsc-iaip/) — Intent-based Agent Interconnection Protocol at Agent Gateway.
- [draft-yang-dmsc-ioa-task-protocol](https://datatracker.ietf.org/doc/draft-yang-dmsc-ioa-task-protocol/) — Internet of Agents Task Protocol for heterogeneous agent collaboration.
- [draft-zhang-dmsc-gateway-directory-sync](https://datatracker.ietf.org/doc/draft-zhang-dmsc-gateway-directory-sync/) — Gateway Capability Directory and Synchronization for the Internet of Agents; directory synchronization across gateways.
- The DMSC proponents list additional related drafts, including [draft-wang-hjs-judgment-event](https://datatracker.ietf.org/doc/draft-wang-hjs-judgment-event/).

**External protocols and industry**

- Enterprise agent gateway products (API-gateway vendors extending to agent mediation).
- ETSI ENI GR 056 — multi-agent frameworks for next-generation core networks.

---

## 7. Trust, Human-in-the-Loop, Oversight, and Confirmation

**IETF venues**

- No dedicated venue yet; discussed within agentproto framing, OAuth (CIBA-based flows), WIMSE applicability, and the proposed AUDIT charter (see Category 8).

**IETF work items**

- [draft-borthwick-msebenzi-environment-state](https://datatracker.ietf.org/doc/draft-borthwick-msebenzi-environment-state/) — environment.* constraint family: pre-action fail-closed gates whose failure mode must halt execution; a host-binding profile lets any conforming mandate format carry the family.
- [draft-cui-nmrg-llm-nm](https://datatracker.ietf.org/doc/draft-cui-nmrg-llm-nm/) — framework for LLM Agent-assisted network management with human-in-the-loop.
- [draft-hood-independent-agtp](https://datatracker.ietf.org/doc/draft-hood-independent-agtp/), [draft-hood-agtp-identifiers](https://datatracker.ietf.org/doc/draft-hood-agtp-identifiers/), and [draft-hood-agtp-trust](https://datatracker.ietf.org/doc/draft-hood-agtp-trust/) — AGTP intervention and governance layers; substrate-level confirmation and oversight primitives.
- [draft-klrc-aiagent-auth](https://datatracker.ietf.org/doc/draft-klrc-aiagent-auth/) — CIBA-based human-in-the-loop mechanism inside the AIMS model; identity-bound audit trails.
- [draft-kuehlewind-audit-architecture](https://datatracker.ietf.org/doc/draft-kuehlewind-audit-architecture/) — human-in-the-loop escalations (step-up approvals, refusals, missed escalations) as identifiable records bindable to a run's Interaction Record.
- [draft-morrison-binding-moment-envelope](https://datatracker.ietf.org/doc/draft-morrison-binding-moment-envelope/) — how consequential decisions are presented to a human principal at binding moments; dual-veto envelope with typed outcomes (commit, decline, amend, reject).
- [draft-morrison-org-alter-policy-provision](https://datatracker.ietf.org/doc/draft-morrison-org-alter-policy-provision/) — organizational policy provision for agents.
- [draft-ni-wimse-ai-agent-identity](https://datatracker.ietf.org/doc/draft-ni-wimse-ai-agent-identity/) — includes a comparison with CHEQ (out-of-band verification vs. user double-confirmation).
- [draft-rosenberg-aiproto-cheq](https://datatracker.ietf.org/doc/draft-rosenberg-aiproto-cheq/) — CHEQ: human confirmation of agent-proposed decisions before execution; privacy-preserving human entry of information needed for tool invocation without disclosure to the agent.
- [draft-rosomakho-oauth-txn-challenge](https://datatracker.ietf.org/doc/draft-rosomakho-oauth-txn-challenge/) — OAuth Transaction Authorization Challenge: mechanism for a protected resource to request transaction-specific authorization from a human approver before completing an operation; complements OAuth step-up authentication and CIBA by requesting authorization for a specific transaction rather than fresher authentication alone.
- [draft-schrock-ep-authorization-receipts](https://datatracker.ietf.org/doc/draft-schrock-ep-authorization-receipts/) — authorization receipts for named-human, exact-action authorization of agent operations.
- [draft-schrock-human-authorization-binding](https://datatracker.ietf.org/doc/draft-schrock-human-authorization-binding/) — binding named-human authorization evidence into agent-action records.
- [draft-somoza-dmsc-atn-agent-trust-negotiation](https://datatracker.ietf.org/doc/draft-somoza-dmsc-atn-agent-trust-negotiation/) — Agent Trust Negotiation: Capability, Delegation, and Provenance Binding for AI Agents.
- [draft-yossif-psea](https://datatracker.ietf.org/doc/draft-yossif-psea/) — PSEA Token Profile: an EAT profile (RFC 9711) carrying evidence that a user-verification-gated key on the user's authenticator signed a canonical digest of a specific action payload at execution time; fail-closed action binding, with attested and asserted user-verification anchoring distinguished normatively. Positioned as complementary to OAuth step-up authentication (RFC 9470). See also Categories 1 and 15.

**External protocols and industry**

- EU AI Act Articles 14 and 26 (human oversight, log retention) as regulatory drivers.
- OpenID CIBA as the underlying flow.

---

## 8. Audit and Accountability

**IETF venues**

- CATALIST.
- Proposed AUDIT BoF — draft and strawman charter circulated 19 May 2026 on agent2agent and cross-posted to OAuth and WIMSE lists (charter at github.com/mirjak/draft-audit-architecture); proposed deliverables include an architecture, audit data models and semantics, token formats for audit information, and a deployment BCP. AUDIT was excluded from the five approved IETF 126 BoFs; watch for IETF 127.
- SCITT WG — supply-chain transparency machinery reusable for agent action transparency.

**IETF work items**

- [draft-aravind-oauth-decision-subject](https://datatracker.ietf.org/doc/draft-aravind-oauth-decision-subject/) — dsub: JWT claim naming the party a decision is about on the decision record, distinct from subject, actor, and resource owner; descriptive and non-authorizing.
- [draft-aravind-oauth-operator-of-record](https://datatracker.ietf.org/doc/draft-aravind-oauth-operator-of-record/) — opr: JWT claim marking whether a human or an agent operated when a presentation or decision was produced; descriptive and non-authorizing.
- [draft-birkholz-verifiable-agent-conversations](https://datatracker.ietf.org/doc/draft-birkholz-verifiable-agent-conversations/) — CDDL data format (JSON and CBOR) for verifiable agent conversation records: session metadata, message exchanges, tool invocations, reasoning traces, system events; COSE-signed for SCITT Transparency Service interoperability and RFC 9334 Evidence integration.
- [draft-bondar-wca](https://datatracker.ietf.org/doc/draft-bondar-wca/) — WCA: Warrant Certificate Authorities; auditable data provenance for AI-agent tool-call chains.
- [draft-bubblefish-naalp](https://datatracker.ietf.org/doc/draft-bubblefish-naalp/) — N-AALP (Native Agent Application Layer Protocol): signed-object application layer that rides on a transport such as N-PAMP; each object is a deterministic-CBOR record signed as COSE_Sign1 whose declared effect is its authorization, with a single-use approval ledger and a hash-chained signed audit chain in which the object signer and the audit ordering authority are separate keys. Independent Submission.
- [draft-cui-cdi](https://datatracker.ietf.org/doc/draft-cui-cdi/) — Cross-Domain Interaction with delegation constraints.
- [draft-helixar-hdp-agentic-delegation](https://datatracker.ietf.org/doc/draft-helixar-hdp-agentic-delegation/) — HDP: Human Delegation Provenance Protocol; cryptographic chain-of-custody for agentic AI systems.
- [draft-hood-agtp-log](https://datatracker.ietf.org/doc/draft-hood-agtp-log/) and [draft-hood-agtp-identifiers](https://datatracker.ietf.org/doc/draft-hood-agtp-identifiers/) — AGTP logging: five identity-lifecycle events (genesis issued, revoked, suspended, reinstated, deprecated) as a governance-signed SCITT-aligned transparency log; per-action Attribution-Records agent-signed and hash-chained via previous_audit_id, self-contained third-party-verifiable via AGTP-CERT.
- [draft-krausz-verification-state](https://datatracker.ietf.org/doc/draft-krausz-verification-state/) — the verification.* family: pre-action verification-state records asserting what was checked before an action proceeds; RFC 8785 JCS canonicalization, Ed25519 JWS, multi-issuer composed envelopes co-signed by independent verifiers.
- [draft-kuehlewind-audit-architecture](https://datatracker.ietf.org/doc/draft-kuehlewind-audit-architecture/) — architecture for auditing AI agent delegation and interactions; Interaction, Action, and Delegation Records; work items include a delegation-chain wire profile building on RFC 8693 nested act claims, draft-mw-oauth-actor-chain, and draft-mcguinness-oauth-actor-profile.
- [draft-liu-agent-operation-authorization](https://datatracker.ietf.org/doc/draft-liu-agent-operation-authorization/) — two-phase framework for verifiable delegation of actions from human principals to agents with fine-grained operation authorization.
- [draft-mih-agent-bilateral-attestation](https://datatracker.ietf.org/doc/draft-mih-agent-bilateral-attestation/) — bilateral attestation for cross-organization actions.
- [draft-mih-sato-agent-accountability-composition](https://datatracker.ietf.org/doc/draft-mih-sato-agent-accountability-composition/) — CAN/WHO/WHAT/AUDIT accountability composition.
- [draft-mih-scitt-agent-action-capsule](https://datatracker.ietf.org/doc/draft-mih-scitt-agent-action-capsule/) — SCITT-anchored action records.
- [draft-morrison-substrate-provenance-grammar](https://datatracker.ietf.org/doc/draft-morrison-substrate-provenance-grammar/) — annotation grammar for agent output provenance.
- [draft-msebenzi-evidence-action](https://datatracker.ietf.org/doc/draft-msebenzi-evidence-action/) — the evidence.* family: post-hoc, independently recomputable evidence records for agent tool calls; JCS-canonical signed receipts, hash-chained.
- [draft-mw-spice-actor-chain](https://datatracker.ietf.org/doc/draft-mw-spice-actor-chain/), [draft-mw-spice-intent-chain](https://datatracker.ietf.org/doc/draft-mw-spice-intent-chain/), and [draft-mw-spice-inference-chain](https://datatracker.ietf.org/doc/draft-mw-spice-inference-chain/) — delegation, content, and computational provenance chains.
- [draft-nelson-agent-delegation-receipts](https://datatracker.ietf.org/doc/draft-nelson-agent-delegation-receipts/) — cryptographic delegation receipt protocol with model state attestation.
- [draft-rampalli-pedigree](https://datatracker.ietf.org/doc/draft-rampalli-pedigree/) — PEDIGREE: delegation chain semantics with pre-authorization model.
- [draft-sato-soos-aep](https://datatracker.ietf.org/doc/draft-sato-soos-aep/) — SOOS Agent Execution Protocol: SENSE/PLAN/ACT/OBSERVE flow with governance enforcement component boundary.
- [draft-sato-soos-cap](https://datatracker.ietf.org/doc/draft-sato-soos-cap/) — SOOS Constitutional AI Protocol: enforcement architecture using Cedar policy evaluation with three-tier prohibitions.
- [draft-sato-soos-cap-rrs](https://datatracker.ietf.org/doc/draft-sato-soos-cap-rrs/) — SOOS Regulation Record Specification: machine-compilable representations of legal compliance obligations.
- [draft-sato-soos-dam](https://datatracker.ietf.org/doc/draft-sato-soos-dam/) — SOOS Data Artifact Management.
- [draft-sato-soos-faip](https://datatracker.ietf.org/doc/draft-sato-soos-faip/) — SOOS Federated Agent Intelligence Protocol.
- [draft-sato-soos-gar](https://datatracker.ietf.org/doc/draft-sato-soos-gar/) — SOOS Governance Audit Record.
- [draft-sato-soos-hem](https://datatracker.ietf.org/doc/draft-sato-soos-hem/) — SOOS Human Escalation Mechanism.
- [draft-sato-soos-idp](https://datatracker.ietf.org/doc/draft-sato-soos-idp/) — SOOS Intent Declaration Primitive.
- [draft-sato-soos-mjwt](https://datatracker.ietf.org/doc/draft-sato-soos-mjwt/) — SOOS Mandate JWT.
- [draft-sato-soos-pt](https://datatracker.ietf.org/doc/draft-sato-soos-pt/) — SOOS Progressive Trust.
- [draft-sato-soos-sov](https://datatracker.ietf.org/doc/draft-sato-soos-sov/) — SOOS Sovereign Object.
- [draft-schrock-ep-action-evidence-graph](https://datatracker.ietf.org/doc/draft-schrock-ep-action-evidence-graph/) — EP-AEG: Action Evidence Graphs and Evidence Policy Replay for High-Risk Agent Actions; portable content-addressed graph of references to signed artifacts about one action; deterministic offline evaluation against a relying-party-supplied evidence policy.
- [draft-schrock-ep-authority-introduction](https://datatracker.ietf.org/doc/draft-schrock-ep-authority-introduction/) — Authority Documents and Graded Introduction: trust establishment for agent-action evidence without prior federation.
- [draft-schrock-ep-authorization-receipts](https://datatracker.ietf.org/doc/draft-schrock-ep-authorization-receipts/) — authorization receipts: self-contained signed artifacts binding named-human or M-of-N quorum authorization to an exact operation, verifiable by a relying party offline.
- [draft-schrock-ep-enforcement-point](https://datatracker.ietf.org/doc/draft-schrock-ep-enforcement-point/) — Enforcement Point: mechanisms for effect-boundary enforcement of authorization evidence.
- [draft-schrock-ep-evidence-record](https://datatracker.ietf.org/doc/draft-schrock-ep-evidence-record/) — evidence record format for agent-action evidence.
- [draft-schrock-human-authorization-binding](https://datatracker.ietf.org/doc/draft-schrock-human-authorization-binding/) — binding named-human authorization evidence into agent-action records.
- [draft-sharif-agent-audit-trail](https://datatracker.ietf.org/doc/draft-sharif-agent-audit-trail/) — Agent Audit Trail: standard logging format for AI systems.
- [draft-sharif-attp-agent-trust-transport](https://datatracker.ietf.org/doc/draft-sharif-attp-agent-trust-transport/) — ATTP: protocol-agnostic framework for trust scoring, cryptographic identity, action-limit enforcement, compliance gating, and tamper-evident audit for AI agents.
- [draft-sharif-audit-trail](https://datatracker.ietf.org/doc/draft-sharif-audit-trail/) — audit trail patterns.
- [draft-sokolov-rats-aep-composition](https://datatracker.ietf.org/doc/draft-sokolov-rats-aep-composition/) — binds application-layer Action Evidence Packages (signed per-action records of what an AI-agent system did, under what authority, with what outcome) to platform Evidence per the RFC 9334 remote attestation architecture, so a single Verifier appraises the application-layer action and the platform state together.
- [draft-stone-atep](https://datatracker.ietf.org/doc/draft-stone-atep/) — ATEP: Agent Trust and Execution Passport.
- [draft-wang-hjs-accountability](https://datatracker.ietf.org/doc/draft-wang-hjs-accountability/) — HJS: Accountability Receipts for AI Agents; minimal JEP profile for exportable AI receipts.
- [draft-wang-jac](https://datatracker.ietf.org/doc/draft-wang-jac/) — JAC: Declared Dependency Chains for Agent Receipts.
- Identity-bound audit-trail requirements also appear inside draft-klrc-aiagent-auth and draft-oauth-ai-agents-on-behalf-of-user (token records naming user, agent, and client).

**External protocols and industry**

- ACTA (Agent Communication Trust Architecture) receipts and AgentROA audit patterns.
- OpenID SSF/CAEP for continuous monitoring signals.
- Regulatory drivers: EU AI Act logging obligations; Colorado AI Act reasonable care standard.

---

## 9. Agent Transport and Connectivity

This category is defined by architectural role: transport substrate specifications intended to carry agent traffic, distinct from application-layer protocols that ride on existing transports (HTTP, QUIC).

**IETF venues**

- CATALIST coordination; <enterprise@ietf.org> (IoA@Enterprise); <aidc@ietf.org> (Data Center Networking for AI Clusters); <mcast4ai@ietf.org> (Multicast for AI); DNSOP (DNS-AID as connectivity bootstrap).
- PTTH BoF (IETF 123 and returning at IETF 126 with a proposed charter) — HTTP client-server role reversal; relevant to reverse connectivity to agents behind restrictive network positions.

**IETF work items**

- [draft-hood-independent-agtp](https://datatracker.ietf.org/doc/draft-hood-independent-agtp/) — AGTP core protocol: application-layer transport substrate for agents on IANA-registered port 4480; identity, sessions, semantic methods, authority scope, delegation chains, and attribution. Bindings and composition profiles are catalogued in Category 17.

**External protocols and industry**

- QUIC/HTTP3, WebTransport, WebSockets, and SLIM (see Category 14) as substrates that agent application protocols currently ride; MCP and A2A transport bindings.

---

## 10. Security and Threat Modeling for Agent Protocols

**IETF venues**

- AI Agent Security side meeting — IETF 125 (see Category 1).
- IETF 123 Hackathon: "Agent Protocol Security" project.
- Fragments in WEBBOTAUTH (impersonation), WIMSE and OAuth drafts (delegation security), and SAAG hallway discussion.

**IETF work items**

- [draft-baysal-asimov-safety-architecture](https://datatracker.ietf.org/doc/draft-baysal-asimov-safety-architecture/) — Asimov Safety Architecture for AI agents.
- [draft-berlinai-vera](https://datatracker.ietf.org/doc/draft-berlinai-vera/) — VERA: Verifiable Enforcement for Runtime Agents.
- [draft-feng-nmrg-ain-architecture](https://datatracker.ietf.org/doc/draft-feng-nmrg-ain-architecture/) — names the coordination-plane threat surface (capability-claim spoofing, routing-state poisoning, semantic namespace abuse and IC-OID hijacking, intent privacy leakage).
- [draft-hood-agtp-trust](https://datatracker.ietf.org/doc/draft-hood-agtp-trust/) — AGTP trust scores, package integrity verification, and governance binding; three Tier 1 verification paths (DNS-anchored, log-anchored, hybrid).
- [draft-messous-eat-ai](https://datatracker.ietf.org/doc/draft-messous-eat-ai/) — attestation as a security primitive (also see Category 1).
- [draft-mw-spice-intent-chain](https://datatracker.ietf.org/doc/draft-mw-spice-intent-chain/) — chains mapped to the STRIDE threat model (spoofing, tampering, repudiation, elevation of privilege).
- [draft-ni-a2a-ai-agent-security-requirements](https://datatracker.ietf.org/doc/draft-ni-a2a-ai-agent-security-requirements/) — security requirements for A2A-style agent interactions.
- [draft-stone-aivs](https://datatracker.ietf.org/doc/draft-stone-aivs/) — AIVS: Agentic Integrity Verification Standard.
- [draft-stone-swarmscore-v1](https://datatracker.ietf.org/doc/draft-stone-swarmscore-v1/) — SwarmScore V1: volume-scaled agent reputation protocol.
- [draft-stone-swarmscore-v2-canary](https://datatracker.ietf.org/doc/draft-stone-swarmscore-v2-canary/) — SwarmScore V2 Canary: safety-aware agent reputation protocol.
- [draft-westerbeck-reason-protocol](https://datatracker.ietf.org/doc/draft-westerbeck-reason-protocol/) — reason:// URI scheme and registry protocol for validated agent reasoning artifacts.
- Security Considerations sections of the drafts throughout this document.

**External protocols and industry**

- OWASP Top 10 for Agentic Applications (2026).
- Vendor threat-modeling work around MCP tool poisoning and injection.

---

## 11. Agentic Commerce (Transactions, Merchant Identity, Payments)

**IETF venues**

- No dedicated venue or BoF. Discussion threads in agent2agent and CATALIST; potential overlap with payments-adjacent work.

**IETF work items**

- Commerce-adjacent primitives inside [draft-rehfeld-apix-core](https://datatracker.ietf.org/doc/draft-rehfeld-apix-core/): commercial onboarding, sanctions compliance, and a supply-side funding model for agent-consumable services.
- [draft-hood-agtp-commerce](https://datatracker.ietf.org/doc/draft-hood-agtp-commerce/), [draft-hood-agtp-merchant-identity](https://datatracker.ietf.org/doc/draft-hood-agtp-merchant-identity/), [draft-hood-agtp-lei](https://datatracker.ietf.org/doc/draft-hood-agtp-lei/) — AGTP commerce substrate: merchant identity primitives, Legal Entity Identifier binding, transaction support via the PURCHASE method and TRANSACT verb category.
- [draft-sharif-agent-payment-trust](https://datatracker.ietf.org/doc/draft-sharif-agent-payment-trust/) — trust scoring and identity verification for AI agent payment transactions.
- [draft-stone-vcap](https://datatracker.ietf.org/doc/draft-stone-vcap/) — VCAP: Verified Commerce for Agent Protocols.
- [draft-stone-vcap-ap2-binding](https://datatracker.ietf.org/doc/draft-stone-vcap-ap2-binding/) — VCAP-AP2 Binding: verified commerce settlement for the Agent Payments Protocol.
- WEBBOTAUTH is the closest chartered dependency (agent verification for commerce flows).
- [draft-skyfire-oauth-kyapay-token Defines](https://datatracker.ietf.org/doc/draft-skyfire-oauth-kyapay-token/) KYAPay Token
- [draft-skyfire-oauth-using-kyapay-tokens](https://datatracker.ietf.org/doc/draft-skyfire-oauth-using-kyapay-tokens/) (with Akamai) Overview on how to use KYAPay tokens
- [draft-skyfire-oauth-kyapay-token-exchange](https://datatracker.ietf.org/doc/draft-skyfire-oauth-kyapay-token-exchange/) (with Okta and Ory) Describes exchanging KYAPay token for an OAuth access token (e.g., for MCP)
- [draft-skyfire-oauth-amr-values](https://datatracker.ietf.org/doc/draft-skyfire-oauth-amr-values/) (with Akamai and Experian) Defines additional Authentication Method Reference (“amr”) claim values
- [draft-skyfire-oauth-id-verification](https://datatracker.ietf.org/doc/draft-skyfire-oauth-id-verification/) (with Akamai and Experian) Defines Identity Verification Methods claim and values
- [draft-skyfire-oauth-aml-methods](https://datatracker.ietf.org/doc/draft-skyfire-oauth-aml-methods/) (with Experian) Defines Anti-Money Laundering Methods claim and value

**External protocols and industry**

- AAIF — Linux Foundation Agentic AI Infrastructure Foundation (founding members include AWS, Anthropic, Block, Bloomberg, Cloudflare, Google, Microsoft, OpenAI).
- ANP community roadmap includes an AP2 agent payment protocol at its application layer.
- Visa TAP and Mastercard Agent Pay — adopting Web Bot Auth as their agent-verification foundation.

---

## 12. AI Content Preferences (Opt-outs, Training Data Control)

**IETF venues**

- AIPREF WG (chartered January 2025; first met at IETF 122 Bangkok).
- Active WG list: <aipref@ietf.org>. The <ai-control@ietf.org> list was the pre-WG and workshop-era list.
- Origin: IAB AI-CONTROL workshop (September 2024).

**IETF work items**

- [draft-ietf-aipref-attach](https://datatracker.ietf.org/doc/draft-ietf-aipref-attach/) — attachment via robots.txt extensions, HTTP headers, Well-Known URIs.
- [draft-ietf-aipref-vocab](https://datatracker.ietf.org/doc/draft-ietf-aipref-vocab/) — vocabulary for AI-usage preferences.

**External protocols and industry**

- Liaisons and adjacent work: IPTC, PLUS Coalition, WHATWG/W3C, Common Crawl, publisher coalitions.

---

## 13. Bot and Crawler Authentication

**IETF venues**

- WEBBOTAUTH WG (chartered early 2026 following the IETF 123 BoF) and <web-bot-auth@ietf.org>. Charter liaises with AIPREF, HTTPBIS, OAUTH, TLS, WIMSE. Scope: authenticates the agent or bot to sites intended for humans; end-user auth and agent-to-agent and API auth are out of scope.

**IETF work items**

- [draft-meunier-web-bot-auth-architecture](https://datatracker.ietf.org/doc/draft-meunier-web-bot-auth-architecture/) — core architecture (HTTP Message Signatures per RFC 9421, Signature-Agent header, key directory).
- [draft-meunier-webbotauth-registry](https://datatracker.ietf.org/doc/draft-meunier-webbotauth-registry/) — registry and signature agent card.
- WG milestones: standards-track auth and bot-information specs to IESG April 2026; key-management and deployment BCP August 2026.

**External protocols and industry**

- Adopted as the verification foundation for Visa TAP and Mastercard Agent Pay (see Category 11).
- Cloudflare (originator), Google, Amazon, Akamai, OpenAI, Vercel, Shopify, Stytch implementations.

---

## 14. AI for Network Management, Operations, and Agent Networking

**IETF venues**

- IRTF NMRG and <ainetops@ietf.org>; OPS-area discussions.
- IRTF T2TRG — agentic operation of IoT and constrained environments.

**IETF work items**

- AINetOps draft ("AI for Network Operations") circulated on agent2agent.
- ANP network requirements; IPv6-for-IoA capability-requirements discussions.
- [draft-akhavain-moussa-ai-network](https://datatracker.ietf.org/doc/draft-akhavain-moussa-ai-network/) — AI Network for Training, Inference, and Agentic Interactions.
- [draft-an-nmrg-i2icf-cits](https://datatracker.ietf.org/doc/draft-an-nmrg-i2icf-cits/) — agentic interface to in-network computing functions for software-defined vehicles in cooperative intelligent transportation systems.
- [draft-bernardos-nmrg-agentic-network-optimization](https://datatracker.ietf.org/doc/draft-bernardos-nmrg-agentic-network-optimization/) — solutions for enabling agentic sensing with network optimization.
- [draft-chen-nmrg-multi-provider-inference-api](https://datatracker.ietf.org/doc/draft-chen-nmrg-multi-provider-inference-api/) — multi-provider extensions for agentic AI inference APIs.
- [draft-chuyi-nmrg-agentic-network-inference](https://datatracker.ietf.org/doc/draft-chuyi-nmrg-agentic-network-inference/) and [draft-chuyi-nmrg-ai-agent-network](https://datatracker.ietf.org/doc/draft-chuyi-nmrg-ai-agent-network/) — agentic network architecture and protocol for agent interconnection and multi-level inference.
- [draft-cui-nmrg-llm-benchmark](https://datatracker.ietf.org/doc/draft-cui-nmrg-llm-benchmark/) — framework to evaluate LLM agents for network configuration.
- [draft-du-catalist-routing-considerations](https://datatracker.ietf.org/doc/draft-du-catalist-routing-considerations/) — routing considerations in agentic networks.
- [draft-feng-nmrg-ain-architecture](https://datatracker.ietf.org/doc/draft-feng-nmrg-ain-architecture/) — Agentic Intent Network (AIN); routing-based architecture for AI agent coordination: Intent Datagrams, Intent Routers, Capability Routing Tables, IC-OID semantic identifiers, Agent Domains.
- [draft-hong-nmrg-agenticai-ps](https://datatracker.ietf.org/doc/draft-hong-nmrg-agenticai-ps/) — problem statement for agentic AI in network management.
- [draft-irtf-nmrg-ai-challenges](https://datatracker.ietf.org/doc/draft-irtf-nmrg-ai-challenges/) and [draft-irtf-nmrg-ai-deploy](https://datatracker.ietf.org/doc/draft-irtf-nmrg-ai-deploy/) — NMRG baseline documents.
- [draft-jadoon-nmrg-agentic-ai-autonomous-networks](https://datatracker.ietf.org/doc/draft-jadoon-nmrg-agentic-ai-autonomous-networks/) — architectural principles for agentic augmentation of the IP suite.
- [draft-jimenez-t2trg-iot-agent](https://datatracker.ietf.org/doc/draft-jimenez-t2trg-iot-agent/) — agentic AI operation of constrained RESTful (CoAP) environments.
- [draft-mpsb-agntcy-messaging](https://datatracker.ietf.org/doc/draft-mpsb-agntcy-messaging/) — overview of messaging systems and their applicability to Agentic AI.
- [draft-song-anp-aip](https://datatracker.ietf.org/doc/draft-song-anp-aip/) (Agent Internet Protocol) and [draft-song-anp-aitp](https://datatracker.ietf.org/doc/draft-song-anp-aitp/) (Agent Internet Transport Protocol).
- [draft-wmz-nmrg-agent-ndt-arch](https://datatracker.ietf.org/doc/draft-wmz-nmrg-agent-ndt-arch/) — network digital twin and agentic AI based architecture for AI-driven network operations.
- [draft-yc-ipv6-for-ioa](https://datatracker.ietf.org/doc/draft-yc-ipv6-for-ioa/) — capabilities and future requirements of IPv6 for the Internet of Agents.
- [draft-zhang-cats-token-aware-ts](https://datatracker.ietf.org/doc/draft-zhang-cats-token-aware-ts/) — token-aware traffic steering solution for agent service.
- [draft-zhang-rtgwg-agent-policy-aware-network](https://datatracker.ietf.org/doc/draft-zhang-rtgwg-agent-policy-aware-network/) — use cases and requirements for AI agent policy-aware network.
- [draft-zhao-ccamp-actn-optical-network-agent](https://datatracker.ietf.org/doc/draft-zhao-ccamp-actn-optical-network-agent/) — integration of Network Management Agent into ACTN-based optical network.
- [draft-zhao-nmop-network-management-agent](https://datatracker.ietf.org/doc/draft-zhao-nmop-network-management-agent/) — AI-based Network Management Agent (NMA): concepts and architecture.
- [draft-zhao-nmrg-ai-agent-for-ndt](https://datatracker.ietf.org/doc/draft-zhao-nmrg-ai-agent-for-ndt/) — AI agent architecture for Network Digital Twin.

**External protocols and industry**

- ETSI ENI multi-agent studies; vendor AIOps platforms.

---

## 15. Coordination, Problem Framing, and Architectural Principles

Documents in this category inform how the community thinks about agent standards without themselves being protocol specifications. Includes cross-effort coordination, problem-space analyses, dimensional frameworks, and architectural principles.

**IETF venues**

- agentproto, DAWN, and DMSC BoF preparation threads; IETF-OPS-AD and AINETOPS GitHub tracking.
- CATALIST — where DMSC and related agent efforts presented.

**IETF work items**

- [draft-agentic-ai-usecases-requirements](https://datatracker.ietf.org/doc/draft-agentic-ai-usecases-requirements/) — use cases and protocol requirements for AGENTPROTO.
- [draft-farrel-catalist-ai4all](https://datatracker.ietf.org/doc/draft-farrel-catalist-ai4all/) — emerging applications of AI in IETF specifications.
- [draft-foroughi-agent-protocol-dimensions](https://datatracker.ietf.org/doc/draft-foroughi-agent-protocol-dimensions/) — dimensional model for characterizing agent protocols and their substrates.
- [draft-morrison-substrate-observation](https://datatracker.ietf.org/doc/draft-morrison-substrate-observation/) — architectural principle that concurrent heterogeneous sessions should observe a shared substrate rather than negotiate an envelope wire format.
- [draft-rosenberg-aiproto-framework](https://datatracker.ietf.org/doc/draft-rosenberg-aiproto-framework/) and strawman charter (feeding agentproto); see Category 3.
- [draft-scrm-aiproto-usecases](https://datatracker.ietf.org/doc/draft-scrm-aiproto-usecases/) — agentic AI use cases.
- [draft-teodor-pilot-problem-statement](https://datatracker.ietf.org/doc/draft-teodor-pilot-problem-statement/) — problem statement: network-layer infrastructure for agent communication.
- [draft-yao-catalist-problem-space-analysis](https://datatracker.ietf.org/doc/draft-yao-catalist-problem-space-analysis/) — analysis of the IETF-relevant problem space, candidate WG areas, and open-source coordination.
- [draft-yossif-agent-mandate-problem](https://datatracker.ietf.org/doc/draft-yossif-agent-mandate-problem/) — problem statement: binding executed action parameters to the constraint set a human signed before the agent acted; states requirements without proposing a mechanism.
- [draft-yossif-enrollment-problem](https://datatracker.ietf.org/doc/draft-yossif-enrollment-problem/) — problem statement: what enrollment must guarantee before a device-bound signature can be resolved to a named human; a dependency inherited by every profile in this space, stated as requirements without a mechanism. See also Category 1.

**External protocols and industry**

- AAIF (Linux Foundation) as the open-source coordination counterpart; W3C AI-adjacent community groups.

---

## 16. Agent Packaging, Manifests, Integrity Verification, and Unified Lifecycle Management

**IETF venues**

- No dedicated venue. Discussions surface in agent2agent, CATALIST, and discovery contexts; potential overlap with SUIT (software update manifests) and SCITT (artifact transparency) machinery. SCIM agent lifecycle drafts (Category 1) cover the provisioning slice.

**IETF work items**

- [draft-hood-independent-agtp](https://datatracker.ietf.org/doc/draft-hood-independent-agtp/) family (packaging and lifecycle profiles under development) — AGTP open agent package format (.agent and .nomo): Merkle-tree integrity, manifest verification, lifecycle management, cryptographic accountability chain, trust scores bound to packages, marketplace orchestration and discovery support.
- Closest reusable primitives: SUIT manifests, SCITT receipts, EAT attestation (Category 1).
- Discovery-side manifest fragments: [draft-gaikwad-woa](https://datatracker.ietf.org/doc/draft-gaikwad-woa/) host manifests; [draft-narvaneni-agent-uri](https://datatracker.ietf.org/doc/draft-narvaneni-agent-uri/) URI-based framework; ADP agent descriptions (ANP suite); Agntcy directory records; APIX Manifests (APM).

**External protocols and industry**

- OCI images and npm-style registries as the de facto packaging substrate for agent frameworks.

---

## 17. Agent Bindings and Composition

This category captures work that binds agent protocols to specific underlying transports, or that specifies composition profiles for combining agent-layer semantics with substrate-layer or transport-layer implementations.

**IETF venues**

- agentproto and DMSC BoF preparation threads; MoQ (Media over QUIC) coordination.

**IETF work items**

- [draft-hood-agtp-bindings](https://datatracker.ietf.org/doc/draft-hood-agtp-bindings/) — AGTP substrate profiling for composition with existing transports (HTTP, QUIC) and existing protocols.
- [draft-hood-agtp-composition](https://datatracker.ietf.org/doc/draft-hood-agtp-composition/) — AGTP composition profiles: agent group messaging protocols, external identity providers, and HTTP gateways.
- [draft-jennings-ai-mcp-over-moq](https://datatracker.ietf.org/doc/draft-jennings-ai-mcp-over-moq/) — Model Context Protocol and Agent Skills over Media over QUIC Transport.
- [draft-liu-agent-protocol-over-moq](https://datatracker.ietf.org/doc/draft-liu-agent-protocol-over-moq/) — Agent Protocol over MoQ.
- [draft-nandakumar-ai-agent-moq-transport](https://datatracker.ietf.org/doc/draft-nandakumar-ai-agent-moq-transport/) — MoQ transport for agent protocols.
- [draft-wang-lisp-ai-agent](https://datatracker.ietf.org/doc/draft-wang-lisp-ai-agent/) — using LISP as a network substrate for AI agent communication.

**External protocols and industry**

- MoQ (Media over QUIC) as the transport being bound to; MCP transport bindings from Anthropic; A2A transport bindings from AGNTCY.

---

## Additional Cross-Cutting or Minor Items

- **Containerless Wasm and Virtual Actor Runtimes** — runtime and execution rather than standards; informs Transport (Category 9) and Packaging (Category 16) discussions.
- **IRTF and Research Aspects** — beyond NMRG and T2TRG, general research interest in agentic systems.
- **Privacy-Preserving Interactions and Data Minimization for Agents** — cross-cutting (Identity, Communication, Commerce, Governance). Limited dedicated IETF focus. CHEQ's disclosure-avoiding credential entry is a concrete mechanism; draft-birkholz-verifiable-agent-conversations documents privacy risks of reasoning traces and metadata in audit records.
- **RCNS (Runtime Contract Negotiation Substrate)** — dynamic request-time agent-to-server contract negotiation (part of the AGTP work items in Category 3).
- **OT and ICS Agent Authority** — physical-world and industrial control agent authority. [draft-morrison-ot-command-authority](https://datatracker.ietf.org/doc/draft-morrison-ot-command-authority/) addresses this space.
- **Additional Morrison drafts covering adjacent architectural principles**: [draft-morrison-live-reference-resolution](https://datatracker.ietf.org/doc/draft-morrison-live-reference-resolution/), [draft-morrison-compute-location-gate](https://datatracker.ietf.org/doc/draft-morrison-compute-location-gate/), [draft-morrison-consent-settlement](https://datatracker.ietf.org/doc/draft-morrison-consent-settlement/).
- **Running code inventory** — IETF 123 hackathon (Agent Protocol Security), IETF 124 hackathon (T2TRG IoT agent), DNS-AID reference implementation, Web Bot Auth production deployments (Cloudflare, Google, Stytch), AGTP MCP-over-AGTP running implementation (github.com/nomoticai/agtp, mcp.nomotic.ai:4480).

---

## Contributions

Reviews, corrections, and additions to this document have been provided by:

- Bradd McBrearty, Chad Stephens, Scott Lipsig, Iman Schrock, Enrique Somoza, Vedh Krishnan, Ramesh Raskar, Kaliya Young

Additional contributions and corrections are welcome. Open an issue or pull request on the repository, or contact the author.
