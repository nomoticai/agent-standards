# Agent Standards

A community-maintained reference layer for AI agent standardization: solution-agnostic problem statements, multi-venue landscape catalogs, a necessity assessment, evaluation rubrics, and a working vocabulary.

## What this is

Agent standardization is happening in at least six venues at once (IETF, W3C, Linux Foundation, IEEE, ISO/IEC, ITU), across hundreds of drafts, projects, and community groups, with no official coordination between them. No single participant can hold the full picture in mind. This repository provides the shared map: what the problems are, what exists, which problems require standards at all, and how to evaluate any specific proposal's treatment of them.

The repository holds four artifact families, each in its own directory:

### [agent-standards-use-cases/](agent-standards-use-cases/)

Fifteen solution-agnostic problem statements covering the coordination primitives of agent infrastructure: transport, identity, authorization, delegation, trust, discovery, communication, session, semantics, tools, commerce, audit, governance, naming (as an open question), and transactions. Each follows a shared apparatus: a strongest-form skeptical case, observable present-day failures, properties without a common definition, prior art treated as constraint, an honest account of what no solution can promise, explicit non-claims, and a declared dependency map. The suite deliberately withholds verdicts; the assessment below renders them.

### [agent-standards-landscape/](agent-standards-landscape/)

Living catalogs of agent-related standards activity, one per venue (IETF, W3C, Linux Foundation, IEEE, ISO/IEC, with ITU and industry catalogs in progress), plus a master index. The venue catalogs are descriptive inventories. The master index is the only place comparative and cross-venue findings appear, written so each finding can be checked against the catalogs. Counts live in the venue files so they stay current without dating the index.

### [agent-standards-evaluation/](agent-standards-evaluation/)

The judgment layer, in two documents. The necessity assessment renders a verdict per primitive on whether a standard is required at all, under one named test: standardize what strangers must interpret identically; let the market provide what parties choose between. Each verdict cites a precedent and is disputable by pull request. The evaluation rubrics provide seventeen core criteria plus per-primitive supplements for grading any draft's treatment of these problems: evidence over opinion, citations required, no composite scores, no rankings, self-grading first.

### [agent-standards-vocabulary/](agent-standards-vocabulary/)

A recommended taxonomy for engineering accuracy: levels of agent automation (the SAE J3016 analog for agents), corrections for terms the industry conflates (automation and autonomy, search and discovery, catalog and population, name and identifier and identity and address), and deprecated usages with replacements. Published deliberately as a living recommendation rather than a standard.

## How the pieces fit

The suite says what the problems are. The assessment says which require standards. The landscape says what exists. The rubric says how well any of it treats the problems. The vocabulary keeps the words accurate while the rest of it is argued. The loop is designed to close: evaluations surface missing primitives, missing primitives become problem statements, problem statements gain rubric supplements, and the suite versions upward. Section 15 (Transactions) was the first document produced by that loop.

## What this is not

This repository does not advocate for any particular proposal. The problem statements and the necessity assessment carry stated, disputable positions, especially on the line between internet standards and enterprise software. The landscape catalogs and the rubric are built as unbiased reference material and are governed to stay that way.

## How to contribute

Every artifact family accepts contributions, with routing rules that keep disputes where they belong. See [CONTRIBUTING.md](CONTRIBUTING.md). The short version: venue catalogs stay descriptive and comparative claims go to the master index; suite disputes go against specific documents; verdict disputes go against specific primitives; rubric contributions follow the depth-supplement path; vocabulary proposals argue one criterion, whether the usage makes specifications more accurate about where capability, state, and accountability live.

## Snapshots and citation

The files in this repository are the current source of truth; point to their URLs for the always-current view. Periodic snapshots may be filed as IETF Internet-Drafts under the Independent Submission Stream for citation stability. See [SNAPSHOTS.md](SNAPSHOTS.md) where present in each directory.

## License

Released under the [Creative Commons Attribution 4.0 International License (CC-BY-4.0)](LICENSE). You are free to share and adapt this material with attribution.

## Maintainers

- Chris Hood ([@nomoticai](https://github.com/nomoticai)) — original author, current maintainer

Additional co-authors and maintainers welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) and [AUTHORS.md](AUTHORS.md).

## Acknowledgements

This repository reflects the collective work of hundreds of contributors across the IETF, W3C, Linux Foundation, IEEE, ISO/IEC, ITU, and industry. Every draft, project, and group catalogued here represents someone's substantive engagement with the agent standards question. The categorization and the verdicts are one editorial view; the work itself belongs to the community.
