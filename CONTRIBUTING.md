# Contributing to Agent Standards

Thank you for considering a contribution. This repository exists because the agent-standards space has grown beyond what any single participant can track alone, across at least six venues. Community contributions keep it accurate, current, and honest.

## The routing rule

Different artifact families accept different kinds of claims. Contributions land cleanly when they go to the right place:

- **Venue landscape catalogs** are descriptive inventories. Additions, corrections, and clearer descriptions are welcome. Comparative claims between efforts are out of scope in the catalogs.
- **The landscape master index** is the only home for comparative and cross-venue findings. Disagreement with a finding goes here, as an issue or PR against the specific finding, never into the venue files.
- **Use-case problem statements** accept disputes against specific documents: a skeptical case made stronger, a failure example corrected, a dependency mis-mapped. New primitive proposals follow the reverse-coverage path: raise the concern as an issue, draft it in the suite's apparatus, and it enters with a matching rubric supplement.
- **The necessity assessment** accepts verdict disputes by PR against specific primitives, with reasoning stated so it can be attacked specifically rather than in aggregate.
- **The evaluation rubrics** accept criterion refinements and contributed depth supplements per primitive. Contributed criteria carry their contributor's name. Evaluations performed with the rubric are welcome as linked artifacts; self-evaluations are useful, independent evaluations are more honest.
- **The vocabulary** accepts term proposals and counter-definitions, adjudicated on one criterion: does the proposed usage make specifications more accurate about where capability, state, and accountability actually live.

## Ground rules

**Neutrality where the artifact promises it.** Catalogs and rubrics are reference material. Descriptions should be factual; where an approach is distinctive, describe the mechanism rather than editorializing about whether it succeeds.

**Positions where the artifact carries them.** The problem statements and the assessment take stated positions. Contributions may sharpen or dispute those positions; they should never smuggle new positions into the neutral artifacts.

**Attribution accuracy.** Authors, affiliations, dates, and version references should match the authoritative source (Datatracker for IETF drafts, the group or project pages elsewhere). If a draft moves streams (draft-lastname-* becoming draft-ietf-*), update the reference and note the transition.

**Category placement.** Categories reflect architectural role. Work touching multiple areas belongs primarily where its main scope lands, with cross-references. When in doubt, file an issue before opening a PR.

## What kinds of changes are welcome

**Additions**: new drafts, projects, groups, venues, or coordination efforts in any catalog; new user stories in the established format; new vocabulary terms; contributed rubric depth criteria.

**Corrections**: wrong names, attributions, or dates; outdated or superseded entries; miscategorization; broken links.

**Improvements**: clearer descriptions, better cross-references, style consistency.

**Structural changes**: new categories, new primitives, renamed or merged sections. Discuss via issue first.

## How to contribute

**File an issue** for small corrections, missing entries, observations, or anything needing discussion before a PR. Include the change, the reasoning if non-obvious, and a citation to the authoritative source.

**Open a pull request** for additions and clear-cut corrections:

1. Fork the repository
2. Create a branch (`git checkout -b add-draft-example`)
3. Edit the relevant file
4. Verify links resolve
5. Match the formatting of existing entries in the same file
6. Commit with a clear message and open a PR against `main`

### Landscape entry format (IETF catalog)

```markdown
- [draft-author-topic](https://datatracker.ietf.org/doc/draft-author-topic/) (Author, Affiliation; version and date if notable) — brief description of what the draft does, its architectural approach, and its relationship to other work in the category.
```

Descriptions run 1-3 sentences, factual. Entries are alphabetical within categories. External protocols and industry efforts go in the industry catalog or the External subsections, as the file indicates.

## Style

Match the file family you are editing. The landscape catalogs and these governance files use plain IETF-friendly prose: no em dashes, avoid negative contractions, active voice, technical accuracy over marketing language. The use-case suite has its own established apparatus and typography; edits to those documents should match the surrounding text rather than these rules. Neutral tone on contested categorization questions applies everywhere.

## Co-authorship

The current maintainer welcomes co-authors who agree with the general approach of a document or of the repository. Co-authorship means being named in the document header and AUTHORS.md, named on any I-D snapshots filed to the Independent Submission Stream, and holding merge rights. File an issue titled "Co-authorship inquiry" describing your interest and the perspective you would bring. This is a standing commitment: co-authors review PRs, respond to issues in their areas, and help maintain the material over time. Editorship of any artifact can move to a group if one forms around it.

## Code of conduct

Discussion in issues and PRs should be technical, substantive, and respectful. Disagreements about categorization, verdicts, and scope are welcome; that is what the routing rule is for. Personal attacks, promotional posts, and bad-faith engagement are unwelcome.

The IETF Note Well applies to any material that references or is derived from this repository.

## Questions

For anything not covered here, file an issue or contact the maintainer (see README.md).
