# Agent Infrastructure Draft Evaluation Rubrics

A standardized, protocol-agnostic instrument for evaluating proposals
across the agent infrastructure landscape · v0.5 · July 2026

Companion to the Agent Infrastructure Problem Statements and the
Landscape Survey. This document supplies the third artifact: a common
rubric of seventeen core criteria and primitive-specific supplements
that any author, reviewer, or working group can apply to any draft,
including their own. 

---

## 1. Purpose and Neutrality Provisions

The landscape now holds proposals numbering in the hundreds across
the primitives, with heavy duplication and thin cross-referencing;
the Landscape Survey carries the authoritative count and this
document cites it rather than restating a number that dates. Review
capacity is finite. This instrument exists so that evaluation effort
is spent once, comparably, and in the open, rather than repeated
privately inside every effort.

The following provisions keep the instrument unbiased, and they are
requirements of use rather than suggestions:

1. **Evidence over opinion.** Every grade cites the section of the
   draft (or the artifact: repository, measurement, deployment) that
   justifies it. A grade without a citation is void.
2. **No composite scores.** Grades stay per-criterion. Aggregating
   into a single number or ranking is outside the instrument, because
   composite scores embed weighting choices this instrument refuses
   to make. Readers weight dimensions for their own purposes.
3. **Self-grading first.** Authors are the intended first users.
   A published self-grade with citations invites correction; a
   third-party grade uses the identical scale, so disagreement
   localizes to specific criteria and specific evidence.
4. **The scale measures maturity of treatment, never quality of
   idea.** A draft can treat a concern honestly at the Asserted
   level; the grade records where the treatment stands, so readers
   know what remains to be shown.
5. **Reverse coverage is mandatory.** Every evaluation answers the
   final section: does this draft address a real problem absent from
   the problem-statement suite? The suite is corrected by the
   drafts, in both directions.

## 2. The Evidence Scale

Every criterion in this document is graded on one four-level scale:

| Level | Test |
|---|---|
| **Absent** | The concern is unaddressed and unmentioned. |
| **Asserted** | The concern is claimed or discussed without normative treatment. |
| **Specified** | The concern has normative text: defined behavior, defined formats, defined failure handling. |
| **Demonstrated** | The specification is backed by running code, measurement, deployment, or formal analysis, publicly inspectable. |

The levels are ordered and objective: mentioned, claimed, written,
shown. Two refinements:

**The Demonstrated ladder.** Demonstrated is graded at one of four
rungs, each with a checkable test: **Demonstrated-simulated** (runs
only within an environment the author constructed; no external party
has reproduced it), **Demonstrated-reference** (a third party can
obtain the implementation and run it against real transport from
public instructions), **Demonstrated-interop** (two independently
authored implementations exchange traffic; the historical meaning of
running code), and **Demonstrated-production** (a reachable
deployment carries traffic from parties other than the author).
Claiming a rung above the evidence is a citable error. Simulation is
evidence and is graded as what it is.

**Annotations.** Where a criterion is out of scope by the draft's
own explicit statement, record **Scoped out** with the citation;
scoping out with a stated reason is legitimate design rather than
Absent, and a Scoped-out cell never lowers any other grade. Where a
facet is provided by another document in the same family, record
**Inherited** with the providing document cited; the evaluation unit
is the individual draft, and families may publish a facet-coverage
table mapping documents to provided facets so inheritance is
auditable rather than double-counted.

**Decision tests.** The two transitions that invite disagreement get
explicit tests. Asserted becomes Specified only when at least one of
the following is present and cited: normative requirements language
(MUST, SHOULD, MAY, or equivalent), a concrete data model or state
machine, or explicit failure and edge behavior; discussion and
intent stay Asserted. Any Demonstrated grade requires the artifact
location in the citation (repository with commit or tag, measurement
report, public endpoint with observation date, or interop report),
and the artifact must be publicly reachable without credentials at
evaluation time; a simulation harness that is itself private stays
below Demonstrated-simulated, which requires public reproduction
instructions. **Demonstrated-formal** (a machine-checked model or
proof) is a parallel evidence class rather than a rung: formal
verification and running code answer different questions, and an
evaluation records whichever the draft offers, or both.

**Inheritance rules.** A family claiming Inherited grades publishes
a living facet-coverage matrix (document by facet) cited in every
member evaluation. The matrix itself carries a version or date,
and every Inherited cell cites the matrix version it relied on, so
inheritance claims are timestamped and re-checkable. An Inherited
cell also records the providing
document's own grade on that facet, or notes that the provider is
ungraded. Inheriting is prohibited where the providing document
itself grades Absent or Asserted on the facet: a family cannot
inherit what no member has specified.

## 3. Core Rubric (seventeen criteria, applied to every draft, every primitive)

**C1. Problem statement.** Does the draft state the problem it
solves, independently of its solution? Test for Specified: the
problem statement admits solutions other than the draft's own.

**C2. Problem alignment.** Which problem statements from the suite
does the draft address? Enumerate by stable identifier. The scheme:
each suite section carries a three-letter prefix (given with each
supplement heading in §4); the section's core use case is XXX-00;
the additional use cases are XXX-01 onward in their listed order,
against the suite version cited in the evaluation header. This
criterion is descriptive enumeration only: zero matches carries no
negative weight and routes to the reverse-coverage section.

**C3. Layer justification.** Does the draft state which layer it
operates at and why that layer is correct for the concern? Test:
the draft names what it inherits from below and what it declines to
rebuild.

**C4. Prior and adjacent work.** Does the draft cite the adjacent
proposals in its primitive and state its relationship to each
(composes with, supersedes, overlaps, orthogonal)? An overlap
register listing the nearest five drafts is the Specified bar. The
same enumeration is required for deployed standards: which
established mechanisms (OAuth or GNAP, WIMSE, SCITT, COSE or JOSE,
DNS, ACME, or whichever apply) the draft reuses, profiles,
replaces, or declines, with the reason stated. C3 asks what the
design inherits from below; this criterion requires the
inheritance named, standard by standard.

**C5. Composability.** Are the draft's interfaces to other
primitives defined (what it consumes, what it emits, what it
requires present)? Test: another primitive's draft could cite this
one normatively without ambiguity. Drafts claiming to provide a
common coordination substrate or transfer layer additionally state,
per primitive they touch, whether that primitive becomes native to
their layer (defined once, present on every exchange) or remains a
convention above it (defined per stack) — either answer is
gradable; the unstated answer is the failing one.

**C6. Verifiability of claims.** For each property the design
asserts about participants (identity, capability, standing), is the
property self-asserted, third-party attested, or structurally
derived? The grade records the strongest class the design achieves
for its central claims.

**C7. Threat treatment.** Does the draft name what its mechanism
must resist and the mitigation for each named threat?

**C8. Failure and degradation.** Is behavior under partial failure
defined (staleness bounds, partition behavior, overload shedding,
recovery)?

**C9. Scale story.** Does the draft state its scaling regime and the
arithmetic or precedent behind it? Demonstrated requires measurement
or a deployed analog at the claimed order of magnitude.

**C10. Capture and rent surfaces.** Does the design create points
where an operator can extract rent from allocation, ranking, or
chokepoint position, and does the draft acknowledge and bound them?
This criterion grades acknowledgment and treatment, never intent.

**C11. Governance of vocabularies.** Where the design depends on
controlled vocabularies or registries, is the governance stated
(who curates, at what cadence, with what change discipline)?

**C12. Implementation evidence.** Graded on the Demonstrated
ladder. The tests, in order: can a party other than the author run
it; does it speak the specified wire rather than a simulation of it;
do two independently authored implementations interoperate; does a
reachable deployment carry third-party traffic. A repository
existing satisfies none of these by itself.

**C13. Assumption transparency and falsifiability.** Does the draft
state its load-bearing architectural assumptions, name the
deployment classes where its design fails or degrades, and record
the alternatives considered with reasons for rejection? This
instrument never grades whether a contested architectural position
is correct; correctness of contested architecture is decided by
consensus, informed by exactly this transparency. Two designs built
on opposed assumptions can both grade Specified here, and the
disagreement then localizes to the stated assumptions, where it
belongs. The failing grade belongs to the draft whose assumptions
must be reverse-engineered by its reviewers. Load-bearing
assumptions about what lives in the transfer layer versus above it
— state model, method semantics, identity placement, primary unit —
belong here whenever the draft depends on them, whichever side of
each axis it takes.

**C14. Operational and cognitive load.** Does the draft state the
prerequisites an implementer or operator must satisfy, and the
behavior when prerequisites are partially met? This grades the
treatment of complexity, never whether the complexity is excessive;
a heavy design that maps its own weight grades above a light design
that hides its assumptions. Operational treatment includes
observability: does the draft state how operators observe,
diagnose, and debug the mechanism in production (fault visibility,
health signals, tracing across organizational boundaries), the
dimension RFC 5706 asks of every protocol?

**C15. Privacy and minimization.** Beyond named threats, does the
design minimize what participants must reveal, retain, or make
correlatable, with visibility and retention bounds stated? Grades
the stated bounds and their enforcement location, on the same scale
as everything else.

**C16. Adoption path.** Does the draft state who deploys first,
what value exists for the earliest adopters (including at two
participants), and the incremental migration story from the
incumbent stacks it displaces, wraps, or coexists with (HTTP
conventions, MCP, A2A, or whichever incumbents apply)? This grades
the treatment of the bootstrap problem, never the
draft's market prospects; a design that maps its own cold start
honestly grades above one that assumes ubiquity, and a design that
can state concrete value for the second participant (or the first
pair) grades above one whose story begins at once-widely-deployed.

**C17. Internationalization.** Where the design carries names,
identifiers, or human-readable fields, are encoding,
normalization, and comparison rules stated (the IDNA and PRECIS
territory), with confusability handling where strings face humans?
Scoped-out with reason is the expected grade for designs that
carry no such fields.

## 4. Primitive Supplements

The supplements follow the problem-statement suite's canonical
numbering and resolve the provisional taxonomy carried in v0.2:
primitives §1 through §13, Naming §14 as an open question carrying
the same qualification it carries in the suite, Transactions §15 as
a problem-statement-backed addition, and one provisional supplement
pending a problem statement. Each supplement adds
criteria specific to the primitive, graded on the same scale. The
identifier prefix for C2 enumeration is given with each heading. A
crosswalk from the v0.2 numbering closes this section.

Supplements vary in density, and density is never a completeness
demand: not every draft must address every sub-criterion. Explicit
Scoped-out with a stated reason is preferred to silent omission;
the instrument grades treatment, never coverage of the whole
problem space, and a draft that maps what it declines to treat
grades above one that leaves the gaps to be discovered.

**Recurring terms**, defined once and used identically wherever
they appear across supplements. *Chain carriage*: the delegation
chain, from originating principal to acting agent, present on the
wire or in the record under evaluation. *Delegation lineage*: the
same chain read as provenance, identity plus every delegation hop.
*Offline verifiability*: verification at the point of use without
callbacks to issuers or third parties. *Effect-time validity*:
authority evaluated as still-valid at the moment of effect, with
revocation reaching the verifier. Where one normative mechanism in
a draft satisfies a recurring term across several supplements, the
same citation serves in every row that grades it: the mechanism is
specified once and cited many times, never re-derived per row.

### P1. Transport (prefix TPT)

The criteria below grade whether a draft takes and analyzes
positions on the transfer layer's design axes — never which
position it takes. A design that answers every axis with
"above the wire, by convention" reaches Specified by stating the
choice and analyzing the resulting reconstruction costs, exactly
as a design answering "native to the protocol" reaches Specified
by stating that choice and analyzing its scale, middlebox, and
adoption costs.

- Layer inheritance: what the design takes from existing transports
  (TCP, QUIC, WebTransport, HTTP as carrier) and what it adds,
  stated precisely?
- Primary unit and scoping: does the draft state whether its
  coordination unit is connection-scoped or session-scoped across
  participants and carriers — and define what happens when members
  of one coordination are reached over different underlying
  carriers?
- State model: does the transfer layer itself know that long-lived
  coordination state exists (sessions, subscriptions, presence as
  protocol facts), or is all state an application matter above a
  stateless wire, with the choice and its consequences (scale-out
  and middlebox costs on one side, per-stack reconstruction costs
  on the other) stated? What session state *contains* is graded
  under P8; this criterion grades the wire-level axis only.
- Method surface: does the transfer protocol's method vocabulary
  carry coordination acts, or only content and resource
  operations, with the choice and its consequences stated? The
  act vocabulary itself and its meaning are graded under P9; this
  criterion grades whether intent lives on the method surface at
  all.
- Identity placement: is the acting party addressable inside the
  protocol — the identifier as the element the protocol routes and
  addresses by — or present only as an annotation above it, with
  the choice and its consequences stated? Message-level
  attribution carriage is graded under P7 (CMM identity carriage
  and per-message attribution); one mechanism satisfying both is
  cited in both rows, per the recurring-terms rule.
- Control propagation semantics: are cancellation, revocation,
  priority, and partial teardown defined with cross-participant,
  cross-carrier ordering and visibility rules — including
  preemption of in-flight transfers flowing to *other*
  participants, and per-participant confirmation or known
  failure — or is propagation explicitly left to a higher layer,
  with the consequences stated? The multi-party, multi-carrier
  aspect is the graded substance; per-connection stream
  cancellation alone does not reach Specified here.
- Continuity carriage: does the transport carry session identity
  across pauses, process restarts, and cross-organization
  transitions, or delegate that to the session layer, with the
  split stated?
- Multi-modal streams: are concurrent streams (voice, video,
  structured data, text) treated as coordination primitives with
  defined interleaving, or as content payloads?
- Multiplexing: defined for the traffic patterns the design claims
  to serve?

Position crosswalk (v0.3 → v0.4): 1→1 (unchanged), 2→6
(generalized to control propagation), 3→7 (unchanged), 4→8
(unchanged), 5→9 (unchanged); positions 2–5 are new in v0.4.

### P2. Identity (prefix IDY)

- Permanence model: is the identifier stable under relocation,
  re-hosting, and credential rotation?
- Issuance model: allocated by an authority, derived from content,
  or self-generated, with consequences stated?
- Key lifecycle: rotation, revocation, and compromise recovery
  defined separately from identity continuity?
- Collision and conflict semantics: defined behavior when two
  parties present the same identifier?
- Accountability retrieval: can any party resolve an identifier to
  a verifiable accountability record?
- Algorithm agility: can cryptographic suites be rotated or
  replaced, including a stated post-quantum migration path,
  without identity discontinuity?

### P3. Authorization (prefix AZN)

- Scope model: is authority expressed in verifiable, bounded scopes
  carried with requests?
- Enforcement point: where is authority checked, and can
  enforcement be bypassed by path choice?
- Admission locality: does the final decision before an
  irreversible effect sit with the party bearing the consequence,
  applying its own requirements?
- Escalation resistance: can composition or chaining exceed the
  caller's authority, and is the check at evaluation or execution?
- Offline verifiability: can a receiver verify authority at the
  point of action without callbacks to issuers in the chain?
- Effect-time validity: for consequential operations, is authority
  evaluated as still-valid at the moment of effect, with revocation
  reaching the verifier, rather than a once-valid credential
  admitting indefinitely?
- Action-instance binding: is authorization bound to the specific
  action performed rather than a class, resisting replay and
  substitution of a different instance under the same evidence?
- Consumability: is authority bounded in quantity and decremented
  by use, so identical evidence cannot admit the effect twice?
- Chain lineage: is the causal chain of authority carried across
  organizational boundaries and verifiable end to end?
- Cumulative constraints: are aggregate bounds (budget caps, rate
  ceilings) expressible and checkable across many actions and many
  receivers?
- Algorithm agility: can credential and signature suites evolve,
  including post-quantum migration, without reissuing the
  authority base or breaking verification mid-chain?

Offline verifiability and effect-time validity pull in opposite
directions by design, and both are graded: a draft that chooses one
states the trade and the deployment classes where its choice holds
(per C13), and a draft that reconciles them (bounded validity
windows, revocation freshness proofs) cites the mechanism.

### P4. Delegation (prefix DGN)

- Chain carriage: is the delegation chain on the wire and in the
  record?
- Bounding: is the delegate's authority provably within the
  delegator's?
- Revocation: mid-chain revocation semantics defined, with
  propagation bounds?
- Purpose and temporal scoping: can a mandate be framed by purpose
  and time window rather than by enumerated actions, with receiver
  evaluation of the frame defined?
- Sub-delegation control: is the right to delegate itself
  delegable, boundable, and recorded?

### P5. Trust (prefix TRU)

- Claim classes: are verified, attested, and self-asserted signals
  distinguished structurally?
- Tiering: do verification levels exist with stated tests for each?
- Score provenance: who computes standing, from what evidence, with
  the subject excluded from authorship?
- Contamination bounds: can low-assurance identities affect
  high-assurance views?
- Portability: does standing carry across contexts and
  applications, with the carrying mechanism stated?
- Freshness: are staleness bounds on trust state defined and
  queryable?
- Certification instrument: where attestation is carried as a
  credential, are format, chain-to-issuer, and validation rules
  common, so strangers validate identically without bilateral
  setup? Offline verifiability applies per the recurring term.
- Issuer plurality: can multiple independent certifiers issue,
  with acceptance policy at the relying party and no single
  mandatory root? A single-root design reaches Specified by
  stating the dependence and analyzing it; a plural design by
  stating its acceptance and conflict handling.
- Certification revocation: is instrument status checkable with
  effect-time validity per the recurring term, distinct from
  identity revocation?
- Renewal separation: does credential rotation and issuer
  transition leave the subject's identifier and evidence history
  undisturbed (the credential rotates; the identity persists)?
- Algorithm agility: can certification suites evolve, including
  post-quantum migration, without invalidating held evidence?

### P6. Discovery (prefix DSC)

- Query model: by identifier, name, attribute, intent; which are
  structural versus approximate?
- Result integrity: are results signed, and by a party whose own
  standing is verifiable?
- Ranking determinism: is ranking reproducible from stated inputs,
  and are self-asserted fields excluded from it?
- Population source: does discovery query a live population or a
  registry, and is freshness bounded?
- Identity-first resolution: given a stable identifier, does
  discovery return the current endpoint, and does the identifier
  survive infrastructure change?
- No-starting-point case: is discovery defined for requesters with
  no domain or name to begin from?
- Phase separation: are capability-class resolution and
  entity-level negotiation distinguished, with the handoff between
  them defined?

### P7. Communication (prefix CMM)

- Identity carriage: is identity native to the wire or layered by
  convention?
- Per-message attribution: does attribution (identity plus
  delegation lineage) ride on every message rather than only at
  connection setup?
- Peer symmetry: is the model peer coordination (either party
  initiates, proposes, negotiates) or client-server, with the
  choice justified?
- Intermediary inspection: can intermediaries verify attribution
  without access to the payload?
- Delivery semantics: ordering, duplicate detection, and delivery
  guarantees defined per message class?

### P8. Session (prefix SSN)

- Session primitive: protocol-level or application convention?
- Binding: session bound to verified identity from establishment?
- Lifecycle: expiry, suspension, resumption, and revocation
  defined?
- Cryptographic continuity: is a resumed session provably
  continuous with the original, verifiable without trusting the
  store that held the state?
- Multi-endpoint span: can one session carry coherent state across
  multiple participants in a delegation chain?
- Transactional semantics: commit-or-rollback defined where
  workflows require it, including unwinding after partial failure?
- Algorithm agility: does session continuity survive cryptographic
  suite transitions, with the migration path stated, including
  post-quantum?

### P9. Semantics (prefix SEM)

- Vocabulary source: controlled catalog, self-description, or
  free text, with governance per C11?
- Claim binding: are capability claims tied to demonstrable
  bindings rather than assertion?
- Measured effect: is there evidence that the semantic design
  changes agent behavior or accuracy?
- Determinism: given a verb and typed parameters, is the receiver's
  interpretation exact, with no language-inference step on the
  coordination path?
- Extension discipline: can domain vocabularies be added without
  breaking base interoperability, governed per C11?

### P10. Tools (prefix TOL)

- Boundary position: does the design treat tools as substrate
  participants or as a separate stack, with the choice justified
  and the bridging cost of a separate stack stated?
- Integrity attestation: can a caller verify that the tool at this
  endpoint is the expected implementation before invocation?
- Chain pass-through: do delegation chains survive tool invocations
  mid-chain, visible to downstream calls?
- Containment: execution boundaries and privilege limits defined,
  with enforcement located?
- Description precision: is the machine-readable capability
  description sufficient for selection and invocation without
  out-of-band knowledge?

### P11. Commerce (prefix CMC)

- Scope discipline: does the design carry transactions without
  becoming a payment system, with exclusions stated?
- Signature symmetry: can either party unilaterally alter a
  committed record?
- Budget authority: are spending bounds principal-signed and
  verifiable?
- Rail neutrality: settlement mechanisms referenced opaquely?
- Discovery-time pricing: are prices machine-readable,
  merchant-signed, and available in discovery results before
  commitment?
- Machine-scale economics: does the design state the transaction
  sizes and volumes it serves, including micro-transactions that
  existing rails cannot carry?

### P12. Audit (prefix AUD)

- Authorship separation: can the audited party write, alter, or
  omit its own trail?
- Tamper evidence: append-only structure with independent
  verifiability?
- Wire binding: is the audit identity the operational identity?
- Regulatory mapping: which record-keeping regimes the records can
  satisfy, stated without overclaim?
- Chain carriage: does each record preserve the delegation chain
  from the acting agent back to the originating principal?
- Selective disclosure: can a party prove that one action occurred
  (or that no record of it exists) without exposing unrelated
  activity by the same agent or principal?
- Longevity across algorithms: do records remain verifiable across
  cryptographic transitions (re-timestamping, transition chains,
  or equivalent), with the post-quantum exposure of long-lived
  records stated?

### P13. Governance (prefix GOV)

- Lifecycle states: defined transitions with defined visibility
  consequences and propagation deadlines?
- Registry consequence: is registration an act or a consequence of
  lifecycle, and is deregistration bounded in time?
- Policy carriage: are governance constraints machine-readable and
  enforced at stated points?
- Accountability binding: is every participating agent resolvable
  to a natural or legal person accountable under existing law, with
  the binding verifiable at the substrate?
- Evidentiary composition: is the relationship to digital-signature
  and evidence law (eIDAS, ESIGN, or their counterparts) stated
  without overclaim, parallel to P12's regulatory mapping?
- Cross-jurisdiction recognition: does the evidence design state
  how records remain usable across the jurisdictions the deployment
  spans, and where recognition depends on treaty, mutual
  recognition, or arbitration agreement?

### P14. Naming (prefix NAM)

The suite carries Naming as an open question rather than a
fourteenth primitive, and this supplement carries the same
qualification. A draft is never graded down for declining to
provide a naming layer; the supplement applies where a draft
provides one.

- Identity separation: is the name a mutable pointer distinct from
  the identity, with the binding explicit and the identifier never
  derived from the name?
- Authority model: who grants names, at what scope, with disputes
  handled where?
- Federation: cross-authority resolution defined with trust
  boundaries stated?
- Squatting and impersonation treatment, including homograph
  handling and verification against the resolved identity's
  binding rather than the label?
- Collision semantics: are duplicate names expected, with
  resolution under ambiguity defined (candidates returned with
  identity metadata rather than failure or a silent guess)?
- Lifecycle rebinding: are rename, transfer, and retirement
  recorded as pointer changes that never sever the identifier's
  history?
- Local aliasing: are private petnames supported without global
  coordination?
- Lookup separation: are resolution (one identifier returned, or
  ambiguity surfaced) and directory search (candidates expected)
  distinguished as separate operations?

### P15. Transactions (prefix TXN)

The criteria grade the unit-of-account layer the suite's §15
defines: the engagement (transaction), its constituent acts
(actions), and the receipt. One boundary governs the supplement,
inherited from the problem statement: this layer names and
receipts; it never evaluates. Designs that bundle the identifier
with judgment are graded on whether the bundling is stated and
analyzed, per C13 — the supplement itself takes no side on where
judgment lives, only on whether the unit layer is separable.

- Unit naming: is a transaction identifier established when the
  engagement forms, resolving to the same unit in every
  participant's records and to a later stranger reading any of
  them?
- Action structure: are individual acts named and bound to their
  transaction, with relational child-to-parent references rather
  than one identifier spanning the graph?
- Receipt: is there an attributable statement (identity, act,
  unit, time) that a named action occurred, held by participants
  in equivalent form? Where one mechanism supplies receipt
  attribution and P7's per-message attribution, the same citation
  serves both rows, per the recurring-terms rule.
- Verdict neutrality: do identifiers and receipts carry zero
  evaluation, with the design stating where judgment lives and
  keeping the unit layer separable from it?
- Bearer power: does possession of a transaction or action
  identifier confer nothing, with correlation and authorization
  structurally separated?
- Mint model: who establishes the identifier between strangers
  (initiator, responder, jointly, derivation, or a registry), with
  the trade analyzed — a central mint states its namespace-operator
  dependence, a decentralized mint states its collision and
  forgery handling; either reaches Specified by stating and
  analyzing its choice.
- Outcome independence: are aborted and failed engagements named
  and referenceable equally with completed ones?
- Cross-span propagation: do identifiers survive session
  boundaries, carrier changes, and delegation hops? Where the
  mechanism is the transport's continuity carriage (P1) or session
  span (P8), the same citation serves, per the recurring-terms
  rule.
- Unlinkability: do unrelated engagements yield unrelated
  identifiers, with severing stated as a property? Retention and
  correlation bounds are graded under C15; this row grades the
  identifier design.

### Provisional supplement: Presence and Population Awareness (prefix PRS, reserved)

This supplement entered through the reverse-coverage mechanism and
is the worked example of that mechanism: evaluation surfaced a
concern the problem-statement suite does not yet cover. The suite's
Discovery section names structural discovery through presence, and
its Transport section lists ambient availability advertisement, but
no problem statement owns the concern. A problem statement in the
suite's format is the intended next step; until it lands, C2
enumeration for presence-centered drafts records zero matches and
routes to reverse coverage, and this supplement applies.

- Join semantics: is discoverability a consequence of joining or a
  separate registration act?
- Liveness: staleness bounds, refresh, and eviction defined?
- Visibility control: can a participant bound who observes it, with
  enforcement located where?
- Convergence: propagation bounds stated for the intended scope
  sizes?

### Depth supplements (contributed stress criteria)

The primitive supplements above grade provision: what a draft
provides and at what evidence level. For primitives whose hard
questions are cross-organizational (delegation, trust, revocation,
audit), a second layer grades survival: whether the capability
holds when the counterparties are strangers, the issuer is
unreachable, and the verifier is offline. Depth supplements carry
these stress criteria. They are contributed by domain experts,
credited to their contributors, expressed in the row schema on the
same evidence scale, and held to the same solution-neutrality bar
as the core.

Integration rules, so unfinished depth rows never read as
mandatory: depth criteria are additive, never replacements for
base supplement criteria. A depth criterion is graded only where
the base supplement stands at Specified or above, or where the
draft itself claims cross-organizational capability; below that
bar, depth rows are omitted rather than recorded Absent. A draft
may record Scoped-out against a depth criterion with the same
legitimacy, and the same citation requirement, as against any
other criterion.

In the pipeline: candidate delegation and trust depth criteria
derived from the R1-R9 requirements of
draft-reece-wimse-cross-org-delegation (contributor: M. Reece),
with the comparison rows in the Karthik mapping work as the
worked example for the delegation slice. Candidate criteria land
only after passing the neutrality test and contributor conflict
gate of §7; reservation here records the pipeline and never
pre-commits the content. Further depth supplements are invited per
primitive through the same path.

### Crosswalk from the v0.2 supplement numbering

Restored per the v0.3 revision note. v0.2 supplements map to the
canonical numbering as follows: v0.2 P1 Identity → P2 (IDY); P2
Naming → P14 (NAM); P3 Discovery and Search → P6 (DSC); P4
Presence → the PRS provisional; P5 Authority → P3 (AZN); P6
Delegation → P4 (DGN); P7 Trust → P5 (TRU); P8 Attribution and
Audit → P12 (AUD); P9 Semantics → P9 (SEM); P10 Transport → P1
(TPT); P11 Session → P8 (SSN); P12 Commerce → P11 (CMC); P13
Governance → P13 (GOV). New with no v0.2 ancestor: P7
Communication (CMM) and P10 Tools (TOL), added in v0.3; P15
Transactions (TXN), added in v0.5.

## 5. Reverse Coverage (mandatory final section of every evaluation)

State, with citations: does this draft address a genuine problem
absent from the problem-statement suite? If yes, draft the candidate
problem statement in the suite's format and submit it. The suite
versions upward through this mechanism, and an evaluation that skips
this section is incomplete. The Presence supplement in §4 is the
standing example of the mechanism producing a candidate primitive,
and Transactions (§15, P15) is the mechanism's first completed
loop: a concern surfaced across evaluations, drafted as a problem
statement in the suite's format, and landed as a supplement.

## 6. Publishing an Evaluation

An evaluation is a table: criterion, grade, citation, one-line note.
Publish alongside the draft name and version evaluated, the suite
version used for C2 identifiers, the evaluator's identity and
affiliation, a conflict statement where the evaluator authors work
in the same primitive or holds intellectual-property interests
touching it, and the evaluated draft's IPR status (disclosures
filed per the datatracker, or none found as of the evaluation
date). IPR status is header metadata rather than a graded
criterion: it is recorded because readers weigh it, never scored,
consistent with the instrument's refusal to grade intent.

Grades attach to the version evaluated, never to the draft in
perpetuity. Re-grading on revision is the intended mechanism: the
instrument exists so that revisions improve against named criteria,
and a draft moving from Asserted to Specified between versions is
the system working. An evaluation of a superseded version stays
published, marked superseded, so the trajectory is visible. A
re-grade carries a delta note: which criteria moved, with the new
citations. Demonstrated-interop and Demonstrated-production grades
older than two years are re-verified or annotated unconfirmed
unless the deployment or measurement remains live; evidence that
has become unreachable marks the grade stale on re-evaluation.

**Disagreement resolution.** Two evaluators can publish
conflicting grades on the same criterion of the same version, and
the instrument treats that as data rather than a fault. The
mechanism: a counter-evaluation links the evaluation it contests
and carries a delta note listing only the contested criteria, each
with the competing citation and one line on why the cited evidence
does or does not meet the level claimed. Both evaluations stay
published and linked. The instrument never adjudicates between
them: contested criteria with competing citations are exactly the
input consensus processes exist to weigh, and the loop closes
there.

**Evaluator checklist**, affirmed with every published evaluation:
treatment maturity was graded, never idea quality; every grade
carries a precise citation (section and paragraph, repository path
with commit, or endpoint with observation date); C2 identifiers
match the suite version cited in the header; Scoped-out and
Inherited cells are recorded explicitly; the reverse-coverage
section is present.

**Row schema**, so evaluations collect and diff without anyone
inventing a scoring system. One row per criterion:

```
| criterion | grade | citation | note |
| C1 | Specified | draft-x-02 §2.1 | admits external solutions |
```

Grades take the values Absent, Asserted, Specified,
Demonstrated-simulated, Demonstrated-reference,
Demonstrated-interop, Demonstrated-production, Demonstrated-formal,
Scoped-out, Inherited(from-doc). C12 may record a running-code rung
and Demonstrated-formal together (for example, Demonstrated-interop
plus Demonstrated-formal) where both artifact classes exist; every
other criterion carries one grade. Supplement criteria are
addressed positionally as Pn.k (the k-th criterion of supplement
Pn) against the rubric version in the header, which is why the
header carries the rubric version. Header metadata: draft, version,
suite version for C2, rubric version for supplement rows,
evaluator, affiliation, conflict statement, draft IPR status, date.

**Conflict-statement template**, completed honestly and present
even when every answer is none: "I author or co-author [drafts] in
this primitive; I hold or have applied for intellectual-property
rights touching [area]; my affiliation [ships / does not ship]
products in this space."

**Machine-readable form.** The same table, expressed so
evaluations collect, diff, and query without anyone inventing a
scoring system on top:

```yaml
draft: draft-x-02
suite_version: v0.3
rubric_version: v0.3
evaluator: J. Reviewer
affiliation: Example Org
conflict: "authors draft-y in P4; no IPR; affiliation ships"
ipr_status: "none found, 2026-07-22"
date: 2026-07-22
rows:
  - criterion: C1
    grade: Specified
    citation: "draft-x-02 §2.1"
    note: "admits external solutions"
  - criterion: C12
    grade: [Demonstrated-interop, Demonstrated-formal]
    citation: "repo@abc123; model in /formal, checked 2026-07-20"
    note: "both evidence classes offered"
  - criterion: P14.6
    grade: Scoped-out
    citation: "draft-x-02 §1.2"
    note: "naming lifecycle explicitly out of scope"
```

**Reverse-coverage template**, matching the problem-statement
suite's format: a one-paragraph statement naming the failure in
current infrastructure, the parties affected, the consequence of
leaving it unsolved, and the test a solution must pass, written
without reference to any particular solution.

**First pass in one sitting.** Authors self-grading their own draft:
walk C1 through C17 in order against the current version with the
draft open, cite as you go, mark honestly what lands at Asserted,
complete the primitive supplement and reverse coverage, publish.
The instrument is sized so a first pass fits in a working session;
depth comes from the corrections it invites.

## 7. Governance of This Instrument

The instrument imposes change discipline on everyone's registries
(C11) and owes its own. Revisions are versioned in the open
repository alongside the problem-statement suite; changes arrive
through issues and pull requests; every renumbering ships a
crosswalk (§4 carries the current one); and grades attach to the
version evaluated, so no revision invalidates a published
evaluation.

Two gates apply to every candidate criterion, whether core,
supplement, or depth:

- **Neutrality test.** A candidate criterion must be phrased so
  that at least two architecturally opposed designs could each
  reach Specified on it. A criterion only one architecture can
  satisfy encodes a solution and is rejected or rephrased, however
  sound the underlying concern.
- **Contributor conflict gate.** Criterion and depth contributors
  complete the same conflict-statement template as evaluators
  (§6). Holding drafts or intellectual-property interests in a
  primitive never disqualifies a contribution; the conflict is
  recorded with the contribution, and the neutrality test does the
  filtering that disqualification would do badly.

Two standing limits, stated here so they never have to be argued:

- **Relationship to process.** The instrument informs: it gives
  working-group adoption calls, directorate reviews, and
  individual reading a common evidence structure. It never
  substitutes for consensus, and no grade is a prerequisite for
  any process step anywhere. An evaluation is an input to
  judgment, never a gate in front of it.
- **No scoring, ever.** Future versions will not add
  recommendation, ranking, weighting, or composite-score fields to
  the row schema or anywhere else. The pressure toward scoring is
  the instrument's known failure mode; describing without ranking
  is the property everything else here protects.

---

## Contributions

This instrument improves through contributed criteria, corrections,
and depth supplements. Contributors are recorded here as their
material lands.

- Morgan Reece: delegation and trust depth criteria (R1-R9,
  draft-reece-wimse-cross-org-delegation); landscape additions.


## Change log

**Revision note (v0.3).** The supplements are renumbered to the
problem-statement suite's canonical order, resolving the provisional
taxonomy carried in v0.2. Transport and Communication are split into
separate supplements, matching the suite. A Tools supplement is
added. Naming moves to P14 and carries the suite's open-question
qualification. Presence and Population Awareness is held as a
provisional supplement pending a problem statement, as the worked
example of the reverse-coverage mechanism. Criteria are added across
supplements to cover the suite's stated requirements. A sixteenth
core criterion (Adoption path) is added. A stable identifier scheme
for C2 enumeration is defined. 

**Revision note (v0.4).** P1 Transport is rebuilt to match the
revised Transport problem statement, which now centers on the
transfer layer's design axes and the placement question. Criteria
are added for primary unit and scoping, state model, method
surface, and identity placement; the cancellation criterion is
generalized to control-propagation semantics with cross-participant,
cross-carrier ordering and visibility. All five v0.3 criteria
survive — three verbatim, one generalized, one unchanged in text
but repositioned — and a position crosswalk closes the supplement,
per the positional-addressing rule. Cross-references are added
between P1 and the adjacent P7 and P8 criteria so that one
mechanism grades once under the recurring-terms rule rather than
twice under neighboring supplements. 

**Revision note (v0.5).** P15 Transactions (prefix TXN) is added,
grounded in the suite's new §15 problem statement: the accountable
unit of agent interaction, its action structure, and the dual-held,
verdict-neutral receipt. Transactions enters as a
problem-statement-backed supplement — the contrast with the
Presence provisional is deliberate, and §5 now records Transactions
as the reverse-coverage mechanism's first completed loop (candidate
concern to problem statement to supplement). 