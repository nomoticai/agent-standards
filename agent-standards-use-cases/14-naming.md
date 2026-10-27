# 14. Naming (NAM)

## Scope, status, and terminology

This document is a problem statement about a **human-facing resolution layer**, not a machine-coordination primitive. Machine coordination runs, and must continue to run, solely on the identifiers of document 02; nothing below proposes otherwise, and the discipline is stated as a rule up front: **no security, authorization, audit, or continuity property may ever depend on a name string — only on the resolved identifier and its bindings.** Names are mutable pointers humans use; identity is the durable anchor machines use; a resolution layer connects them. The document stops short of prescribing a namespace architecture, a registry model, or a display convention.

Two lookup patterns are distinguished throughout, because conflating them is where naming systems fail: **resolution** (a name must return exactly one identifier; ambiguity is a condition to surface, never to guess through) and **directory search** (a name query returns all matching candidates; ambiguity is the expected result). Many observed failures are one pattern deployed where the other was meant.

**Suite alignment.** The two-step lookup this document describes — name to Agent-ID here, Agent-ID to current endpoint and verification material at discovery — has its second step already defined: it is document 06's identity-first resolution, referenced rather than reinvented. The verification posture — check the resolved identity's binding, never trust the string — is only possible because document 02 made the resolved identity verifiable; the naming layer inherits that dependency rather than adding assurance of its own. And the regime questions this layer generates (dispute resolution, trademark and reserved-name policy, registry stewardship) are routed to the governance backlog collected in document 13, as additions to it.

## The skeptical case

Naming for human reference is among the oldest solved problems in computing and civilization: domain names, email addresses, handles, package registries, legal names, trademarks, and every phone's contact list. And the suite itself arms the skeptic: document 02 established that coordination attaches to stable, opaque identifiers — delegation binds identifiers, audit attributes to identifiers, discovery resolves identifiers. Humans operate the same way underneath their names (an employee number identifies the person whatever anyone calls them). The identifier is the substrate; the name is display; display belongs at the application layer, where each product labels things as it likes. Nothing new required.

## Where the mismatch actually lies

The skeptic is right about the architecture and wrong about the consequence. Every identifier system that touches humans has grown a naming layer, without exception, because humans cannot operate on opaque identifiers: DNS exists because nobody remembers dotted quads; nobody asks a bookstore for an ISBN; every phone ships with a contact list; every banking app offers account nicknames. The pattern is universal — machine coordination on identifiers, human participation on names, resolution between — and humans remain in the loop of agent coordination at every consequential point this suite has described: voice instruction, candidate selection (06), approval, finance reconciliation (03/11), audit review (12), evidence read by a court (13). None of those touchpoints can carry a 64-character hash.

So the question is never *whether* names will exist — applications invent them the moment users demand them — but whether the layer is designed once, with resolution, collision, lifecycle, and impersonation handled coherently, or reinvented per application with none of those properties holding across any two.

Scale makes the design harder than the contact-list image suggests. Domain naming worked because the unit was roughly the organization; agent populations multiply per employee and per department, putting hundreds of thousands of agents under one corporate domain — a population flat labels were never meant to carry. Human naming conventions inherit human collision behavior (every common name is shared by thousands), tolerable at human scale because humans disambiguate by context; at machine speed, the disambiguation must be designed in. And **Zooko's triangle** bounds the whole space: human-meaningful, globally unique, decentralized — conventionally two of three. DNS chose meaningful and unique and paid with centralized registries, squatting markets, and the UDRP as a permanent institution. Petname systems chose meaningful and decentralized and paid with global uniqueness — which is exactly the corner the Agent-ID already covers. The layered answer settled by petname research decades ago — global uniqueness at the identifier, human meaning at the name, local aliases resolving to global IDs — is the architecture this document assumes, with the honest note that it *distributes* the triangle across layers rather than dissolving it.

The market will produce several conventions in parallel, and the layer must accommodate all rather than pick a winner: hierarchical namespacing (short names recurring safely across scopes — the directory pattern); vanity name plus discriminator (with its deployed history noted below); descriptive capability names (job titles that collide by design, duplicating discovery metadata in a worse form, and inevitable anyway); anthropomorphic personal names (with the anthropomorphism question left to deployments, per document 02's handling of the workforce frame); and pure local petnames (the contact-list model, requiring no global coordination — which is exactly why it works).

## What observably breaks today

**Voice and casual reference are foreclosed.** The only unambiguous reference to a third agent is its raw identifier, which cannot be spoken, remembered, or reliably typed; every spoken instruction that names an agent is currently resolved by application-specific guesswork or not at all.

**Labels are per-application and non-portable.** The chat interface, the workflow tool, and the finance system each keep their own alias table for the same agent; a label created in one means nothing in the next, and no two systems agree.

**Cross-system human review is manual correlation.** Finance, audit, and legal receive different labels for one agent from different systems — document 03's reconciliation archaeology and document 12's log-joining problem, reproduced at the human-display layer even where the identifier joins succeed underneath.

**Renames orphan history where names were load-bearing.** Teams that overload existing mechanisms (DNS labels, hostnames) couple the name to the identity; when the agent is renamed, reassigned, or replaced, records keyed on the old label go ambiguous or orphan — the failure the identifier/name split exists to prevent, occurring wherever the split was not enforced.

**Collision handling is guess, fail, or fall back to the hash.** A short name used where many agents share it is resolved by silent guessing (the dangerous case), opaque failure, or forcing the human back to the identifier (the unusable case); no consistent surface-the-candidates behavior exists anywhere.

**Impersonation defense reduces to string inspection.** A lookalike name passes casual review because nothing systematically verifies that the resolved identity carries the claimed organizational binding; the phishing-domain playbook ports to agent names intact, with homographs waiting where internationalized characters are allowed.

## Properties without a common definition

Framed as the failures above, inverted into what humans and their tools must be able to do — with the non-load-bearing rule governing every line:

- **Resolve a name to exactly one identifier, or surface candidates** — with identity metadata for disambiguation, never a guess; and search, separately, returning all matches by design.
- **Scope the same label differently in different namespaces** — personal, organizational, global — with the scope explicit in resolution.
- **Keep private petnames** with no global coordination, resolving locally to global identifiers.
- **Verify the name by the binding, not the string** — a presented name checks the resolved identity's organizational or principal binding (02's verifiability; 05's attested-versus-asserted), so lookalikes fail resolution rather than passing inspection.
- **Rename, repoint, and transfer as recorded operations** — the name moves; the identifier's history does not; every rebinding is recorded. *(02's lifecycle, at the display layer; the audit record keys on the identifier throughout.)*
- **Display name and identifier together** in receipts, approvals, audit views, and evidence — humans read the name; reconciliation and verification run on the identifier; nothing depends on the name being unique or permanent.
- **Handle collision and homograph adversarially** — as expected namespace conditions with defined behavior, not as anomalies.

## Prior art, and what it constrains

**DNS and the UDRP** are the founding precedent for the meaningful-and-unique corner and its permanent costs: squatting markets and an institutionalized dispute process are not DNS bugs but properties of any valuable shared namespace — the first constraint on ambition here.

**The petname literature** (Zooko's original formulation; the petname-system tradition that followed) is the settled architecture this document adopts: local meaning, global identifiers, resolution between. The phone contact list is its billion-user deployment — no two lists agree and none need to.

**Namecoin** is the essential graveyard entry: the system built expressly to square Zooko's triangle — all three corners via a blockchain — whose namespace filled almost entirely with squatted names and died; **ENS** is the live sequel demonstrating that decentralized global vanity names reproduce DNS's squatting economics *without* inheriting the UDRP's remedy. Together they establish the constraint firmly: architecture does not dissolve the triangle or the squatting economics; disputes over valuable names are resolved by regimes, and the regime question routes to document 13's backlog.

**Discord's discriminator reversal** is the honest deployed data point for the vanity-plus-tag convention: the largest-scale deployment of exactly that scheme (handle#1234) abandoned it in 2023 under sustained user demand for unique vanity handles. The lesson is behavioral, not architectural: human preference actively fights the clean layered answer, users want *the* name and not *a* name-plus-suffix, and namespace politics do not stay solved.

**X.500/LDAP directories** (inherited from document 06) are the deployed enterprise precedent for hierarchical scoped naming — "which Chris?" answered by scope is literally the directory information tree — proven at organizational scale and never beyond it.

**The IDN homograph history** — punycode, browser display policies, the ongoing arms race — is the deployed precedent for internationalized-name attacks and the demonstration that homograph defense is a maintained posture, not a shipped feature.

**The maritime registry** remains the cleanest lifecycle model and is promoted from analogy to prior art: a century of practice in which the ship's name changes with every sale while the IMO number carries the vessel's identity and history through every rename, reflag, and transfer — the name-as-mutable-pointer architecture, operating under (note the recurring condition) an international registry regime.

The constraint the history imposes: the layered petname architecture is settled and right; every attempt to get all three Zooko corners in one string has failed or reproduced DNS's pathologies; valuable shared namespaces grow squatting and disputes under every architecture yet tried, and their resolution is institutional; and deployed experience says users will fight the layering itself. A naming layer's design is the easy half; its namespace politics are the hard half, and the hard half is governance.

## The core use case: referring to the right agent by name at organizational scale

A global enterprise employs 150,000 people; each works with one to five agents; departments run shared fleets on top. The population under the corporate domain exceeds half a million. An employee finishes a draft agreement and tells their assistant, by voice: *"Send this to Ted for legal review."*

Several hundred agents across the company answer to some form of Ted, because teams named their agents independently. Several dozen agents perform legal review. The employee means exactly one: the legal-review agent their department works with. The instruction must resolve, at machine speed, from a spoken three-letter name to one Agent-ID — or fail cleanly into a disambiguation prompt, never a guess.

**What breaks today.** The employee's only unambiguous option is the raw identifier, so voice reference to a third agent is effectively foreclosed. Each tool keeps its own alias table; none is portable. Teams that wanted memorable names overloaded DNS labels under the corporate domain, so the flat space strained immediately, meaningful names exhausted, internal squatting appeared, and names became coupled to identities — renames now orphan records. Finance sees whatever label each vendor system captured; connecting a statement line to an agent is manual correlation on top of the identifier problems documents 03 and 12 already catalogued. And a lookalike of the legal department's naming convention passes casual review, because nothing checks that the label binds to an agent legal actually operates.

**What any adequate answer must make possible.** "Ted" resolves as a petname, scoped to the employee and department, to one Agent-ID — confirmed by voice with name plus a distinguishing attribute ("Ted, legal review"), with coordination proceeding on the resolved identifier from that point. The organization holds a canonical scoped name for the same agent; both point at one identifier; neither is the identity. Resolution is the two-step of documents 14-then-06 — name to identifier here, identifier to endpoint there — which is exactly what lets the name survive infrastructure changes and the identity survive renames. Receipts, approvals, and audit views display name alongside identifier; humans read one, systems verify the other. When the department replaces its legal-review agent, the canonical name is repointed and the rebinding recorded; the old agent's history stays attributed to the old identifier; employees keep saying "Ted"; the audit trail never blurs. And the external lookalike fails resolution against the organizational namespace, because the check runs against the resolved identity's binding, not the string.

**What no answer can promise, and this document does not claim.** *Escape from Zooko:* the layered design distributes the triangle across layers — no single string gets meaning, uniqueness, and decentralization at once, ever. *Escape from namespace economics:* any shared namespace with valuable names grows squatting and disputes under every architecture tried; the remedy is a dispute regime (UDRP-shaped), which is a document-13 backlog item, not a naming-layer feature. *Escape from human perception:* however non-load-bearing the system makes names, humans will treat them as identity — thirty years of "check the URL" has not ended phishing, and email display-name spoofing is the standing proof that a string carrying zero system trust still carries de facto human trust. The achievable claim is exact: the *system* never depends on the string, and the human-perception attack surface remains, mitigated by verification presentation and the trust layer's evidence, never eliminated. *A floor under voice:* homophones, accents, and recognition error give spoken resolution an irreducible error rate; the disambiguation prompt is the designed floor, not a failure state.

**Open questions this use case exposes.** Where organizational namespaces are administered and how they federate; what the global vanity layer, if any, should be and who runs its disputes *(routed to 13)*; and how internationalized names are displayed safely across the surfaces humans actually read — solution-space and regime-space; the failures stand regardless.

## Additional naming needs

Stated as human-facing needs or failures; the non-load-bearing rule applies to every item.

**Reference and resolution**
- Resolve a spoken or typed name to one identifier, or surface candidates with disambiguating identity metadata — never guess.
- Search a directory by name expecting many matches, as a distinct operation from resolution.
- Resolve the same short label differently in personal, departmental, and organizational scopes, with scope explicit.
- Keep private petnames requiring no coordination with anyone.

**Lifecycle**
- Rename an agent without disturbing its identifier-keyed history anywhere. *(02's continuity, displayed.)*
- Repoint a canonical name to a successor agent as a recorded rebinding, with the predecessor's history intact. *(08's succession, at the display layer.)*
- Carry a name, or deliberately not carry it, when an agent transfers between organizations — with the binding change recorded either way.

**Adversarial**
- Fail lookalike and homograph presentations at resolution, by binding verification rather than string comparison. *(02/05.)*
- Surface, in every human-facing display of a name, enough of the resolved identity's binding that verification is one glance rather than an investigation.

**Display**
- Present name and identifier together in receipts, approvals, audit views, and legal evidence — the name for the human, the identifier for documents 03, 12, and 13. *(Audit records key on identifiers, always; names appear in presentation only.)*

Items routed to the governance backlog (13): vanity-name registries and their dispute resolution; trademark and reserved-name policy; transparency logging of name-binding changes as a mandated practice; privacy-preserving resolution policy (who may learn who is looking up whom).

## What this document does not claim

- **Not** a primitive. Machine coordination neither reads nor depends on names; this layer exists for humans, and the suite's substrate is complete without it — which is precisely why it is placed last and framed as a question.
- **Not** load-bearing, ever. No security, authorization, continuity, or audit property may attach to a name string; every such property attaches to the resolved identifier. This is repeated because every deployed naming system's worst failures came from forgetting it.
- **Not** a namespace architecture, registry design, or convention winner. The market will run several conventions in parallel; the layer's job is coherent resolution, collision, lifecycle, and verification behavior across them.
- **Not** an escape from Zooko's triangle, squatting economics, or human perceptual trust in strings — the limits above are accepted as permanent operating conditions.
- **Not** the governance of names. Disputes, trademarks, and registries are regime questions, added to document 13's backlog where the suite keeps such things.

The claim is: humans participate at every consequential point of agent coordination and cannot speak, remember, compare, or adjudicate opaque identifiers; names will therefore exist by demand, and today exist as non-portable per-application labels with inconsistent collision behavior, string-inspection impersonation defense, and history that orphans on rename; the settled layered architecture — meaning at the name, uniqueness at the identifier, resolution between, verification by binding — is known and nowhere commonly implemented; and the residue that no architecture removes (namespace disputes, perceptual trust in strings, voice's error floor) is named as such, with its institutional half handed to governance where the suite's other regime questions already wait.

## Forward and backward dependencies

- **Identity (02), backward:** the entire layer rests on the identifier's verifiability and continuity; the SKU shorthand of early drafts is retired per that document's rewrite — the resolved identity is checkable, which is what makes "verify the binding, not the string" possible at all.
- **Discovery (06), backward:** supplies step two of resolution (identifier to endpoint and verification material); directory search composes with discovery's attribute filtering; the name layer adds the human-label index above both.
- **Trust (05), backward:** binding verification consumes the attested-versus-asserted distinction; the perceptual attack surface that names cannot shed is mitigated by evidence presentation, per that document's framing.
- **Authorization (03), Audit (12), Governance (13), backward:** reconciliation, audit records, and legal evidence key on identifiers and *display* names; the finance-correlation and evidence-presentation failures land here as display-layer requirements, never as record-keying changes.
- **Governance (13), forward and final:** the naming layer's regime questions — dispute resolution, trademark policy, registry stewardship, binding-transparency mandates — are formally added to the backlog that document collected, closing the suite where it chose to end: with its institutional questions gathered in one visible place.

---

[← Governance](13-governance.md) &nbsp;·&nbsp; [README](README.md)
