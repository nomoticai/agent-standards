# Agent Standards Landscape

**Author**: Chris Hood, <chris@chrishood.com>, <chris@nomotic.ai>  
**Version**: v.01 — 2026-08-10  
**Purpose**: Master index for the per-venue landscape catalogs, and home for the findings only a cross-venue view can support.

---

## How to use this document

The individual venue catalogs are descriptive inventories. They list active work items, projects, and groups without comparative claims.

This master index is the only place in the series where comparative and cross-venue observations are made. Those observations are deliberately written so they can be checked against the venue catalogs. Disagreement with a finding should be directed here, not into the venue files.

---

## The catalogs

Each venue has its own living catalog, maintained in parallel structure:

- **IETF** — Active Internet-Drafts across the agent problem space, organized by primitive, plus BoF and working-group activity (agentproto, DMSC, AUDIT, WIMSE, DAWN, aipref, and related lists).

- **W3C** — Community Groups on agent identity, declaration and assurance, agent-to-agent protocols, semantic communication, and web-facing capability exposure (WebMCP), building on DIDs, Verifiable Credentials, and Linked Data.

- **Linux Foundation** — the Agentic AI Foundation (MCP, goose, AGENTS.md, agentgateway), the A2A Project, and AGNTCY, with commerce and security working groups forming.

- **IEEE** — Artificial Intelligence Standards Committee PARs on agentic frameworks (P3709), LLM agent interfaces (P3394), description-discovery-registry interoperability (P3931), industrial agent systems (P3945 series), and benchmarking (P3777), plus the historical FIPA suite.

- **ISO/IEC** — SC 42 foundational terminology (ISO/IEC 22989, which formally defines "AI agent") and AI management systems (ISO/IEC 42001), plus the Chinese national GB/Z Agent Interconnection series recorded for its international relevance.

- **ITU** — FG-TIDA (Focus Group on Trusted Identity for Digital Agents), first meeting December 2026, Paris. Catalog to be added after the inaugural meeting.

- **Industry** — payment-network protocols (Visa TAP, Mastercard Agent Pay, Google AP2, x402) and major vendor efforts outside SDO governance. Catalog to be added; current references appear in the Linux Foundation catalog and in the cross-venue findings below.

Counts of drafts, projects, and groups are maintained inside the individual venue files so this index does not date them.

---

## Cross-venue findings

These observations require the combined view. Each is checkable against the venue catalogs.

### 1. The same primitives are fragmenting the same way in every venue

Agent identity is the clearest case: approximately sixty identity-related Internet-Drafts at the IETF (WIMSE and adjacent), three W3C Community Groups (identity registry, declaration and assurance, plus the DID/VC foundation), AGNTCY's identity framework at the Linux Foundation, SPIFFE at CNCF, identity treatments embedded in IEEE interface and registry PARs, and FG-TIDA forming at the ITU. Six venues, one primitive, and no shared operational definition of the thing being identified — even though ISO/IEC 22989 has formally defined "AI agent" since 2022 and no venue's current work builds on that definition. A definition existing and going unused is a stronger indictment than a definition missing.

Discovery shows the same pattern: DAWN at the IETF, AGNTCY directories at the Linux Foundation, A2A Agent Cards, W3C protocol Community Groups, and IEEE P3931's full description-registry-discovery stack, all concurrent, none systematically cross-referencing.

### 2. The venues have divided labor without agreeing to

Linux Foundation projects ship runnable protocols and SDKs. W3C groups produce credentials, semantic description, and browser-facing surfaces. IETF drafts concentrate on identity, transport, and discovery primitives. IEEE formalizes interfaces, industrial profiles, and benchmarks. ISO/IEC supplies vocabulary and management systems. The division is real and nobody chartered it, which means the seams between layers belong to no venue, and the seams are where the interoperability failures live.

### 3. The same organizations sit in every room

The membership rosters of AAIF, the W3C agent Community Groups, and the active IETF drafts overlap heavily. Fragmentation is happening with the same participants in each venue, which rules out ignorance as the cause. The likelier cause is that each venue's tools shape what gets proposed there, and no venue's tools fit the layer underneath all of them.

### 4. Coordination artifacts exist in two places, and in no SDO-official one

The W3C AI KR Community Group maintains taxonomies, inventories, and vocabulary crosswalks from the W3C side. This repository maintains the problem-statement suite, the necessity assessment, the evaluation rubrics, and these catalogs. No SDO-official coordination body exists. The two unofficial efforts arrived at substantially the same findings independently, which is either corroboration or shared error, and is checkable.

### 5. The only coordinated substrate design so far is a national standard

The Chinese GB/Z Agent Interconnection series covers architecture, identity codes, registration and credentials, capability description, discovery, interaction protocols, and tool invocation as seven parts of one framework, developed by more than seventy organizations under ministry guidance. Agree or disagree with its choices, it is the single place where the agent substrate was designed as a whole rather than fragmented across venues. No international body has chartered the equivalent, and the venue catalogs above document the fragments.

### 6. The field is re-solving problems it already standardized once

The FIPA suite, now under IEEE, standardized agent communication languages, agent naming, and directory-based discovery in the late 1990s. Current proposals across all venues revisit the same territory, largely without citation. Whatever one concludes about FIPA's fit for LLM-era agents, a prior generation's complete stack existing unexamined is evidence about how this field treats its own history.

### 7. Everything rides HTTP, and each venue is separately rediscovering what that costs

Sessions became correlation headers at the IETF. Pauses became retry loops at the Linux Foundation (MCP `input_required`). Capability exposure became a browser API at the W3C (WebMCP). Message envelopes and session management became a Universal Message Format at IEEE (P3394). Four venues, four workarounds, one substrate assumption nobody chartered anyone to examine.

### 8. No venue owns a shared, location-independent addressing model

No venue has produced a stable, location-independent agent identifier that is both cryptographically verifiable and usable for routing. Canonical cryptographic IDs, DIDs, Agent Cards, registry entries, and DNS-based schemes all exist; none has become the common pivot that the other venues resolve against. As a result, "find the agent" and "reach the agent" remain separate problems solved differently in every room.

---

## Implication

The combined view shows that the agent substrate is being specified as a set of partially overlapping partial solutions. The seams between those solutions are currently owned by no venue.

---

## Contributions

Corrections, additions, and updates are welcome in any catalog. Open an issue or pull request, or contact the author. Venue catalogs stay descriptive; comparative and cross-venue claims live here, where they can be disputed as claims.
