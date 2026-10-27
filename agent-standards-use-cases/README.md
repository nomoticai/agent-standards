# Foundational Problem Statements for Agent Coordination

**Author:** Chris Hood
**Version:** 0.5 draft
**Status:** All fifteen documents (Transport §1 through Governance §13, Naming §14 as an open question, and Transactions §15) follow the shared solution-agnostic apparatus: a strongest-form skeptical case, observable present-day failures, properties without a common definition, prior art treated as constraint, an honest "what no solution can promise" account, explicit non-claims, and a declared dependency map connecting each document to its neighbors. §9 (Semantics) carries controlled empirical evidence for its central claim. New in this version: §15 (Transactions), the first document to arrive through the evaluation loop — a concern surfaced in evaluation, drafted in the suite's format, landed with a matching rubric supplement; and §5 (Trust) expanded with the certification instrument (the agent analog of the TLS certificate) as an explicit property, with the WebPKI's base tier joining its prior art. Feedback on any specific document is welcome.
**Purpose:** An opinionated framing of the problems that agent systems must solve for a coherent internet of agents to exist. Protocol-agnostic. One architect's view.

---

## Opening

The IETF landscape currently contains [over 230 individual submissions](../agent-standards-landscape/agent-standards-landscape-ietf.md) targeting AI agent standards. Most propose new protocols. Most tie those protocols to specific enterprise objectives, product lines, or organizational agendas. Few step back to ask what problems agent systems actually need solved, independent of which particular protocol solves them.

Beneath the volume of proposals sits a familiar problem from product development: solutions arriving before the design is clear. Every protocol author is building. Few have paused to establish what a coherent internet of agents actually needs, or whether the primitives everyone assumes are required are required at all. Cryptographic delegation formats, transparency-log integrations, and semantic method vocabularies all show up in drafts before the underlying questions have been examined.

This suite takes the earlier step, and takes it in a specific form: **problem statements, not solution proposals.** Each document asks three questions. What, precisely, has no common definition today — across agent stacks, across organizations, across the deployed prior art? What observably breaks right now because of that absence — not hypothetically, but in the multi-stack, cross-organization workflows teams are building this year? And what can no solution honestly promise — which tradeoffs (offline verification versus revocation, completeness versus privacy, meaning versus decidability) are permanent features of the territory that any proposal must take a defined position on rather than claim to escape?

The suite is opinionated. It reflects one architect's judgment about which problems matter, what the honest skeptical case is for each, and which use case best illustrates the need. Consensus documents converge on the least controversial framing, which is often the least useful. Readers will disagree with specific selections. Disagreement about which problems matter is a more productive conversation than disagreement about which protocol wins.

The suite is also protocol-agnostic. Multiple protocols may address the same problem; a single protocol may span several. Neither shape is inherently correct. The framing exists to make comparison tractable and to keep the conversation grounded in problems rather than solutions.

## The Structure of Each Document

Every document follows the same apparatus, and the apparatus is doing deliberate work:

1. **Scope, status, and terminology.** What the document claims, what it defers, and the boundary declarations against neighboring documents — stated up front so overlap disputes are settled before they start.
2. **The skeptical case, at full strength.** Each document arms its skeptic with the best available position — modern HTTP/3 and QUIC rather than 1999's HTTP, SPIFFE rather than "servers have IPs," Temporal rather than cookies, the capability-token literature rather than "tokens exist," "the API is English" rather than a strawman. A problem that survives its strongest skeptic is a real problem.
3. **Where the mismatch actually lies.** The skeptic's true points are conceded — usually the wire-level, single-trust-domain, or already-legislated ones — and the gap is relocated to where it actually is.
4. **What observably breaks today.** Reproducible, present-day failures, before any abstraction.
5. **Properties without a common definition.** The requirement set, framed as questions parties must be able to answer — never as features of a presupposed solution.
6. **Prior art, and what it constrains.** Every document confronts its own graveyard (UDDI, SPKI, proxy certificates, FIPA-ACL, WS-\*, CORBA, Namecoin, the micropayment dead) and its own living partial answers (SPIFFE, Macaroons, Matrix, Temporal, CloudEvents, KSI, EDI). The recurring finding: each individual requirement is met *somewhere* in prior art; no system provides the combination, and the graveyard is proof of difficulty, not unreality. Prior art also sets the bar for solutions: state your delta against the nearest existing system, or be presumed to be it.
7. **The core use case**, restructured as: what breaks today / what any adequate answer must make possible / **what no answer can promise** / open questions. The no-promise block is the suite's signature honesty device — each document names the tradeoff curves it cannot escape and demands a defined position on them instead.
8. **Additional needs**, stated as observable failures, with ownership tags routing each item to the document that owns it.
9. **What this document does not claim.** Explicit non-claims, closing the over-reading a hostile reviewer would otherwise perform.
10. **Forward and backward dependencies.** Every handoff between documents is declared on both ends — see "The Seam Architecture" below.

## The Fifteen Documents

1. **Transport (TPT)**: what carries the coordination — and what session-level primitives have no common definition at any layer?
2. **Identity (IDY)**: what must agent identity provide beyond solved workload identity?
3. **Authorization (AZN)**: what must a receiver be able to verify at the point of action?
4. **Delegation (DGN)**: does a live mandate stand behind this valid credential?
5. **Trust (TRU)**: on what evidence does a relying party decide, given that judgment stays local?
6. **Discovery (DSC)**: how are candidates found when the requester has a need, not a name?
7. **Communication (CMM)**: what must any single message make visible, verifiable, and dispatchable?
8. **Session (SSN)**: what is the durable, multi-party, cross-organization coordination context?
9. **Semantics (SEM)**: what shared meaning do coordination acts carry between strangers?
10. **Tools (TOL)**: do the first nine primitives extend uniformly across the tool/agent line? (A participant-model claim, not a tenth primitive — the suite's first composition test.)
11. **Commerce (CMC)**: what coordination and evidence layer does value transfer require, composing with payment rails?
12. **Audit (AUD)**: what evidence properties do agent actions require — and what can no record attest?
13. **Governance (GOV)**: who is accountable in a form legal systems can act on — and who governs the substrate itself? (The suite's deliberate open ending.)
14. **Naming (NAM)**: how do humans refer to agents, given that no security property may ever depend on a name? (An open question, set apart from the primitives; possibly not a primitive at all.)
15. **Transactions (TXN)**: what names the unit of work — the engagement, its actions, and the receipt — so every participant and every later stranger references the same thing? (The first document produced by the suite's own feedback loop; verdict-neutral by construction.)

## Summary Table

| # | Document | Core problem (one sentence) | Core use case (one sentence) |
|---|----------|------------------------------|------------------------------|
| 1 | Transport | Modern carriers (HTTP/2–3, QUIC) scope every primitive to one connection between two endpoints, while coordination sessions span multiple participants, carriers, organizations, and days — and the coordination primitives themselves (session identity, attribution, control propagation) have no common definition at any layer. | A trip booking across four participants where "cancel the hotel, keep the flight" must propagate with defined — not assumed — semantics, including when the cancellation loses its race against an irreversible charge. |
| 2 | Identity | Workload identity is solved within a trust domain (SPIFFE-class); what nothing provides in combination is subject definition for agents, principal binding, cross-organization verifiability, agent lifecycle semantics, and message-granularity binding. | Organization B verifying that a message truly comes from organization A's specific assistant agent — including after A's infrastructure changed over the weekend — with no shared IAM. |
| 3 | Authorization | Chained, attenuable, offline-verifiable authority has been built repeatedly (SPKI, proxy certificates, Macaroons, Biscuit, UCAN) and never adopted; deployed practice runs on bearer tokens and over-provisioned standing credentials, and even the known constructions miss effect-time validity, consumable cross-hop bounds, action-instance binding, and the confused-deputy threat model. | The $500/month travel budget: after-the-fact reconciliation is solvable by verifiable chains; at-decision-time enforcement of the cumulative cap is provably not solvable by any offline credential — the requirement is a named enforcement point with a defined exposure window. |
| 4 | Delegation | A valid credential and a live mandate are different facts, and receivers cannot tell them apart — the mandate exists today as prompt text, outside every verification, audit, and revocation mechanism, and purpose-scoped authority has no evaluation procedure or dispute path anywhere. | An executive's two-week absence mandate — bounded approvals, a springing condition, one permitted sub-delegation — none of it verifiable, revocable, or disputable by anyone today. |
| 5 | Trust | Trust is a relation (relying party, agent, action class, context), never a property the agent carries; the substrate-shaped gap is portable, attested, action-class-differentiated, freshness-bounded, reset-resistant *evidence* — judgment stays local, and no global score is proposed or wanted. | Choosing among twenty tax-preparation claimants when verified and invented claims render identically at the moment of decision. |
| 6 | Discovery | The dominant patterns — name-centric, publishing-centric, domain-anchored, static-endpoint — leave capability-based lookup, query-time attribute filtering, and identity-first resolution unserved; and no index architecture (centralized, federated, distributed, crawled) escapes the operator question, only answers it differently. | Finding a legal Japanese-to-English translator under price, timeline, and evidence constraints — starting from no name, no domain, and no prior relationship. |
| 7 | Communication | Message-level coordination properties — per-message attribution, lineage reference, an intent slot structurally distinct from content, modality binding, an inspectability partition — are private per-stack conventions; prose-as-interchange is in-band signaling, and prompt injection is its blue box. | A cross-stack booking conversation in which a poisoned payload carrying a booking instruction is structurally indistinguishable from the principal's own instruction. |
| 8 | Session | Durable execution (Temporal-class) solved the single-operator workflow; the logical cross-organization session — named by an identifier all participants share, resumable with currency re-checked, continuity-evidenced without trusting any single store, with saga-shaped partial-failure semantics — exists nowhere. | A week-long research task spanning two organizations, with nightly restarts, a full-day dormancy, and a sub-delegation revoked during the pause that resumption must surface rather than launder. |
| 9 | Semantics | The coordination-act layer (query, propose, accept, refuse, commit, and the session acts) has no common definition; bilateral contracts scale as the product of participants, per-message interpretation collapses the evidentiary record to prose — and controlled evidence (7,200 trials, four model families) shows intent-dense act names are a measured engineering variable (10–29 pp selection gains) whose absence also produces maximally *confident* wrong dispatch. | Two strangers completing a purchase with each step recognizable as its act without a prior contract, and the transaction record adjudicable rather than archaeological. |
| 10 | Tools | Not a tenth primitive but a participant-model claim: the tool/agent protocol partition is exactly where each earlier primitive breaks — attribution collapses to service credentials, lineage dies at invocation, discovery and audit fork — and the tool channel carries the ecosystem's live injection attacks (description poisoning, rug pulls, poisoned results). | A calendar tool, a partner organization's agent, and an email tool composed under one principal, one delegation chain, and one audit trail — instead of three stacks, three identity models, and three logs. |
| 11 | Commerce | Agent value transfer needs a coordination-and-evidence layer that composes with payment rails — machine-readable offers at discovery, verifiable authority at payment, dual-held agreement records, economics admitting machine volume — and the industry's in-flight protocol layers confirm each gap while fragmenting across rival stacks exactly as §1 predicts. | The same $500 budget at the transaction layer: the supplier verifies the mandate rather than just the card, both parties hold the signed record, and the cap's enforcement point is named instead of assumed. |
| 12 | Audit | Application logs are held by the audited party and transparency systems serve discrete claims; agent evidence requires per-agent attribution, preserved chains, tamper-evident sequence, selective disclosure, cross-boundary verifiability, and *defined coverage* — with the honest floor that a signed lie chains perfectly, records attest only the observable exchange, and completeness is purchased by regime, never by cryptography. | The blacklisted-supplier payment: proving the check ran before the payment — or is damningly absent — without exposing the agent's unrelated activity, and without the external service's cooperation. |
| 13 | Governance | Attribution stops at the substrate boundary; accountability requires a counterparty-checkable bind between agent action and a natural or legal person, with evidentiary properties recognition regimes can attach to — recognition being conferred by legislatures and courts, not produced by design — while the substrate's own stewardship questions (operators, vocabularies, attestors, mandates) are collected as the suite's deliberate open ending. | A court asking whether an agent had authority to bind its principal: the factual layer becomes record inspection; the risk of excess remains doctrine's question, as agency law has always had it. |
| 14 | Naming* | Humans cannot speak, remember, or adjudicate opaque identifiers; names are mutable, strictly non-load-bearing pointers resolving to identifiers — meaning at the name layer, uniqueness at the identifier layer, verification by the resolved binding, never by the string. | "Send this to Ted for legal review," spoken inside an organization running half a million agents, resolving to exactly one Agent-ID — or failing cleanly into disambiguation, never a guess. |
| 15 | Transactions | Every deployed identifier scheme is single-namespace (trace-context per deployment, order numbers per merchant, Message-ID without attribution), so participants name the shared unit privately, receipts are unilateral database keys, failed engagements vanish, and the unit keeps getting bundled with the verdict — no neutral layer owns it. | The disputed purchase: five actions, one transaction, two sessions, four parties including two strangers — where "does the charge match the terms" becomes a comparison of two receipted actions rather than cross-namespace archaeology. |

\*Naming is included as an open question rather than as a fourteenth primitive; see the note in §14 itself, which argues the point deliberately.

## How the Documents Compose

The documents interlock rather than merely stack. Reading in order (1 through 14) follows the dependency structure; the relationships below are the load-bearing ones.

**Carriage, message, and context are three layers with declared boundaries.** Transport (§1) owns carriage — connections, streams, priority, teardown, control propagation. Communication (§7) owns the message — what any single message must make visible and verifiable, independent of what carries it. Session (§8) owns the context — the durable object messages reference, which outlives every connection. The three boundaries are declared in all three documents, in both directions, because this triple is where earlier drafts overlapped most.

**Identity anchors everything, and accountability completes it.** Every primitive attaches to the identifier (§2); the accountable/trustworthy distinction drawn there runs through the suite — an authenticated identifier makes an agent traceable, evidence (§5) informs whether to rely on it, and only the governance bind (§13) makes anyone answerable.

**Authorization and delegation are two sides of one declared seam.** §3 owns the receiver's decision at the point of action; §4 owns the mandate — its creation, scope, transitivity, and life. The seam's sharpest exhibit is the matrix cell they share: a *valid credential with no live mandate* is the dominant real-world failure (injection, revocation, departed principals), invisible to any system that can only check credentials.

**Discovery produces claims; trust evaluates them; authorization acts on them.** §6 returns candidates and unverified claims; §5 owns the evidence layer between results and decisions; §3 owns the grant. All three consume §2 underneath.

**Semantics fills the slot communication reserved.** §7 requires an intent slot structurally distinct from content; §9 defines what legitimately fills it — the act layer — while deliberately disclaiming the content layer's meaning, on FIPA's evidence. §8's session states (suspend, resume, compensate, succeed) are expressible as acts by requirement.

**Tools is the composition test.** §10 makes no new claim; it demands that §§1–9 extend uniformly across the tool/agent line, and documents that the line is where they currently break — and where the live attacks are.

**Commerce is the primitives with money attached.** §11 inherits §3's budget trilemma at the layer where it gets expensive, carries §4's mandate to the payment act, and produces the dual-held records that are §12's raw material — while composing with, never replacing, payment rails.

**Audit is where the suite's receipts come due.** Nearly every document declares an audit seam, and §12 receives them all; the refrain running through the suite — *recorded, referenceable, disputable* — cashes out there or nowhere. Its own honest conclusion hands §13 the heaviest seam in the set: every audit analogy's integrity is purchased by an enforcement regime, not produced by a data structure.

**Transactions names the unit everything else references.** §15 sits numerically after the open question and architecturally between audit's records and governance's regime: it supplies the shared join keys §12's cross-boundary investigations currently lack, produces the dual-held receipt §11 claims as transaction evidence, and hands §13 the referent its decisions and disputes cite — the receipt is what the regime acts on; without the name, there is nothing to adjudicate. Its governing boundary is the suite's verdict-neutrality pattern at its purest: the layer names and receipts, and never evaluates. Against §7 and §8 the distinction is the phone call versus the purchase order number: carriage is communication, continuity is session, and the unit of account is here — one transaction may span sessions, and agents may communicate at length and transact nothing.

**Governance supplies force, and collects what it cannot supply.** §13 receives the regime question and owns the accountability bind. It also gathers the suite's accumulated backlog — the operator question in its five appearances, vocabulary stewardship, attestor accreditation, coverage mandates, commerce's regulatory collisions — and names it as a distinct, deliberately *unsolved* problem: the substrate's primitives require stewardship and enforcement functions no protocol layer can supply for itself. The suite ends on that open question by design.

**Naming rides above identity, non-load-bearing by rule.** §14's names resolve to identifiers; identifiers resolve (via §6) to endpoints; humans operate on names while every substrate property attaches to the identifier underneath — and no security, continuity, or audit property may ever depend on the string.

## Recurring Findings

Patterns that emerged independently across the documents, worth naming once:

- **Every requirement is individually met in prior art; no system provides the combination.** SPIFFE, Macaroons, Matrix, Temporal, SIP, DIDComm, EDI, KSI, CloudEvents each solve a piece; the graveyards (UDDI, SPKI, proxy certificates, FIPA, WS-\*, CORBA, Namecoin, the micropayment dead, X.500) mark where combinations were attempted. The unmet need is always the combination; the graveyard is proof of difficulty, not unreality.
- **The single-trust-domain pattern.** The deployed world has repeatedly solved each problem *inside one operator's boundary* — Unix for uniform invocation, Temporal for durable workflows, OpenTelemetry for causal tracing, cloud IAM for identity, service registries for discovery. The suite's territory begins exactly where the single operator ends.
- **The operator question recurs and is never escaped.** Who runs the substrate, the index, the session store, the hub, the record custody — centralized, federated, distributed, and crawled architectures answer it differently and all pay; every solution owes a position, none a transcendence.
- **Any common layer collapses N×M — including a conventions spec.** The quadratic-integration argument alone proves less than it appears to; the honest formulation everywhere is that the *primitives lack a common definition*, with the layer question left to solutions.
- **Evidence, not scores; judgment stays local.** The global trust score is retired throughout; what can be common is portable, attested, provenance-carrying evidence, consumed under each relying party's own policy.
- **Disputability, not decidability.** Purpose compliance (§4), content meaning (§9), and authority-excess allocation (§13) cannot be made objectively decidable; the achievable requirement, in each case, is a recorded, referenceable artifact both sides can be held to — the conclusion agency law reached by other means.
- **The receipt, never the verdict.** The unit layer (§15), the record layer (§12), and the accountability layer (§13) converge on one discipline: what can be common is the named unit, the attested record, and the referenceable artifact; judgment, enforcement, and consequence live above, with the parties and the regime. Proposals that bundle the identifier with the evaluation are how a governance industry and thirty audit drafts each redefined the unit privately.
- **Integrity is purchased by regime, not produced by data structures.** The notary has notary law; the flight recorder has a mandate and a criminal statute; recognition of signatures came by legislation with supervisory apparatus attached. The evidence layers can supply what a regime needs; they cannot supply the regime — which is why the suite ends where it does.

---

## Contents

- [§1. Transport](01-transport.md)
- [§2. Identity](02-identity.md)
- [§3. Authorization](03-authorization.md)
- [§4. Delegation](04-delegation.md)
- [§5. Trust](05-trust.md)
- [§6. Discovery](06-discovery.md)
- [§7. Communication](07-communication.md)
- [§8. Session](08-session.md)
- [§9. Semantics](09-semantics.md)
- [§10. Tools](10-tools.md)
- [§11. Commerce](11-commerce.md)
- [§12. Audit](12-audit.md)
- [§13. Governance](13-governance.md)
- [§14. Naming](14-naming.md)
- [§15. Transactions](15-transactions.md)

---

## Companion Documents

Three artifacts travel with the suite, each answering a question the problem statements deliberately withhold:

- **[Necessity Assessment](../agent-standards-evaluation/necessity-assessment.md).** The suite removed its verdicts by design; the assessment renders them, standalone and contestable — for each primitive, whether a standard is required at all, under one named test (the stranger test: standardize what strangers must interpret identically; let the market provide what parties choose between), with a precedent cited per verdict and disputes invited by PR against specific primitives.
- **[Evaluation Rubrics](../agent-standards-evaluation/agent-protocol-evaluation-rubrics.md).** Seventeen core criteria and per-primitive supplements for grading any draft's *treatment* of these problems — evidence over opinion, no composite scores, self-grading first. C2 enumeration uses the suite's stable identifiers (TPT-00 through TXN and onward), against the suite version cited in each evaluation.
- **[Landscape Catalogs](../agent-standards-landscape/agent-standards-landscape.md).** The authoritative census of active proposals across the primitives, per venue (IETF, W3C, Linux Foundation, IEEE, ISO/IEC, and more), with a master index carrying the cross-venue findings. Counts live in the venue files so they never date the analysis.

The reading order the three imply: the suite says what the problems are; the assessment says which require standards; the landscape says what exists; the rubric says how well any of it treats the problems. The loop closed for the first time with §15: an evaluation surfaced the concern, the assessment argued its necessity, the problem statement landed in the suite's format, and the rubric gained its supplement in the same round.

## How to Read and Use This Suite

The documents above are one architect's view, offered as a common frame for evaluating agent standards proposals. When reading a new draft, working-group scope, or BoF charter, the useful tests are now sharper than "which primitive does it address":

- **Which failures does it fix?** Each document leads with observable present-day breakage; a proposal should map to specific items on those lists, not to a primitive in the abstract.
- **What is its delta against the nearest prior art?** Every document names the closest existing or attempted system (SPIFFE, UCAN, Temporal, UDDI, FIPA, WS-\*, the in-flight commerce stacks). A proposal that cannot state its delta in a paragraph should be presumed to be the thing it resembles — including its fate.
- **Where does it stand on the named tradeoffs?** Offline verification versus revocation latency; cumulative bounds versus shared state; completeness versus privacy and erasure; meaning versus decidability; and the operator question. The documents establish that these cannot be escaped; proposals owe defined positions, and "our design avoids the tradeoff" is the reddest available flag.
- **Which seams does it respect?** The boundary declarations (carriage/message/context; check/mandate; act/content; attribution/accountability) exist so proposals can be placed precisely. A proposal that spans a seam should say so and honor both sides' constraints.
- **What does it not claim?** Every document models this discipline; proposals should too.

The suite is deliberately protocol-agnostic and deliberately opinionated, for the reasons stated in the opening: comparison should be tractable, and positions the author is prepared to defend are more useful than framings no one can object to.

## Feedback

Corrections, disagreements, and additions are welcome. Commentary is best framed as a specific claim about a specific document's failure modes, prior-art reading, or declared seams, so the framing can be refined iteratively rather than argued in aggregate. Additions to any document's failure or needs lists are explicitly invited; both lists everywhere are illustrative rather than exhaustive.

## A Note on Evidence

These are problem statements, and most of their claims are architectural. One is now empirical: §9's claim that the intent density of act encodings is an engineering variable rests on a controlled 7,200-trial benchmark across four model families (Hood, 2026), imported with its stated limitations. Where further controlled evidence exists for or against any document's claims, it is the most welcome category of feedback.
