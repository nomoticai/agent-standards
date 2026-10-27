# W3C AI Agent Standards Landscape

**Author**: Chris Hood, <chris@chrishood.com>, <chris@nomotic.ai>  
**Version**: v.01 — 2026-08-10  
**Purpose**: Living document to track W3C Community Groups, Working Groups, and related work items for AI agent and agentic standards.

---

## How to read this document

This is a living catalog. It records current W3C Community Groups, related Working Group activity, and work items that address AI agents, agent identity, agent communication, and web-native agent capabilities. Listings are descriptive. Technical claims about specific drafts or group outputs belong in those groups’ repositories and publications. Comparative claims between efforts are outside the scope of this catalog.

W3C work on agents is currently concentrated in Community Groups (lightweight, open-participation venues) rather than chartered Working Groups. These groups explore identity, declaration/assurance, semantic communication, and web-facing capability exposure. They frequently build on or reference core W3C technologies such as Decentralized Identifiers (DIDs), Verifiable Credentials (VCs), and Linked Data.

Each category uses three standard subsections where applicable:

- **W3C venues** — Community Groups and relevant Working Groups.
- **Work items and specifications** — concrete deliverables or stated scope.
- **Related / adjacent** — complementary W3C technologies or external efforts that the groups explicitly reference.

Additions and corrections are welcome.

---

## 1. Agent Identity and Credentials

**W3C venues**

- **Agent Identity Registry Protocol Community Group** — focused on verifiable AI agent identity infrastructure. Addresses how agents present cryptographically verifiable credentials that bind them to controlling organizations, enabling cross-organizational trust negotiation without pre-existing bilateral agreements.
- **Decentralized Identifier Working Group** — maintains the core DID specifications that multiple agent efforts reuse.
- Related discussion also appears in the AI Agent Protocol Community Group and Semantic Agent Communication Community Group.

**Work items and specifications**

- Scope of the Agent Identity Registry Protocol CG includes:
  - A DID method specification for agent identity resolution.
  - An agent credential format based on W3C Verifiable Credentials.
  - Trust negotiation protocol for cross-organizational agent interactions.
  - Trust-level definitions and verification requirements.
  - Integration profiles with complementary protocols (MCP, A2A, OAuth/OIDC, SPIFFE).
  - Revocation and credential lifecycle management.
  - Post-quantum cryptographic requirements for agent identity.
- Core DID Resolution and DID Core specifications (Candidate Recommendation / Recommendation track) that agent identity systems build upon.
- Verifiable Credentials data model and related suites used for agent credentials.

**Related / adjacent**

- OpenID Foundation AI identity work and IETF agent-identity drafts that reference or interoperate with DIDs/VCs.
- SPIFFE/SVID patterns frequently discussed alongside DID-based agent identity.

---

## 2. Agent Declaration, Assurance, and “Know Your Agent”

**W3C venues**

- **Agent Declaration and Assurance Community Group** (proposed / forming; public proposal June 2026) — focused on cryptographic, verifiable mechanisms for autonomous AI systems to declare identity, operational boundaries, and alignment constraints.

**Work items and specifications**

- Stated deliverables under discussion:
  - **Open KYA (Know Your Agent) Manifest Schema** — cryptographic JSON-LD metadata format for agents to disclose underlying models, ownership, software versions, and alignment parameters.
  - Graduated conformance profiles (e.g., Core, Deploy, Transact) mapping risk categories and acceptable guardrails to agent capability levels.
  - Runtime integrity and environmental bindings (TEEs, DID frameworks).
  - Reference architectures for third-party platforms to parse, verify, and enforce policies based on an agent’s declared manifest.

**Related / adjacent**

- Broader industry “Know Your Agent” discussions and regulatory pressure for agent accountability.
- Agent Identity Registry Protocol CG work on credentials and trust levels.

---

## 3. Agent-to-Agent Communication and Protocols

**W3C venues**

- **AI Agent Protocol Community Group** — mission to develop open, interoperable protocols that enable AI agents to discover, identify, and collaborate across the Web. Explores inter-agent communication, identity models, standardized metadata, and protocol interoperability with existing Web standards.
- **Semantic Agent Communication Community Group** — focuses on semantic structures that enable autonomous software agents to interoperate in a verifiable, accountable, and machine-interpretable manner.

**Work items and specifications**

- AI Agent Protocol CG scope:
  - Inter-agent communication protocols (discovery, intent/capability exchange, role negotiation, dynamic collaboration).
  - Agent identity models based on open standards.
  - Standardized metadata formats for capabilities, interfaces, goals, and states (web-native formats).
  - Protocol interoperability and best practices for leveraging existing Web protocols.
  - Draft community white paper and protocol specification under development (hosted on group GitHub).
- Semantic Agent Communication CG scope:
  - Formal ontology and semantic structures for Intent, Delegation, Capability, ExecutionRecord, and related constructs.
  - Identity and verifiability primitives using DIDs, Verifiable Credentials, signatures, and attestation models.
  - Semantic logs and conformance definitions.
  - Emphasis on protocol-agnostic semantic payload (usable over HTTP, WebSockets, or other transports).

**Related / adjacent**

- Linux Foundation A2A and MCP (frequently referenced as complementary or integration targets).
- ActivityPub / Fediverse patterns occasionally discussed as prior art for decentralized actor communication.

---

## 4. Web-Facing Capability Exposure (Browser / Website Agents)

**W3C venues**

- **Web Machine Learning Community Group** — home of WebMCP work.
- Broader Web platform groups that may later formalize related APIs.

**Work items and specifications**

- **WebMCP (Web Model Context Protocol)** — proposed web standard (Google + Microsoft involvement) that lets a website register callable tools an agent can invoke through a `navigator.modelContext`-style API. Shifts control so the website declares structured capabilities rather than forcing agents to reverse-engineer page interactions. Published as a Community Group draft (early 2026); public origin trial in Chrome reported.
- Goal: safer, structured agent interaction with ordinary websites (e.g., “book this appointment”) instead of brittle DOM automation.

**Related / adjacent**

- Existing Web platform APIs and permission models.
- Broader industry interest in “agent-readable” web surfaces.

---

## 5. Semantic Description, Ontologies, and Metadata

**W3C venues**

- Semantic Agent Communication Community Group (primary).
- AI Agent Protocol Community Group (metadata formats).
- Existing W3C Semantic Web / Linked Data infrastructure.

**Work items and specifications**

- Agent Semantic Communication Ontology (TTL, JSON-LD, SHACL) under development by the Semantic Agent Communication CG. Provides a shared vocabulary for agents, capabilities, and interaction rules built on DIDs and VCs.
- Standardized metadata formats for agent capabilities, interfaces, goals, and states (AI Agent Protocol CG).
- Reuse of JSON-LD, schema.org patterns, and Verifiable Credential vocabularies.

**Related / adjacent**

- Broader Linked Data and knowledge-graph ecosystems.
- Capability description approaches appearing in A2A Agent Cards and MCP tool schemas.

---

## 6. Trust, Verification, and Cross-Organizational Interaction

**W3C venues**

- Agent Identity Registry Protocol CG.
- Agent Declaration and Assurance CG.
- Semantic Agent Communication CG (verifiability primitives).

**Work items and specifications**

- Trust negotiation protocols and trust-level definitions (Agent Identity Registry Protocol CG).
- Runtime integrity bindings and policy-enforcement reference architectures (Agent Declaration and Assurance CG).
- Attestation models that bind agent identity to trusted execution environments.
- Integration profiles that map W3C identity/credential primitives onto MCP, A2A, OAuth/OIDC, and SPIFFE.

**Related / adjacent**

- IETF RATS, WIMSE, and OAuth agent-related drafts.
- OpenID Foundation AI identity work.
- Regulatory drivers (accountability, human oversight, auditability).

---

## 7. Core W3C Building Blocks Reused by Agent Work

These are mature or maturing W3C technologies that agent-focused Community Groups treat as foundational rather than inventing replacements.

- **Decentralized Identifiers (DIDs)** — DID Core, DID Resolution, and DID method infrastructure.
- **Verifiable Credentials (VCs)** — data model, securing mechanisms, and presentation protocols.
- **Linked Data / JSON-LD** — representation format for agent manifests, ontologies, and credentials.
- **Web Cryptography and related security primitives**.
- **HTTP, WebSockets, and emerging WebTransport** patterns used as underlying transports.

---

## Additional Cross-Cutting Notes

- **Community Group character**: Most current agent work lives in Community Groups. These can publish specifications and build consensus, but formal Recommendation-track standardization would require a chartered Working Group.
- **Web-native emphasis**: W3C efforts tend to stress DIDs, VCs, Linked Data, and browser/platform integration more heavily than pure network-layer or messaging-layer protocols.
- **Interoperability posture**: Groups frequently list explicit integration profiles with Linux Foundation projects (MCP, A2A) and IETF identity/authorization work.
- **Relationship to IETF and Linux Foundation work**: W3C contributions are strongest on identity credentials, semantic description, declaration/assurance, and web-facing capability exposure. Lower-layer transport, discovery registries, and high-scale messaging are more often addressed in IETF or Linux Foundation venues. Many of the same organizations participate across all three.

---

## Contributions

Corrections, additions, and updates are welcome. Open an issue or pull request on the repository, or contact the author.

*This document is intentionally parallel in structure and tone to the companion “IETF AI Agent Standards Landscape” and “Linux Foundation AI Agent Standards Landscape” catalogs.*
