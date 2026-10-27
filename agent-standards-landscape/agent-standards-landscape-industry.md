# Industry AI Agent Standards Landscape

**Author**: Chris Hood, <chris@chrishood.com>, <chris@nomotic.ai>  
**Version**: v.01 — 2026-08-10  
**Purpose**: Living document to track industry protocols, conventions, and consortium efforts for AI agents that operate outside formal SDO governance.

---

## How to read this document

This is a living catalog. It records agent-related protocols, conventions, and verification programs developed by companies and consortia outside (or ahead of) formal standards bodies. Listings are descriptive. Technical claims about specific efforts belong in their own specifications and repositories. Comparative claims between efforts are outside the scope of this catalog.

Industry efforts move faster than SDO processes and frequently precede them. A recurring pattern documented here: successful industry protocols migrate into neutral governance once adoption forces the question (MCP, AGENTS.md, and A2A all began as vendor efforts and now live at the Linux Foundation; they are cataloged there and only their origins are noted here). Efforts listed below remain under company or consortium control as of this version.

Each category uses three standard subsections where applicable:

- **Sponsors / venues** — the companies or consortia behind the effort.
- **Work items and specifications** — concrete deliverables.
- **Related / adjacent** — connected efforts, including SDO work the items reference.

Additions and corrections are welcome.

---

## 1. Agentic Commerce and Payments

**Sponsors / venues**

- Visa, Mastercard, Google, OpenAI with Stripe, Coinbase, PayPal, and partner networks.

**Work items and specifications**

- **Visa Trusted Agent Protocol (TAP)** — announced October 2025, developed with Cloudflare. Cryptographic verification allowing merchants to distinguish trusted agent traffic from other automated traffic at checkout, carrying agent identity and payment intent.
- **Mastercard Agent Pay** — announced April 2025. Agentic payment program built on tokenized credentials, introducing agent registration and transaction controls for agent-initiated payments.
- **Google Agent Payments Protocol (AP2)** — announced September 2025 with a large partner roster. An extension in the A2A lineage for payment authorization, mandates, and verifiable intent between agents, merchants, and payment networks.
- **Agentic Commerce Protocol (ACP)** — OpenAI and Stripe, September 2025. Checkout flow allowing agent purchases inside conversational surfaces, with merchant integration through Stripe rails.
- **x402** — Coinbase, May 2025. Revival of the HTTP 402 status code for machine-native payments, aimed at per-request stablecoin settlement between agents and services.

**Related / adjacent**

- AAIF agentic-commerce working-group formation at the Linux Foundation.
- Card-network dispute infrastructure (ISO 8583, acquirer reference numbers) that agent payment programs inherit.

---

## 2. Agent Verification and Bot Identity

**Sponsors / venues**

- Cloudflare and partner networks; browser and CDN operators.

**Work items and specifications**

- **Web Bot Auth / signed agents** — Cloudflare-led mechanism for agents to cryptographically identify themselves using HTTP message signatures, with a verified-bot program built on it. Notable as an industry effort that has produced companion IETF drafts, so it is cross-referenced in the IETF catalog.
- **Pay per crawl** — Cloudflare program using HTTP 402 responses to charge crawlers for access, creating a commercial layer on top of crawl-permission signals.

**Related / adjacent**

- IETF web-bot-auth drafts and HTTP message signature standards the mechanisms build on.
- aipref working-group vocabulary work at the IETF, which addresses preference expression while explicitly excluding the enforcement these programs supply commercially.

---

## 3. Content Licensing and Crawl Signals

**Sponsors / venues**

- RSL Collective and publisher coalitions; Cloudflare; individual publishers.

**Work items and specifications**

- **Really Simple Licensing (RSL)** — September 2025. Extends robots.txt with machine-readable licensing and compensation terms for AI use of content, with a collective-licensing organization behind it.
- **Content Signals Policy** — Cloudflare, September 2025. Managed robots.txt additions distinguishing search, AI input, and AI training uses.
- **llms.txt** — proposed 2024. Convention giving language models a curated, markdown entry point to a site's content, adopted organically across documentation sites.

**Related / adjacent**

- robots.txt (RFC 9309) as the substrate all three extend.
- aipref vocabulary work covering adjacent preference expression.

---

## 4. Registries and Academic-Industry Consortia

**Sponsors / venues**

- MIT Project NANDA and collaborating institutions.

**Work items and specifications**

- **Project NANDA** — MIT-led architecture for an "Internet of AI Agents": decentralized registry and index design, agent facts and resolution records, and a published paper series covering registry architecture, trust, and commerce layers. Functions as an academic-industry consortium producing implementable designs outside SDO process.

**Related / adjacent**

- AGNTCY at the Linux Foundation and DAWN at the IETF, addressing overlapping registry and discovery territory.

---

## 5. De Facto Framework Conventions

**Sponsors / venues**

- LangChain, Microsoft, CrewAI, and the broader framework ecosystem.

**Work items and specifications**

- Orchestration frameworks (LangGraph, AutoGen and its successors, CrewAI) whose internal conventions for tool schemas, agent state, and handoffs function as de facto standards for large developer populations.
- Vendor agent-manifest and configuration conventions that travel between tools through copying rather than specification.

**Related / adjacent**

- MCP tool schemas and AGENTS.md (now Linux Foundation) as the formalized descendants of this layer.

---

## Additional Cross-Cutting Notes

- **The graduation pattern**: MCP, AGENTS.md, and A2A each began as single-vendor efforts and moved to neutral governance within roughly a year of broad adoption. Industry origin followed by foundation stewardship is currently the most reliable path an agent protocol has taken to multi-vendor status — no agent protocol has yet completed the equivalent path through an SDO ballot process.
- **Payments fragmentation mirrors SDO fragmentation**: five payment-adjacent protocols launched within eighteen months (TAP, Agent Pay, AP2, ACP, x402), from five sponsor groups, with overlapping scopes and no shared transaction record between them. The pattern documented in the master index's findings reproduces here with commercial sponsors in place of working groups.
- **Enforcement lives here**: preference and permission vocabularies are being written in SDOs, while the mechanisms that make any of it binding — verification, payment, access denial — are shipping as commercial programs. The split matters: the binding layer is currently proprietary.
- **Relationship to SDO catalogs**: several efforts here produce companion SDO artifacts (Web Bot Auth's IETF drafts) or are expected to migrate (commerce protocols into AAIF working groups). Cross-references are maintained in both directions.

---

## Contributions

Corrections, additions, and updates are welcome. Open an issue or pull request on the repository, or contact the author.

*This document is intentionally parallel in structure and tone to the companion IETF, W3C, Linux Foundation, IEEE, and ISO/IEC AI Agent Standards Landscape catalogs.*
