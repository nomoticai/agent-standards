# ISO/IEC AI Agent Standards Landscape

**Author**: Chris Hood, <chris@chrishood.com>, <chris@nomotic.ai>  
**Version**: v.01 — 2026-08-10  
**Purpose**: Living document to track ISO/IEC and closely related national/international standards activity relevant to AI agents and agentic systems.

---

## How to read this document

This is a living catalog. It records current ISO/IEC work and closely associated national standards activity that addresses AI concepts, terminology, management systems, and agent interconnection. Listings are descriptive. Technical claims about specific standards belong in the official publications. Comparative claims between standards or with non-ISO efforts are outside the scope of this catalog.

ISO/IEC activity on modern LLM-era agent *protocols* is currently lighter than the concentration of work in IETF, Linux Foundation, IEEE, or W3C venues. The most mature ISO/IEC deliverables relevant to agents are foundational terminology and AI management-system standards. Significant complementary national work (notably the Chinese national standards series on Agent Interconnection) is included because it is frequently referenced in international discussions and may influence or align with future ISO/IEC activity.

Each category uses three standard subsections where applicable:

- **ISO/IEC venues / committees** — the formal homes.
- **Work items and standards** — published or active items.
- **Related / adjacent** — national standards or other SDO work that sits nearby.

Additions and corrections are welcome.

---

## 1. Foundational Concepts and Terminology

**ISO/IEC venues / committees**

- ISO/IEC JTC 1/SC 42 — Artificial Intelligence (primary home for AI terminology and concepts).

**Work items and standards**

- **ISO/IEC 22989:2022 — Information technology — Artificial intelligence — Artificial intelligence concepts and terminology**  
  Establishes terminology and describes concepts in the field of AI. Explicitly defines **AI agent** as an automated entity that senses and responds to its environment and takes actions to achieve its goals. Provides the baseline vocabulary used by many subsequent AI and agent-related discussions. Also covers related terms (AI system, AI component, trustworthiness, etc.).

**Related / adjacent**

- Other SC 42 terminology and conceptual frameworks that feed into management-system and trustworthiness standards.

---

## 2. AI Management Systems and Governance

**ISO/IEC venues / committees**

- ISO/IEC JTC 1/SC 42 and related management-system committees.

**Work items and standards**

- **ISO/IEC 42001 — Artificial intelligence management system**  
  Requirements for establishing, implementing, maintaining, and continually improving an AI management system. Provides an organizational governance frame that can be applied to systems that include or orchestrate agents. Focus is management-system controls rather than agent-to-agent protocols.
- Related trustworthiness, risk, and impact-assessment standards under SC 42 that organizations use when deploying agentic systems.

**Related / adjacent**

- National and sector guidance that maps ISO/IEC 42001 to agent-specific risks.
- Industry assurance programs that reference ISO 42001 alongside agent-specific technical controls.

---

## 3. Agent Interconnection (National Standards with International Relevance)

**ISO/IEC venues / committees**

- No dedicated ISO/IEC multi-part agent-interconnection series identified as of the current version of this catalog. Significant national work is recorded here because of its scale and potential influence on future international standardization.

**Work items and standards**

- **China national standards series — Artificial Intelligence — Agent Interconnection (GB/Z series, 2026)**  
  Seven-part series developed under the guidance of the Ministry of Industry and Information Technology with participation of more than 70 enterprises, universities, and research institutes. Approved as national standards governing interoperability among AI agents. Parts address:
  1. Overall architecture for intelligent agent interconnection.
  2. Encoding, assignment, and management of intelligent agent identity codes.
  3. Agent identity registration, accounts, credentials, and authentication.
  4. Agent capability description.
  5. Agent discovery.
  6. Interaction protocols.
  7. Architecture, processes, and data formats for external tool invocation.

  Collectively the series aims to provide a unified technical framework covering identity, description, discovery, interaction, and tool access so that agents can interoperate across platforms and scenarios.

**Related / adjacent**

- Parallel national or regional efforts that may later feed into ISO/IEC processes.
- Conceptual overlap with discovery, identity, and tool-invocation topics treated in IETF, IEEE, Linux Foundation, and W3C venues.

---

## 4. Related Evaluation, Trustworthiness, and Domain Work

**ISO/IEC venues / committees**

- SC 42 and sector-specific ISO committees.

**Work items and standards**

- Trustworthiness, bias, robustness, and transparency standards under SC 42 that apply to AI systems generally and therefore to agents.
- Domain-specific AI application standards (healthcare, industrial, etc.) that may constrain or reference agent behavior.
- Ongoing SC 42 work program items that may expand coverage of autonomous or multi-agent systems as the technology matures.

**Related / adjacent**

- IEEE benchmarking (P3777) and industrial agent projects.
- NIST AI Agent Standards Initiative and other national research/guidance efforts that interact with ISO/IEC processes.

---

## Additional Cross-Cutting Notes

- **Current emphasis**: ISO/IEC contributions most relevant to agents today are definitional (ISO/IEC 22989) and organizational (ISO/IEC 42001). Explicit multi-part protocol standards for agent identity, discovery, messaging, and tool invocation are more actively developed in other SDOs or as national standards.
- **National-to-international pathway**: Large national programs (illustrated by the Chinese Agent Interconnection series) often serve as inputs or parallel tracks that can later inform ISO/IEC work items.
- **Relationship to other SDOs**: Organizations implementing agents typically combine ISO/IEC management-system requirements with technical interoperability profiles from IETF, Linux Foundation (A2A/MCP), IEEE, or W3C. ISO/IEC provides the vocabulary and governance frame; other bodies more often supply the wire-level or application-level protocols.
- **Watch items**: Future SC 42 new work item proposals on multi-agent systems, agent identity, or agent interoperability would be natural extensions of the existing terminology and management-system base.

---

## Contributions

Corrections, additions, and updates are welcome. Open an issue or pull request on the repository, or contact the author.

*This document is intentionally parallel in structure and tone to the companion IETF, Linux Foundation, W3C, and IEEE AI Agent Standards Landscape catalogs.*
