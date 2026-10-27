# Necessity Assessment: What Requires a Standard

v0.3 · Aligned to the fifteen use case documents (01 through 15) and the evaluation rubric's primitive prefixes (TPT through TXN); each verdict heading carries the prefix used for C2 enumeration. Each primitive answers two different questions, and conflating them is how a landscape reaches 260 proposals: do agents need this, and does this require a standard. A capability can be essential to agents and still belong entirely to the market.

## The Stranger Test

Standardize what strangers must interpret identically. Let the market provide what parties choose between.

Could two parties who have never met, share no vendor, and hold no bilateral integration still interoperate? Whatever must be common for that to be true is the standard. Everything else is product, and products should compete.

The pattern recurs across internet history. Email standardized the identifier format (name@authority) and the resolution path; providers, policy, and features stayed market, and a hundred providers interoperate. Certificate Transparency standardized tamper-evident receipts; certificate authorities and monitors compete as businesses. Syslog standardized log carriage; every analysis product competes above it. Credit bureaus compete on scoring above a shared identifier and a shared reporting format. Thin standards, thick markets.

Each verdict cites the test and names a precedent. Disputes are invited by PR against a specific primitive: show a stranger-interoperation failure the verdict misses, or a standardized element the market demonstrably provides without one.

## Summary

| # | Primitive | Standard required | Thin core | Precedent |
|---|---|---|---|---|
| 01 | Transport (TPT) | Always standardized | The question is which | TCP, HTTP, QUIC |
| 02 | Identity (IDY) | Thin yes | Identifier format + verification | RFC 5322 addr-spec, X.509 |
| 03 | Authorization (AZN) | Thin yes | Carried claims + effect-time semantics | OAuth scopes |
| 04 | Delegation (DGN) | Yes, strongest case | Chain format, attenuation, verification | None complete; RFC 8693 single-hop, RFC 3820 abandoned |
| 05 | Trust (TRU) | Yes | Certification instrument + evidence interchange | WebPKI: X.509 standardized, CAs compete |
| 06 | Discovery (DSC) | Yes | Query semantics, result shape, integrity, liveness bound | DNS, for a different question |
| 07 | Communication (CMM) | Thin yes | Negotiation and framing | USB enumeration, RTP |
| 08 | Session (SSN) | No standalone | Folds into transport | MCP session removal |
| 09 | Semantics (SEM) | Thin yes, methods only | Method vocabulary + extension process | HTTP methods; Semantic Web as the failure |
| 10 | Tools (TOL) | No standalone | Distributes; thin descriptor remains | MCP as the over-standardized contrast |
| 11 | Commerce (CMC) | Thin at most | Budget and purchase authority | GS1/GTIN exist; reuse them |
| 12 | Audit (AUD) | Thin yes | Wire receipt: identity, action, transaction, signature | CT, syslog, SCITT, trace-context |
| 13 | Governance (GOV) | No | Consumes other primitives | None in SMTP or HTTP; that is the answer |
| 14 | Naming (NAM) | No | None; vanity layer | Domain renting |
| 15 | Transactions (TXN) | Yes | Action and transaction identifiers; the receipt | W3C trace-context |

## Per-Primitive Verdicts

**01 Transport (TPT): always standardized; the question is which.** Carriage admits no market alternative: competing wire formats is the non-interoperating world this suite documents. The honest counterargument is that agents already ride HTTP, so a dedicated transport fails strict necessity. The response is a question rather than a claim: HTTP was standardized for stateless document retrieval, and whether it is the right carriage for stateful agent interaction deserves evaluation rather than default. Every standalone session, correlation, and state proposal in the current landscape is evidence the evaluation is overdue.

**02 Identity (IDY): thin yes.** The email precedent is exact: RFC 5322 standardized the identifier format and resolution while providers, provisioning, and policy stayed market, and identity vendors interoperate today only because X.509, OIDC, and JWT sit beneath them. Standardizing an identity provider is impossible and unnecessary; standardizing the identifier format and the verification interface is what makes the provider market work. Identity influences every other primitive, which is why the thin core must exist even though everything above it competes.

**03 Authorization (AZN): thin yes.** Enterprises choose their engines; the standard is what the decision consumes: the claims carried with a request (scope, region, ownership, standing signals) in a form a stranger's enforcement point parses without bilateral setup, plus effect-time semantics so a once-valid grant admits nothing forever. OAuth proved the pattern: scopes standardized, policy engines competing.

**04 Delegation (DGN): yes, the strongest case.** Authority flowing human to agent to agent across organizational boundaries is definitionally stranger interoperation, and the precedent search comes back empty: RFC 8693 does single-hop inside one domain, RFC 3820 did attenuated chains and died with grid computing, macaroons did attenuation and were never standardized. Cross-organizational, multi-hop, attenuated, verifiable chains exist nowhere. This is the clearest open slot in the standards layer.

**05 Trust (TRU): yes.** Two standardizable cores. The certification instrument: agents carry validatable credentials the way sites carry TLS certificates, and the WebPKI is the exact precedent, since X.509 formats and validation rules are common, every relying party validates identically, certificate authorities compete above the standard, and Let's Encrypt showed the price of the baseline instrument falling to zero while the validation standard held. The evidence interchange: attested and asserted claims distinguished structurally, provenance and freshness carried with the evidence, and records following durable identity so standing survives re-hosting and resists re-minting. What stays market: judgment and scoring. The suite's trust statement retires the universal scalar, with the EV consumption failure as precedent, and competing evaluation methodologies above common instruments and common evidence is the bureau pattern operating as intended.

**06 Discovery (DSC): yes.** Run-time partner selection defines agent behavior, and the query one party writes must return results another party verifies and interprets with zero bilateral setup: query semantics, result shape, result integrity, and a declared liveness bound (presence is a property of discovery results rather than a peer primitive). DNS answers a different question, and agents are answers to attribute and capability questions rather than domain names; renting naming conventions is the enterprise layer this suite declines to standardize. Ranking methodology stays market, with disclosure.

**07 Communication (CMM): thin yes.** The USB analogy holds: when two authorized agents are present to each other, a standardized enumeration and negotiation (modalities, formats, framing) lets them communicate, while what flows stays theirs. USB standardized descriptors and device classes, never the devices; RTP standardized framing, never the conversation. Negotiation and framing are the thin core; content is application.

**08 Session (SSN): no standalone standard.** Correlation and continuity are carriage properties. If session must ride the transport as a header, session belongs in the transport; a standalone session standard is an admission about the transport underneath it. The largest deployed agent framework removing protocol-level sessions for scaling reasons is the pattern completing itself.

**09 Semantics (SEM): thin yes, methods only.** Two strangers need a shared action vocabulary, and the standard is the method catalog plus the governed process for extending it, full stop. Payload interpretation belongs to the models; taxonomies stay enterprise. The Semantic Web is the precedent for the thick version: RDF and OWL standardized meaning itself and produced two decades of unused elegance. Measured evidence supports method-level semantics specifically; the line stops there.

**10 Tools (TOL): no standalone standard.** Standardize finding tools (discovery), reaching tools (transport), and authorizing tools (authorization); never their use. What remains is a thin descriptor so a discovered tool is identifiable and its access requirements readable. The contrast case is instructive: current tool protocols standardize the use surface, which is the over-specification this suite argues against.

**11 Commerce (CMC): thin at most.** Product identity and pricing formats exist (GS1, GTIN, schema.org Product); inventing an agent SKU repeats solved work. The genuinely new pieces are budget authority and purchase authority: machine-readable spending bounds and purchase grants, verifiable across organizations, because no human reads the screen. Record formats may earn a thin standard; settlement, custody, and processing never do.

**12 Audit (AUD): thin yes.** The precedents are decisive. Syslog standardized carriage, SIEMs compete. Certificate Transparency standardized tamper-evident receipts, authorities and monitors compete. Trace-context standardized two propagated identifiers, every observability vendor competes. SCITT is the same move for supply chains now. The agent version: a wire receipt carrying identity number, action number, transaction number, and signature, attributable and tamper-evident because the parties who verify it (auditor, counterparty, regulator) are strangers to the system that produced it. Everything above the receipt (formats, retention, compliance workflow, runtime governance) is enterprise software.

**13 Governance (GOV): no.** Governance faces inward and consumes the other primitives: identity, authorization, delegation, and the audit receipt are its inputs, and with those standardized, every decision engine, policy language, and runtime can be enterprise software, which is why hundreds coexist. The checkable question settles it: what governance ships in SMTP or HTTP today? None, and email and the web governed themselves fine above the thin layers.

**14 Naming (NAM): no.** Agents discover by identifier, capability, and intent. Names serve humans; naming is vanity and market, and renting it is the business model this suite declines to standardize. The suite carries naming as an open question; this assessment renders the verdict the open question invites, and disputes route through the same PR path as every other verdict.

**15 Transactions (TXN): yes.** Now a full problem statement in the suite (15-transactions). Two agents connecting and doing something is a transaction; the tasks within it are actions. The standard issues identifiers and the receipt: action identifiers, a transaction identifier binding them, carried on every exchange, signed. The standard never decides whether the action should happen, evaluates it, or reviews it afterward; those are governance, authorization, and delegation consuming the receipt. Trace-context proved the pattern at the tracing layer; this generalizes it to accountable agent interaction. The receipt outlives the decision.

## What the Shape Says

Nine thin-yes verdicts and one always-standardized, and every yes is thin: identifier formats, verification interfaces, carried claims, chain formats, scales with attribution, query and result shapes, negotiation, method catalogs, authority records, receipts. Every thick thing (scoring methodologies, ranking, policy engines, governance products, naming, tool use, settlement, identity management) lands market-side. The standards layer agents require is thin and mostly missing; the market layer is thick and mostly what is being proposed. Much of the current landscape standardizes things that fail the stranger test by category, before their content is ever evaluated.
