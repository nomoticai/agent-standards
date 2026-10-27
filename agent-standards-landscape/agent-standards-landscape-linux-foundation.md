# Linux Foundation AI Agent Standards Landscape

**Author**: Chris Hood, <chris@chrishood.com>, <chris@nomotic.ai>  
**Version**: v.01 — 2026-08-10  
**Purpose**: Living document to track Linux Foundation venues, projects, and work items for AI agent and agentic standards.

---

## How to read this document

This is a living catalog. It records current projects, governance homes, and work items under the Linux Foundation related to AI agents. Listings are descriptive. Technical claims about specific projects belong in those projects’ repositories and specifications. Comparative claims between projects are outside the scope of this catalog.

The Linux Foundation has become a primary neutral home for several high-visibility agent interoperability efforts. The two most prominent organizational vehicles are:

- **Agentic AI Foundation (AAIF)** — founded December 2025; neutral home for MCP, goose, AGENTS.md, and related infrastructure.
- **Agent2Agent (A2A) Project** — donated by Google in June 2025; independent Linux Foundation project focused on agent-to-agent communication.

Additional contributions (notably AGNTCY from Cisco and others) sit alongside or interoperate with these efforts.

Each category uses three standard subsections where applicable:

- **Linux Foundation venues / projects** — the formal homes.
- **Work items and specifications** — concrete deliverables.
- **Related / adjacent** — closely connected industry or open-source efforts.

Additions and corrections are welcome.

---

## 1. Foundation Governance and Coordination

**Linux Foundation venues / projects**

- **Agentic AI Foundation (AAIF)** — launched 9 December 2025. Neutral stewardship for open agentic AI infrastructure. Founding contributions from Anthropic (MCP), Block (goose), and OpenAI (AGENTS.md). Platinum members have included AWS, Anthropic, Block, Bloomberg, Cloudflare, Google, Microsoft, and OpenAI. Membership has grown rapidly (reports of 190–200+ organizations by mid-2026).
- **Agent2Agent (A2A) Project** — launched under Linux Foundation governance 23 June 2025 (Open Source Summit North America). Independent project focused on the A2A protocol. Founding / early supporters included AWS, Cisco, Google, Microsoft, Salesforce, SAP, and ServiceNow. By its one-year mark (April 2026) it reported 150+ supporting organizations and production deployments.

**Work items and specifications**

- AAIF project intake and working-group formation (security, privacy, agentic commerce, and related domains reported as priorities).
- A2A Project governance, specification maintenance, and SDK ecosystem (Python, JavaScript, Java, Go, .NET).

**Related / adjacent**

- Broader Linux Foundation AI and cloud-native projects that agents consume (CNCF, SPIFFE/SPIRE, etc.).

---

## 2. Agent-to-Agent Communication

**Linux Foundation venues / projects**

- **A2A Project** (primary home).

**Work items and specifications**

- **Agent2Agent (A2A) Protocol** — open standard enabling agents built on different frameworks and by different vendors to discover one another, negotiate interaction modalities, and collaborate on tasks while remaining opaque about internal memory, tools, and logic. Transport is JSON-RPC 2.0 over HTTP(S); supports request/response, streaming (SSE), and asynchronous push. Agent discovery via “Agent Cards.” Specification reached v1.0 (production-ready) around March 2026. Repository and docs: a2a-protocol.org / GitHub a2aproject/A2A.
- Complementary positioning relative to MCP: A2A focuses on agent-to-agent coordination; MCP focuses on agent-to-tool/data connections.

**Related / adjacent**

- AGNTCY components that integrate with or implement A2A (Directory, Identity, SLIM Messaging, Observability).
- Enterprise production deployments reported across multiple industries by April 2026.

---

## 3. Agent-to-Tool and Data Connectivity

**Linux Foundation venues / projects**

- **AAIF** (primary home for MCP).

**Work items and specifications**

- **Model Context Protocol (MCP)** — Anthropic-originated protocol, contributed to AAIF. Universal standard for connecting AI models/agents to tools, data sources, and applications. Widely adopted; reported high monthly download volumes. Positioned as the agent-to-tool counterpart to A2A’s agent-to-agent focus.
- Supporting SDKs, server implementations, and ecosystem tooling under AAIF governance.

**Related / adjacent**

- OpenAPI and JSON-RPC ecosystems that MCP implementations commonly interoperate with.
- Framework-level MCP support across major agent runtimes.

---

## 4. Agent Discovery, Directories, and Identity Infrastructure

**Linux Foundation venues / projects**

- **AGNTCY** (contributed by Cisco and partners; Linux Foundation hosted).
- Components under AAIF and A2A ecosystems.

**Work items and specifications**

- **AGNTCY** — infrastructure aimed at an “Internet of Agents.” Provides discovery, identity, secure messaging (including SLIM — Secure Low-Latency Interactive Messaging), and observability. Explicitly designed to interoperate with A2A and MCP; directories can list A2A agents and MCP servers. Early members/partners included Dell, Google Cloud, Oracle, Red Hat, and others.
- A2A Agent Cards — capability and connection metadata used for discovery.
- Directory and identity frameworks within AGNTCY that support cross-vendor agent location and authentication.

**Related / adjacent**

- SPIFFE/SVID (CNCF) workload identity primitives frequently referenced in agent identity discussions.
- Enterprise directory and service-mesh patterns adapted for agents.

---

## 5. Agent Frameworks, Packaging, and Developer Guidance

**Linux Foundation venues / projects**

- **AAIF**.

**Work items and specifications**

- **goose** — Block-contributed open-source, local-first AI agent framework. Combines language models, extensible tools, and standardized MCP-based integration. Positioned as a practical starter kit / reference implementation under AAIF.
- **AGENTS.md** — OpenAI-contributed markdown convention that gives AI coding agents a consistent, repository-level source of project-specific guidance. Improves predictability of agent behavior across diverse codebases and toolchains. Supported by multiple coding-agent products.

**Related / adjacent**

- Broader open-source agent frameworks that adopt MCP and/or A2A.
- Packaging and manifest conventions emerging around these projects.

---

## 6. Gateways, Routing, and Observability

**Linux Foundation venues / projects**

- **AAIF** (agentgateway reported as a hosted project).
- AGNTCY observability and messaging components.

**Work items and specifications**

- **agentgateway** — described as routing and policy infrastructure built on A2A and MCP. Functions include routing agent-to-LLM and agent-to-microservice traffic, authentication enforcement, token-usage tracking, and inter-agent channel monitoring.
- AGNTCY SLIM Messaging and Observability frameworks.

**Related / adjacent**

- Existing service-mesh and API-gateway patterns being extended to agent traffic.

---

## 7. Security, Trust, and Governance

**Linux Foundation venues / projects**

- AAIF working groups (security and privacy reported as early priorities).
- A2A Project security considerations and production hardening.

**Work items and specifications**

- Security and privacy working-group activity under AAIF.
- A2A protocol security model (authentication, authorization, and opacity guarantees between agents).
- AGNTCY identity and secure-messaging primitives.

**Related / adjacent**

- CNCF security projects (SPIFFE, etc.).
- Industry threat-modeling work around MCP tool poisoning and agent supply-chain risks.
- Regulatory drivers (EU AI Act and others) that influence governance requirements.

---

## 8. Agentic Commerce and Transactions

**Linux Foundation venues / projects**

- AAIF (agentic commerce listed among emerging working-group areas).
- A2A extensions and related payment protocols referenced in the broader ecosystem.

**Work items and specifications**

- Early AAIF discussion of agentic commerce working groups.
- A2A as a foundation layer referenced by payment-oriented extensions (e.g., AP2 / x402-style work discussed in the wider industry).

**Related / adjacent**

- Visa TAP, Mastercard Agent Pay, and other commercial agent-verification efforts that sit outside pure Linux Foundation governance but interoperate with the same stack.
- Broader industry agent-payment protocols.

---

## Additional Cross-Cutting Notes

- **Complementarity narrative**: The dominant public positioning is that MCP (tools/data) and A2A (agent-to-agent) are complementary rather than competing; AGNTCY supplies discovery, identity, messaging, and observability that sit underneath or alongside both.
- **Governance style**: AAIF has emphasized a “build first, standards emerge from working implementations” approach rather than pure top-down specification.
- **Running code and adoption**: A2A reported production use and major-cloud integration by its first anniversary; MCP has large-scale download and integration metrics; goose and AGENTS.md provide concrete developer-facing artifacts.
- **Relationship to IETF and W3C work**: Many of the same organizations participate across Linux Foundation, IETF, and W3C efforts. Linux Foundation projects tend to prioritize runnable protocols and SDKs; IETF work often focuses on lower-layer identity, transport, and discovery primitives; W3C work emphasizes web-native identity, credentials, and semantic structures.

---

## Contributions

Corrections, additions, and updates are welcome. Open an issue or pull request on the repository, or contact the author.

*This document is intentionally parallel in structure and tone to the companion “IETF AI Agent Standards Landscape” catalog.*
