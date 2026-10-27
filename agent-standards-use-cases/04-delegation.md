# 4. Delegation (DGN)

## Scope, status, and terminology

This document is a problem statement, not a solution proposal. It argues that the delegation relationship — a principal's durable, scoped mandate under which an agent acts — currently exists in most deployments as unverifiable prompt text, detached from every credential the agent presents, and that this gap produces observable failures distinct from the authorization failures of the previous document. It stops short of prescribing a mandate format, an evaluation procedure, or a revocation architecture.

**Reconciling the seam with Authorization (03).** Document 03 split the shared territory operationally: authorization owns the receiver's decision at the point of action; delegation owns how authority is created, attenuated, bound to principals, and moved. This document draws the same line conceptually: *delegation is the mandate* — who asked whom to act, for what purpose, under what constraints, with what rights to involve others; *authorization is the permission check* on a specific action here and now. These are the same boundary viewed from two sides, and this document accepts the handoffs already routed to it: principal binding (from Identity), chain construction, attenuation mechanics, and the grant-side semantics of revocation (from Authorization). Receiver-side checks — effect-time validity, remaining budget, action-instance binding — stay in 03.

One term of art carries most of this document's weight: the distinction between a **valid credential** (evidence that verifies correctly and covers the action) and a **live mandate** (a principal currently wants this done, for this purpose, within this frame). The previous document showed that receivers today cannot verify the first across organizational chains. This document is about the fact that even a perfectly verified credential answers only the first question — and the daylight between the two answers is where the most consequential agent failures live.

## The skeptical case

The word "agent" embeds delegation: an agent is one who acts on behalf of another, and the mandate relationship is as old as agency itself — power of attorney, corporate officers, real estate agents, attorneys. The concept is not in question. What the skeptic questions is whether it needs a primitive separate from the authorization chains of the previous document.

The strong skeptical case comes from the systems that already unify the two. UCAN is *named* for authorization and *built* as delegation chains — each link is simultaneously a grant and a permission. RFC 8693's `act` and `may_act` claims encode acting-on-behalf-of directly inside OAuth's token model. X.509 proxy certificates made each delegation hop a certificate issuance. Kerberos constrained delegation ships bounded sub-delegation in every Active Directory deployment. In all of these, "who asked whom" and "what is permitted" are one chain structure, one verification, one lifecycle. The skeptic's conclusion: the distinction this document wants to draw is bookkeeping inside a structure the previous document already covered. Fold the construction rules into the authorization chain spec and stop.

At small scale the skeptic is right, and the previous document said so: a single principal delegating a single well-scoped task to a single agent collapses the mandate into the credential — the authorization check *is* "did the principal delegate this."

## Where the mismatch actually lies

The mismatch appears when the mandate and the credential come apart — and for agents they come apart constantly. The two axes are independent, and the matrix makes the independence visible:

|                | Authorization check passes                                                              | No authorization required                                                     |
| -------------- | --------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| **Live mandate**    | Legitimate agent action: credential verified, mandate current                       | Legitimate agent action: no permission needed, but the asking still exists     |
|                | *Agent purchases a paywalled subscription the principal requested*                       | *Agent searches public news at the principal's request*                        |
| **No live mandate** | **The dominant failure mode: valid credential, absent mandate**                     | Software executing without direction; outside agent behavior entirely          |
|                | *Injected instruction exercises a genuine token; a revoked or orphaned mandate's credentials keep working* | *Unprompted process activity; no principal anywhere in the picture*            |

Earlier drafts filled the bottom-left cell with "the receiver's authorization check fails" — defining the most important case out of existence. The previous document's central threat model, the confused deputy, lives precisely there: the credential is genuine, the action is in scope, the check *succeeds* — and no principal currently mandates the action, because the "instruction" arrived through injected content, or the mandate was revoked after the credential was minted, or the principal departed months ago and nobody withdrew anything. Authorization answers "does a credential cover this action." Delegation answers "does anyone currently mandate it." Every failure in that cell is invisible to a system that can only ask the first question, and that cell — not the conceptual elegance of the axes — is the operational case for treating the mandate as a first-class, checkable fact.

The familiar professional analogies map onto the matrix and are worth keeping. A real estate agent holds a broad mandate (sell the house); within it, some actions need explicit sign-off (accepting an offer), some need none (showing the house), and some are prohibited regardless of instructions (misrepresentation). An attorney's engagement works identically: mandate-wide discretion, action-specific authorization for settling or waiving privilege, and hard prohibitions no instruction can unlock. The mandate frames a space; authorization governs points within it; the prohibitions are neither.

Beyond the matrix, four patterns require mandate properties that a per-action credential chain does not naturally express:

**Pre-authorization across unavailability.** "My assistant may approve invoices under $500 while I am traveling for two weeks." The grant is made when the principal is present, exercised when they are not — the mandate must outlive the granting interaction.

**Conditional activation.** "If I am unreachable for 48 hours, my agent may approve time-critical items under $500." The authority springs into existence on a future condition; someone must evaluate the condition, at exercise time, against evidence the principal defined in advance. (An earlier draft used "if the booking exceeds my monthly cap, charge the excess to my personal card" as the example here — which is a governance anti-pattern, converting a hard cap into no cap by automatic overflow, and is retired.)

**Transitive delegation.** "The service I engage may engage others on my behalf, provided each sub-grant is narrower and serves the same purpose." Whether anyone else may act within the frame — and under what attenuation guarantee — is a grant-time governance decision, not a point-of-action check.

**Purpose and temporal scoping.** "For the purpose of managing my child's tuition over the next four years." A purpose is not an action list, and cannot be compiled into one in advance; it is a frame receivers must evaluate case by case. This is the hardest of the four and gets its own section below.

One correction to earlier drafts' framing: it is not true that human organizations lack delegation primitives, nor that software does. Durable powers of attorney, springing powers, corporate delegation-of-authority matrices, and engagement letters *are* delegation primitives — formal, documented, bounded, in routine use; sudo and service impersonation are software's crude versions. What they all share is that the mandate is a document interpreted by humans, disputed through human processes, at human speed and frequency. Agents execute against their mandates thousands of times a day, across organizational boundaries, with no human reading anything. As with the travel-reconciliation case in the previous document, the pattern is old; its existing resolution mechanism does not survive the operating regime.

## What observably breaks today

**The mandate is prompt text.** In nearly every current deployment, the delegation relationship exists as natural-language instructions in a system prompt or task description. It is unverifiable by receivers, invisible to auditors, revocable only by editing text the counterparties never saw, and connected to the agent's actual credentials by nothing at all. The most important fact about every agent action — who asked for it and why — is stored in the one place no verification can reach.

**Receivers cannot distinguish the bottom-left cell.** A mandated request and an unmandated one arrive carrying the same valid token. Injection-driven actions, actions under revoked mandates, and actions of orphaned agents whose principals have departed are all, from the receiver's side, indistinguishable from legitimate ones. The identity document's orphaned-agents failure recurs here with its delegation face: the *credentials* get cleaned up eventually by expiry; the *mandate* was never a system object that could be cleaned up at all.

**Sub-delegation is implicit and unbounded.** Agents spawn sub-agents constantly; no mandate attaches to the spawn, no attenuation is checked, and a downstream sub-grant broader than its parent is undetectable by anyone further down the chain — there is no parent on the wire to compare against.

**Mandates go stale silently.** A purpose-scoped delegation created for a multi-year relationship continues to be honored after the relationship, the organizational policy, or the risk posture that justified it has changed. Nothing distinguishes a mandate that is old-and-current from one that is old-and-abandoned.

**Overlapping mandates create undefined ambiguity.** Two grants from the same principal — different purposes, windows, and sub-delegation rules — can both plausibly cover a given action. Which governs, and whether the action must satisfy both, is defined nowhere.

**Mandate revocation has no path of its own.** Withdrawing a mandate propagates, if at all, through credential expiry — inheriting the undefined revocation semantics documents 01 and 03 already catalogued, with an extra hop of indirection. A parent mandate revoked mid-workflow leaves downstream parties honoring attenuated credentials rooted in nothing.

**Strangers must evaluate purposes.** A receiver in another organization gets a request justified by "manage tuition-related expenses." Sharing no context with the principal, it can only refuse, or trust the immediately preceding hop — the exact failure posture the previous document described for lineage, now for meaning.

## The hard core: purpose scoping and intent drift

The most consequential sentence in this document's territory is also the easiest to write past: *a purpose cannot be expressed as an action list.* Kerberos constrained delegation, OAuth scopes, and UCAN capabilities are all action-list systems, and not by oversight — semantic purpose evaluation was intractable, so the deployed world enumerated. Agents break the enumeration strategy from both ends: principals state mandates in natural language because that is how purposes exist, and agents then execute against those mandates at machine frequency, encountering situations the principal never enumerated because enumeration was never possible.

This raises questions with no current answer of any kind. Who evaluates whether a concrete action serves "managing my child's tuition" — the agent (self-judging), the receiver (a stranger to the principal), or some designated party? By what procedure, at machine speed? What happens when the principal and the receiver disagree after the fact — what record exists to dispute over?

Adjacent to purpose evaluation sits **intent drift**. Over a long chain of autonomous sub-tasks — the agent engages a service, which spawns specialists, which invoke tools — the claim "every action is delegated" attenuates toward vacuity. Each step is locally plausible given the previous one; the composition can wander arbitrarily far from anything the principal meant. The interesting question is never whether an action was caused by the original request (everything downstream was) but whether it is still meaningfully *within* it — and that question is currently not just unanswered but unposable, because the original intent exists nowhere the chain can reference.

Agency law is the honest benchmark here: two centuries of doctrine on actual versus apparent authority, scope-of-mandate disputes, and liability for agents exceeding instructions — and its resolution mechanism is *ex post adjudication*, not ex ante decidability. Human institutions never solved objective purpose compliance; they made it disputable.

**What no solution can promise, and this document does not claim:** that purpose compliance can be made objectively decidable, or that intent drift can be eliminated. The achievable requirement — whose absence is the actual problem — is that the mandate be *recorded, referenceable, and disputable*: a fixed artifact the chain can point to, that receivers can evaluate against by some defined procedure, that auditors and adjudicators can hold both sides to afterward. Today the mandate is a prompt; there is nothing to point to, no procedure to follow, and nothing to adjudicate.

## Properties without a common definition

Framed as the questions a receiver or auditor must be able to answer about the mandate behind an action:

- **Expression:** who delegated, to whom (a specific agent identity, not a credential holder), for what purpose, with what scope and quantitative limits, over what window, under what conditions, with what sub-delegation rights. (Earlier drafts presented this as a record schema; it is stated here as the answerable-question set, with the encoding left to the solution space.)
- **Liveness:** whether the mandate behind a valid credential is current — not revoked, not expired, its principal still existent and still standing behind it.
- **Attenuation guarantee:** whether each sub-grant in a chain is verifiably no broader than its parent, such that expansion is detectable downstream rather than invisible.
- **Conditional semantics:** for springing grants, who evaluates the activating condition, against what evidence, decided when.
- **Purpose evaluation:** by what procedure a party outside the principal's context assesses whether a concrete action falls within a stated purpose — and what the dispute path is when parties disagree.
- **Conflict resolution:** which of several overlapping mandates from one principal governs a given action.
- **Lifecycle:** revocation with a propagation story of its own (mechanism shared with 01's control propagation and 03's effect-time problem); expiry; principal departure and succession; and *transfer* — whether a mandate survives a change of subject, which collides directly with the identity document's continuity rules and is flagged as genuinely hard rather than a routine list item.

## Prior art, and what it constrains

**Durable and springing powers of attorney** are the core use case solved on paper: authority exercised during the principal's unavailability, and authority activating on a future condition, respectively — with centuries of practice behind both. They prove the mandate-as-instrument model works; they also show its operating assumptions (a human reads the instrument; a notary anchored it; a court resolves disputes) — every one of which the agent regime removes.

**Agency law** is the deepest prior art in the suite: actual versus apparent authority *is* the valid-credential/live-mandate gap; scope-of-mandate litigation *is* intent drift with case law attached. Its lesson constrains ambition: even mature human institutions handle purpose disputes by adjudication after the fact, which is why the achievable requirement above is disputability, not decidability.

**Corporate delegation-of-authority matrices** refute the claim that human organizations manage without the primitive — and demonstrate the primitive's current form: documents plus human judgment, at human cadence.

**UCAN** is the strongest technical prior art and the skeptic's best exhibit: delegation-first chains where grant and permission are one structure. The delta any proposal must state: UCAN links are capability lists with lifetimes — they carry no purpose frame, no liveness distinct from expiry, no conditional activation, and no evaluation procedure for anything unenumerated. **Kerberos constrained delegation** deploys bounded sub-delegation at enormous scale — action-list-shaped and trust-domain-bound. **RFC 8693 actor claims** encode on-behalf-of inside an issuer topology, with the same limits document 03 described. **Proxy certificates and Macaroons** recur from that document with their lessons intact, adoption warnings included.

The constraint the history imposes: the mandate-as-instrument is solved at human speed; the chain-as-credential is solved technically several times over. What nothing provides is a machine-referenceable mandate with liveness, purpose framing, governed transitivity, and a defined evaluation-and-dispute procedure — and any proposal inherits both the adoption fates catalogued in document 03 and agency law's warning about how much can be decided in advance.

## The core use case: pre-authorized action while the principal is unavailable

An executive leaves on a two-week international trip, reachable only intermittently. Before departing, they establish a mandate for their assistant agent: approve vendor invoices under $2,000 that match previously approved contracts; respond to project inquiries from known partners; schedule meetings for after the return date; sub-delegate tax-deadline items to the accountant's agent, and nothing further.

During the window, each action meets the matrix differently. Invoice approvals hit receivers that run authorization checks — contract match, amount, window, revocation status — all of which read the mandate established in advance. Responding to a partner's inquiry needs no authorization from anyone, yet is still delegated: the assistant acts because the executive asked, within a purpose the executive framed. The corporate analog is exact and routine: a delegation-of-authority memo issued before an executive absence, under which the designated officer both signs bounded approvals and conducts unremarkable business, all legitimately.

**What breaks today.** The mandate is a prompt. The vendor's systems see a valid credential and nothing behind it — they cannot check the contract-match condition, the window, or revocation, because none of those exist as facts anywhere outside text the vendor has never seen. If the executive's return is delayed and they extend the window by message, the "extension" is another prompt. If they revoke the sub-delegation to the accountant's agent mid-window, nothing propagates. If an injected instruction directs the assistant to approve a fraudulent invoice under $2,000, every check that exists passes. And when the executive returns and disputes an approval, the reconstruction problem of document 03 recurs with a harder edge: not just *what chain authorized this*, but *what mandate was it supposed to serve* — and the mandate was never an object.

**What any adequate answer must make possible.** A receiver or auditor can answer, from records: who established the mandate, its scope and window and conditions, whether it was live at the moment of the action, whether the sub-delegation to the accountant's agent was within the granted transitivity, and — when a dispute arises — what fixed artifact both sides are held to.

**What no answer can promise.** That "matches a previously approved contract" or any purpose-shaped condition becomes objectively decidable at machine speed, or that revocation reaches all parties instantly (document 03's tradeoff curves apply to mandate revocation with full force). The achievable properties are defined evaluation procedures, defined exposure windows, and a disputable record.

**Open questions this use case exposes.** Where condition evaluation lives (agent, receiver, designated evaluator); how mandate liveness is checked against document 03's offline-verification constraints; and what the conflict rule is when this mandate overlaps a standing one.

## Additional delegation needs

Stated as observable needs or failures; receiver-side counterparts remain in document 03.

**Grant and frame**
- Establish a mandate whose terms — principal, subject, purpose, scope, window, conditions, transitivity rights — are referenceable by parties not present at its creation.
- Frame authority by purpose and time where enumeration of actions is impossible, with a defined evaluation procedure for the unenumerated case.
- Establish a springing mandate whose activating condition is evaluated at exercise time by a defined party against defined evidence.

**Transitivity**
- Grant bounded sub-delegation such that each sub-grant is verifiably no broader than its parent, and expansion is detectable downstream.
- Attach a mandate to sub-agent spawning, which today occurs with no grant object at all.

**Lifecycle**
- Revoke a mandate before expiry, with a propagation story distinct from credential expiry. *(Mechanism shared with 01's control propagation; receiver-side detection lives in 03.)*
- Handle principal departure and succession — retirement, offboarding, organizational change — without mandates silently outliving their principals. *(Joint with identity's orphan detection.)*
- Transfer a mandate to a different subject, or determine that it cannot survive the subject change. *(Collides with identity's continuity rules; flagged hard.)*
- Distinguish an old-and-current mandate from an old-and-abandoned one.

**Coexistence**
- Determine which of several overlapping mandates from one principal governs an action, and whether the action must satisfy all applicable frames.
- Evaluate, as a stranger to the principal, whether a concrete action falls within a stated purpose — or determine that the evaluation cannot be made and what follows from that.

## What this document does not claim

- **Not** that the mandate concept is novel. Powers of attorney, agency law, and delegation-of-authority practice are centuries deep; the claim is that their resolution machinery — human reading, human cadence, ex post adjudication — does not survive the agent operating regime, and no machine-regime equivalent has a common definition.
- **Not** that purpose compliance can be made objectively decidable or intent drift eliminated. The claim is limited to recordability, referenceability, and disputability.
- **Not** any mandate format — not that mandates must be structured data, nor that they may remain natural language, nor any signing or anchoring scheme.
- **Not** that delegation replaces or absorbs authorization. The axes are independent; the matrix is the boundary. A live mandate without a passing check is still blocked; a passing check without a live mandate is the failure this document exists to name.
- **Not** a metaphysical claim that delegation constitutes agenthood. The opening observation is rhetorical framing; the operational claims stand without it, and standing missions or exploratory agents that strain the definition do not weaken them.

The claim is: the delegation relationship currently exists outside every verification, audit, and revocation mechanism agents actually use — as prompt text — so receivers cannot distinguish mandated from unmandated actions carrying identical valid credentials; sub-delegation is ungoverned; mandates outlive their principals and their contexts silently; and purpose-scoped authority, the form principals naturally use, has no evaluation procedure and no dispute path anywhere in the deployed world.

## Forward and backward dependencies

- **Transport (01), backward:** mandate revocation rides on the control-propagation requirement; mandate windows span the multi-day session pauses transport establishes; the mandate artifact is part of the attribution context per-message binding must be able to reference.
- **Identity (02), backward:** the principal-anchoring requirement lands here as mandate expression; the subject of a mandate is a specific agent identity (instance distinction); orphan detection is jointly owned; mandate transfer collides with identity's continuity rules.
- **Authorization (03), backward:** the seam declared there is accepted and reconciled above — construction and grant-side revocation here, receiver-side verification there; the confused-deputy threat model there is the bottom-left matrix cell here; effect-time tradeoffs there govern mandate liveness checks here.
- **Trust (05), forward:** whether to *accept* a well-formed mandate from an unknown principal — and what a mandate's existence is worth as evidence — is that document's question, per the accountable/trustworthy distinction running through the suite.
- **Audit, forward:** the mandate artifact is the reference against which the audit document's causal traces acquire meaning; the dispute path required above is jointly owned.

---

[← Authorization](03-authorization.md) &nbsp;·&nbsp; [README](README.md) &nbsp;·&nbsp; [Trust →](05-trust.md)
