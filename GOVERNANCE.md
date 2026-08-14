# Governance

Grom Synthetic BR is maintained as an evidence-driven research project.

## Decision principles

Project decisions should be:

- reproducible;
- traceable to evidence;
- explicit about uncertainty;
- compatible with licensing and provenance requirements;
- reviewed according to their potential impact.

## Decision classes

### Ordinary implementation changes

Routine refactors, documentation corrections and non-behavioral maintenance may follow normal review.

### Research-sensitive changes

Changes affecting any of the following require stronger evidence and review:

- canonical geometry;
- renderer behavior;
- normative interpretation;
- glyph metrics or spacing policy;
- spatial-fit policy;
- font selection;
- dataset-generation policy;
- validation thresholds;
- baseline promotion;
- provenance or licensing rules.

## Baselines

A historical baseline must not be silently rewritten. New baselines should be versioned, justified by evidence and reviewed before promotion.

## Human review

Automated tests are necessary but not sufficient for decisions that materially affect canonical output, normative interpretation or dataset scale-up.

## Uncertainty

When evidence is insufficient, the preferred project state is `UNKNOWN`, `PARTIALLY_SPECIFIED` or an equivalent explicit limitation rather than an invented value.

## Independence

Grom Synthetic BR may support other Grom projects, but it remains independently versioned and independently useful. Integration should not create hidden dependencies that undermine reproducibility.

## Licensing

No technical benefit overrides licensing or provenance requirements. A useful asset with uncertain redistribution rights remains unsuitable for public distribution until its status is resolved.
