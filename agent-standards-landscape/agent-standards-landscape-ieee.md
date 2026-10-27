# IEEE AI Agent Standards Landscape

**Author**: Chris Hood, <chris@chrishood.com>, <chris@nomotic.ai>  
**Version**: v.01 — 2026-08-10  
**Purpose**: Living document to track IEEE Standards Association projects and related work items for AI agents and agentic systems.

---

## How to read this document

This is a living catalog. It records current IEEE projects (primarily under the Artificial Intelligence Standards Committee and related Computer Society activities) that address AI agents, agent interfaces, discovery, interoperability, benchmarking, and domain-specific agent systems. Listings are descriptive. Technical claims about specific projects belong in the official PAR pages and draft documents. Comparative claims between projects or with non-IEEE efforts are outside the scope of this catalog.

IEEE work tends to emphasize formal standards development through Project Authorization Requests (PARs), working groups, and balloting processes. Several projects target LLM-era agents; others continue longer-running industrial-agent and multi-agent system traditions (including the historical FIPA suite now under IEEE).

Each category uses three standard subsections where applicable:

- **IEEE venues / committees** — the formal homes.
- **Work items and projects** — active PARs and related deliverables.
- **Related / adjacent** — closely connected historical or domain work.

Additions and corrections are welcome.

---

## 1. Core Agent Frameworks and Interfaces

**IEEE venues / committees**

- C/AISC — Artificial Intelligence Standards Committee (IEEE Computer Society).

**Work items and projects**

- **P3709 — Standard for Framework and Technical Requirements of Agentic Artificial Intelligence (AI)**  
  Specifies a technical framework for Agentic AI, addressing commercial utilization and defining specific technical requirements. Active PAR (approved 2025-09-10). Working Group: Agentic AI.

- **P3394 — Standard for Large Language Model Agent Interface**  
  Defines natural language interfaces and protocols that facilitate communication between LLM applications, agents, and human users. Covers API syntax/semantics, conversational flow, prompt and chain-of-thought integration, authentication/authorization for LLM plugins, and related aspects. Establishes a Universal Message Format (UMF), message envelopes, semantic payload, agent roles, session management, and interaction patterns. Pre-draft / early development stage reported.

- **P3428 — LLM Agents for Education**  
  Specifies a modular agent architecture and lifecycle tailored to adaptive learning environments. Standardizes agent components, lifecycle states, and orchestration mechanisms to enable plug-and-play integration of multiple AI-driven agents in instructional systems.

**Related / adjacent**

- Broader AISC activity on AI systems engineering and evaluation.

---

## 2. Agent Description, Discovery, and Registry

**IEEE venues / committees**

- C/AISC — Artificial Intelligence Standards Committee.

**Work items and projects**

- **P3931 — Standard for Agent Description, Discovery, and Registry (ADDR) Interoperability**  
  Specifies a description model, registry mechanisms, and discovery protocols for intelligent agents operating across platforms, organizations, and ecosystems. Defines an agent metadata schema (machine-readable model for capabilities, input/output types, semantic and syntactic bindings, version/compatibility policies, runtime environment, resource constraints, and service bindings). Also defines discovery and matching protocols (querying, filtering, ranking, negotiating capabilities) and support for fallback/degradation strategies. Purpose includes enabling cross-ecosystem discovery, compatibility negotiation, and verifiable capability/SLA declarations.

**Related / adjacent**

- Discovery and directory work appearing in Linux Foundation (AGNTCY, A2A Agent Cards) and IETF DAWN-related efforts.

---

## 3. Agent-to-Agent Interoperability (General and Industrial)

**IEEE venues / committees**

- C/AISC and industrial/automation-related committees.
- Historical: FIPA standards under IEEE Computer Society.

**Work items and projects**

- **P3945 series — Industrial Intelligent Agent Systems**  
  - **P3945** — architecture for industrial intelligent agent systems; maps IEC 62264-compliant devices/systems to interoperable intelligent agents; addresses end–edge–cloud resources, generic agent models, role categories, interaction and data-flow patterns.  
  - **P3945.1 — Standard for Agent-to-Agent Interoperability in Industrial Intelligent Agent Systems** — profile enabling interoperability between industrial agents; baseline requirements for cross-vendor message exchange, capability advertisement and discovery, coordination patterns, and extensibility; interoperability levels and conformance testing for factory, edge, and cloud deployments.  
  - **P3945.2 — Standard for Agent-to-Tool and Data Access Interfaces in Industrial Intelligent Agent Systems** — reference interface model for agent-to-tool and data access; architectural principles, interface categories, and extension mechanisms for secure, interoperable, auditable interaction with external resources, tools, and control systems.

- **FIPA (Foundation for Intelligent Physical Agents) suite** — historical de-facto standards for agent communication (accepted as IEEE Computer Society standards committee activity). Includes Agent Communication Language (FIPA-ACL), agent names, and related interaction protocols still referenced in industrial and multi-agent system literature.

**Related / adjacent**

- Industrial multi-agent systems work in power/energy and automation domains.
- IEEE 2660.1 recommended practices on industrial agents (integration of software agents with low-level automation functions).

---

## 4. Benchmarking, Performance, and Evaluation

**IEEE venues / committees**

- C/AISC — Artificial Intelligence Standards Committee.

**Work items and projects**

- **P3777 — Standard for Benchmarking and Performance Metrics of Artificial Intelligence (AI) Agents**  
  Establishes a unified framework for benchmarking AI agents (autonomous, collaborative, and task-specific). Defines core performance metrics, evaluation protocols, and reporting requirements for transparent, reproducible, and comparable assessment. Supports multiple classification criteria and emphasizes objective measures. Dimensions may include efficiency, robustness, adaptability, ethical compliance, and interoperability options (without prescribing agent-to-agent interoperation itself). Active PAR (approved 2025-12-10). Working Group: 3777 — AI Agent Benchmarking and Performance Metrics.

**Related / adjacent**

- Evaluation and continuous-improvement themes appearing in related AISC projects (e.g., semantic information agents discussions).

---

## 5. Domain-Specific and Historical Multi-Agent Work

**IEEE venues / committees**

- Power & Energy Society Multi-Agent Systems Working Group and related industrial groups.
- Computer Society historical FIPA activity.

**Work items and projects**

- Ongoing application of FIPA-style agent communication in power systems and industrial automation.
- Recommended practices for integrating intelligent software agents with low-level automation (IEEE 2660.1 lineage).
- Domain papers and conference activity synthesizing P3394, P3428, and related agentic-system standards.

**Related / adjacent**

- IEC and other industrial standards that IEEE agent projects explicitly map to or interoperate with (e.g., IEC 62264).

---

## Additional Cross-Cutting Notes

- **Style of work**: IEEE projects typically proceed via PAR → working group → draft → ballot → published standard. Many of the agent-related PARs listed above are still in early or active development stages as of mid-2026.
- **Emphasis**: Strong presence in formal interface definition (message formats, roles, sessions), industrial interoperability, discovery/registry schemas, and benchmarking. Less emphasis (so far) on web-native identity credentials or pure network-layer transport compared with W3C or IETF efforts.
- **Relationship to other SDOs**: IEEE industrial agent work often references or maps to IEC standards. LLM-agent interface and discovery projects sit alongside Linux Foundation (A2A, MCP) and IETF identity/discovery drafts in the broader ecosystem. Many of the same organizations participate across venues.
- **Historical continuity**: The FIPA tradition provides a multi-decade baseline for agent communication languages and naming that continues to influence current industrial and research discussions.

---

## Contributions

Corrections, additions, and updates are welcome. Open an issue or pull request on the repository, or contact the author.

*This document is intentionally parallel in structure and tone to the companion IETF, Linux Foundation, and W3C AI Agent Standards Landscape catalogs.*
