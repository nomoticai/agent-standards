# Agent Standards Evaluation

The judgment layer of this repository. The problem statement suite deliberately withholds verdicts; this directory renders them, and provides the instrument for grading any proposal's treatment of the problems.

## The documents

### [Necessity Assessment](necessity-assessment.md)

For each of the fifteen primitives, a verdict on whether a standard is required at all, under one named test: standardize what strangers must interpret identically; let the market provide what parties choose between. Each verdict states its reasoning, cites a precedent (WebPKI, ISO 8583, addr-spec, the credit bureaus, and others), and distinguishes the thin standardizable core from the parts that stay enterprise software. The verdicts are opinionated by design and disputable by pull request against specific primitives.

### [Evaluation Rubrics](agent-protocol-evaluation-rubrics.md)

Seventeen core criteria plus per-primitive supplements (TPT through TXN) for evaluating any draft, protocol, or project's treatment of the agent infrastructure problems. The discipline: evidence over opinion, a citation for every grade, running-code claims graded on a ladder from simulation to independent interop to production, no composite scores, no rankings, and self-grading first. Criteria are written to pass a neutrality test: architecturally opposed designs can each reach the top grade by stating and analyzing their choices. Contributed depth criteria per primitive are invited and carry their contributor's name.

### Applied analyses

Worked analyses produced with these instruments accumulate here alongside them.

- [Agent Discovery Options](agent-discovery-options.md) — discovery approaches compared against the same requirements: query models, scenario coverage, and the architectural consequences of each option.

Evaluations of specific proposals performed with the rubric are welcome as linked artifacts or additions to this directory. Self-evaluations are useful; independent evaluations are more honest.

## How this directory relates to the others

The [problem statements](../agent-standards-use-cases/) define the problems and withhold judgment. The [landscape catalogs](../agent-standards-landscape/) inventory what exists and withhold comparison. This directory is where judgment and comparison live, in instruments built to be checked: every verdict carries its test and precedent, every grade carries its citation, and disputes route against the specific verdict or criterion rather than against the documents in aggregate.

## How to contribute

**Dispute a verdict** by pull request against the specific primitive in the necessity assessment, with reasoning stated.

**Refine a criterion or contribute depth supplements** by issue or pull request against the rubric; the depth-supplement path and naming convention are described in the rubric itself.

**Submit an evaluation** performed with the rubric, as a linked artifact or a file in this directory, with citations per grade.

Entry conventions and style are in the repository [CONTRIBUTING.md](../CONTRIBUTING.md).

## License

Released under the [Creative Commons Attribution 4.0 International License (CC-BY-4.0)](../LICENSE). You are free to share and adapt this material with attribution.

## Maintainers

- Chris Hood ([@nomoticai](https://github.com/nomoticai)) — original author, current maintainer

Additional co-authors and maintainers welcome. See [CONTRIBUTING.md](../CONTRIBUTING.md).
