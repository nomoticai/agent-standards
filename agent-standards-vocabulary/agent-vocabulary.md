# AI and Agent Vocabulary

*A recommended taxonomy for engineering accuracy in agentic systems · v0.1 draft · August 2026*

---

## Posture

This document is a recommendation, deliberately. It follows the model SAE used for driving automation: a taxonomy offered for adoption, revised as the field moves, binding on nobody. It lives outside any standards track by design, because vocabulary imposed as a standard gets ignored, while vocabulary that survives review gets adopted. Companies, working groups, and authors may adopt it whole, adopt it in part, or dispute it by pull request.

The purpose is engineering accuracy. The AI industry conflates its own terms daily. Machine learning, generative AI, and AI travel as one word. Agent has no agreed definition and agentic is applied to anything with an API call. Autonomous describes systems that hold no authority of their own. These are engineering errors with legal consequences, because words in specifications become words in contracts, and words in contracts become words in courtrooms. The automobile industry learned this. SAE J3016 defines levels of driving automation and deprecated autonomous for two reasons, engineering accuracy and legal accountability. This document starts from the same two reasons.

Scope note. This vocabulary covers the terms where the industry's usage and the engineering reality diverge. It leaves breadth coverage to the W3C AIKR CG vocabulary, governance depth to the AI Governance Taxonomy and Reference Glossary, and protocol definitions to their specifications. Where those resources define a term well, this document points rather than duplicates.

---

## Part I. Levels of Agent Automation

The spine of this vocabulary. Modeled on SAE J3016, which classified driving by what the automation does rather than by what marketing claims. The same discipline applies to agents, and the same correction comes first: these are levels of automation, never levels of autonomy. At every level, authority is delegated by a human or an organization, and accountability stays with whoever delegated it. There is no level at which authority originates in the system, and this taxonomy contains no such level by design.

**L0. Manual.** A human performs the task. Software presents, stores, or transmits, and executes nothing.

**L1. Assisted.** The system proposes and the human executes. Suggestions, completions, drafts, and recommendations. Every action is taken by a person.

**L2. Task automation.** The human initiates each task and the system executes it, one bounded task at a time, within parameters set at invocation. This is the macro. Most systems marketed as agents today operate here.

**L3. Conditional workflow automation.** The system executes multi-step workflows under a human-defined scope, selecting steps and tools within it. Exceptions return to the human, and the human must remain available to take them. The defining property is that the system handles the sequence while the human handles the surprises.

**L4. Supervised delegation.** The system executes workflows and handles defined classes of exception within its granted scope. Human involvement moves from per-task to per-checkpoint, through reviews, escalation paths, and standing oversight. The defining property is that scope, budget, and escalation rules are set in advance and enforced by architecture rather than by attention.

**L5. Standing delegation.** The system operates continuously under standing authority, initiating work within its grant, across sessions and time. Human oversight operates through governance, through interrupt authority, revocation, budgets, and audit, rather than through participation in tasks. The correct description of the most capable agent contemplated today is standing delegation under governance. The word autonomy adds nothing to that description except a false claim about where authority lives.

Usage rule. Classify a system by the level it operates at in a specific deployment, never by the highest level its vendor claims it can reach. A system capable of L4 that runs with a human initiating each task is operating at L2.

---

## Part II. Corrections

Each entry names the accurate term, the term it replaces or disambiguates, and the reason.

### Automation, never autonomy

Autonomy means self-law, authority originating in the self. No deployed system holds it. Systems automate under delegated authority, and every capability they exercise was granted, scoped, and revocable. Autonomous is a marketing term, and in regulatory contexts a blame-shifting term, moving responsibility toward a system and away from the party who granted its authority. Preferred usage: automated, delegated, or a level from Part I. For the philosophical structure underneath, autonomy versus heteronomy versus simulated law, see the governance glossary.

### Agent

A small software application connected to one or more language models, designed to perform one or many tasks under delegated authority. In its simplest form, an agent is a macro. The definition deliberately excludes claims about intelligence, intent, or independence, because a definition that requires those claims cannot be tested. Note what the definition implies: agent describes an architecture, never a level of automation. An agent may operate at L2 or L5.

A formal international definition already exists: ISO/IEC 22989 defines an AI agent as an automated entity that senses and responds to its environment and takes actions to achieve its goals. The definition above is compatible with it and narrower, naming the current architecture, a language model under delegated authority. Worth stating plainly: the industry debates what an agent is as if no definition exists, while an ISO definition has stood since 2022 and no venue's active work builds on it. A definition existing and going unused is a vocabulary failure of its own kind, and this document cites it precisely so the divergence stays visible.

### Agentic AI

The capability layer, what systems can do: selecting tools, executing multi-step workflows, connecting to services, acting toward defined goals. Agentic describes capability and never legitimacy. Its governance counterpart, defining the rules and constraints under which those capabilities operate, is covered in the governance glossary.

### AI, machine learning, generative AI

Three terms, three referents. Machine learning is the discipline of systems that improve from data. Generative AI is the subset producing content, text, images, code, from learned distributions. AI is the umbrella and, in most current usage, an imprecision. A document that means an LLM should say an LLM. A thread that conflates all three as "AI" is making category errors it cannot detect itself making.

### Model, agent, workflow, application

A model infers. An agent uses a model to perform tasks. A workflow sequences tasks. An application packages any of the above for users. Regulation aimed at models, applied to agents, misses. Standards aimed at agents, applied to models, standardize a probability distribution.

### Inference

The production of output from a model, a sample from a probability distribution. Inference is probabilistic by construction. What the industry calls hallucination is inference operating as designed in a case where plausible and true diverge. Consequence for engineering: reliability comes from the architecture around inference, verification, bounded authority, confirmation before consequence, rather than from the expectation that inference becomes deterministic.

### Search, discovery

Different operations, routinely conflated. Search is a pull, a point-in-time query with criteria. Discovery is ambient, awareness of what is present in scope. A system can search a catalog. A system discovers a population. The distinction decides architectures, because a query surface can answer search while only presence can answer discovery.

### Catalog, registry, population

A catalog or registry is a document collection, answering what was added, as of when it was added. A population is the set of participants present now. The two diverge on every question involving time: availability, revocation, expiry, liveness. A registry that maintains liveness by heartbeat has become a presence system and should be named as one.

### Findable, available

Findable means an entry exists and a query returns it. Available means the agent can take work now. Websites made the two identical because a page serves everyone at once. Agents reseparate them because work occupies. An agent is online, busy, paused, or offline, and a caller committing work needs the state, never just the entry. The barber's listing stays accurate while a client is in the chair.

### Address, code, capability

Three discovery targets at three levels of solved. An address answers where, and address discovery is solved, DNS, URIs, directories. Code answers what will execute, solved at the system level by package registries and signing, and the address never answers it, because the file behind an address can change while the address stays the same. Capability answers what can be done, and capability discovery is unsolved as infrastructure, implemented privately by every marketplace because nothing underneath provides it.

### Name, identifier, identity, address

Four words the industry uses as one. An identifier is the unique reference. Identity is the verifiable fact the identifier resolves to, the who. A name is a mutable, human-facing pointer to an identifier, carrying no security property, ever. An address is where the identified thing currently is. The pairs that matter: identity is permanent while address is updatable, the way a Social Security number persists while a driver's license renews. A name may change freely because nothing load-bearing depends on it.

### Onboarding, registration, publishing

Registration as commonly deployed is a transaction with a registry, with a namespace owner, often with rent. Publishing is making a signed record available. Onboarding is the organizational act of admitting an agent to an environment, the way employees are admitted, with credentials, scope, and a role. The engineering question worth asking in every design: is entry an act performed with an authority, or a property of publishing? The two produce different architectures, and vocabulary that conflates them hides the choice.

### Agent descriptor

The generic term for the structured, signed description of an agent: identity, capabilities, attributes, terms. The community also says agent card, which is one vendor lineage's name, and genesis document, which is one architecture's name for a descriptor whose hash is the identity. Descriptor is the vendor-neutral term for use in cross-organization text.

### Connection state, interaction state

Two kinds of state the stateless-versus-stateful argument conflates. Connection state is plumbing pinned to a socket. It scales badly and deserves removal. Interaction state is data, sessions, budgets, contracts, pending input, replicable like any record. Removing the first is engineering. Removing the second relocates it into clients, payloads, and middleware, where it interoperates with nothing. A design described as stateless should be asked which state it removed.

### Declares, attests, judges

The three relationships between a statement and its source, and the backbone of trust vocabulary. A document declares, self-asserted, signature proving only authorship. A certificate attests, a third party verified specific claims, with issuer, grant, and expiry, the TLS pattern. A score judges, one party's computed opinion over evidence. The three are routinely collapsed into "trust," and every collapse hides who is accountable for the statement being true.

### Automation bias, oversight

Human oversight degrades, through rubber-stamping, fatigue, and disengagement, and a governance design that assumes attentive humans at every checkpoint has assumed its scarcest resource is infinite. Levels L3 through L5 above are distinguished precisely by where human attention is required, which is why classifying the level honestly is a safety exercise rather than a marketing one. Detailed treatment in the governance glossary under human oversight drift.

---

## Part III. Deprecated usages

A short table for specification authors. The left column keeps appearing in drafts and charters; the right column is the accurate replacement.

| Deprecated | Preferred | Reason |
|---|---|---|
| autonomous agent | automated agent, delegated agent, or an L-level | authority is delegated, never self-originated |
| the AI decided | the system selected, the model returned | decisions imply accountability the system cannot hold |
| levels of autonomy | levels of automation | the SAE correction, adopted here |
| trust score (as the whole of trust) | declaration, attestation, or judgment, named | collapses three accountability relationships into one number |
| agent card (in cross-org text) | agent descriptor | vendor-neutral |
| registry (for live state) | population, presence | a registry stores; presence maintains |
| smart, intelligent (as system properties) | capable of, automates | unverifiable claims |
| hallucination (as malfunction) | inference error, plausible-but-false output | the mechanism worked as built |

---

## Contributing

This is a living document, versioned in the open, grown by addition and dispute. Propose terms, corrections, or counter-definitions by pull request, with the reasoning stated. Disagreements about individual entries are welcome and will be adjudicated on one criterion: does the proposed usage make specifications more accurate about where capability, state, and accountability actually live.

Companion resources. Breadth vocabulary: the [W3C AIKR CG Agentic AI Vocabulary](https://w3c-cg.github.io/aikr/VOCAB29June/). Governance depth: the [AI Governance Taxonomy and Reference Glossary](https://chrishood.com/ai-governance-taxonomy-reference-glossary/). Problem definitions: the [problem statement suite](../agent-standards-use-cases/) in this repository. Formal definitions of record: ISO/IEC 22989 for baseline AI terminology, cited above where this document diverges or extends.
