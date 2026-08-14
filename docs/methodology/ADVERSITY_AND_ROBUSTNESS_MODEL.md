# Adversity and Robustness Model

## Purpose

Grom Synthetic BR is designed around a two-stage research model:

1. generate a high-fidelity canonical Brazilian plate;
2. apply controlled, reproducible visual adversities to create difficult but traceable OCR training and evaluation samples.

The adversity layer is not intended to replace canonical generation. It depends on it. Every difficult sample must retain a verifiable relationship with its clean source and original ground truth.

## Research objective

The principal objective is to train and evaluate OCR systems under conditions substantially more difficult than clean laboratory imagery while preserving experimental traceability.

Rather than assuming that an OCR engine that performs well on ideal samples will generalize to real acquisition conditions, the project aims to generate a curriculum of controlled difficulty.

## Adversity families

Planned adversity families include:

- illumination: underexposure, overexposure, uneven light, glare and shadows;
- weather: rain, wet surfaces, fog, haze and atmospheric contrast loss;
- contamination: dirt, dust, mud and partial surface obstruction;
- aging and wear: fading, material degradation and damaged character regions;
- acquisition: blur, resolution loss, compression and perspective variation;
- visibility: partial occlusion and localized contrast loss;
- typography and layout stress: difficult glyph combinations and spacing-sensitive cases;
- controlled synthetic anomalies representing possible degradation or tampering-like appearances for defensive OCR robustness studies.

The project does not provide instructions for physically defeating identification systems. Tampering-like simulations are synthetic defensive test conditions whose purpose is to determine whether OCR systems remain reliable under difficult visual evidence.

## Non-destructive ground-truth rule

A transformation may reduce visual readability, but it must never silently change the semantic identity of the source sample.

For each generated sample the pipeline should retain at minimum:

- canonical source identifier;
- original plate text;
- generator version;
- canonical geometry version;
- adversity pipeline version;
- ordered transformation list;
- parameters for every transformation;
- deterministic/random seed where applicable;
- output checksum;
- generation timestamp or reproducible run identifier;
- license/provenance context for all required assets.

If a transformation makes the label ambiguous even to the generation pipeline or human validation policy, the sample must be flagged, quarantined or excluded according to a documented rule rather than silently included as ordinary training data.

## Difficulty levels

The future adversity engine should support versioned difficulty profiles rather than a single undifferentiated augmentation mode. A conceptual structure is:

- **L0 — Canonical:** clean reference, no adversity;
- **L1 — Mild:** small acquisition/environmental variation;
- **L2 — Moderate:** combinations that materially reduce OCR confidence while preserving clear ground truth;
- **L3 — Severe:** difficult but defensible synthetic evidence intended for robustness training;
- **L4 — Stress / Research:** edge cases and highly adverse combinations requiring additional human review and separate reporting.

Exact thresholds must be evidence-based and versioned. These labels are a methodological proposal, not normative categories.

## Reproducibility

Every adversity must be parameterized. Examples of required parameter classes include intensity, spatial mask, opacity, direction, blur kernel, perspective matrix, contamination coverage, contrast attenuation and random seed when relevant.

A published experiment should make it possible to answer:

> Which clean plate produced this image, which transformations were applied, in which order, with which parameters, under which code version?

If that question cannot be answered, the sample does not meet the project's target auditability standard.

## Plausibility and anti-artifact controls

Synthetic difficulty is useful only when it does not teach the OCR engine artifacts of the generator itself.

Therefore the project should evaluate:

- unrealistic repeated masks;
- visible synthetic boundaries;
- impossible lighting combinations;
- non-physical weather overlays;
- contamination patterns that correlate with particular labels;
- transformations that accidentally reveal the source label;
- transformations that systematically erase only specific glyph classes;
- distribution imbalance between clean and adverse samples.

The adversity engine should favor stochastic diversity under controlled parameter ranges rather than a small set of reusable visual templates.

## Training / evaluation isolation

Adversity generation must preserve the separation between training data and independent evaluation sets.

No Golden Set image is to be used as a training input, texture source, template or augmentation donor unless a future explicit policy changes that rule with appropriate provenance and licensing review.

Synthetic stress cases may be used to develop the generator, but evaluation claims must remain distinguishable from generator-internal test results.

## Metrics

Future evaluation should consider more than aggregate OCR accuracy. Useful metrics may include:

- exact plate accuracy;
- per-character accuracy;
- confidence calibration;
- performance by adversity family;
- performance by difficulty level;
- degradation curve from L0 to severe conditions;
- confused-pair error rates;
- failure rate by glyph position;
- abstention/inconclusive behavior;
- robustness gain relative to clean-only training.

## Human review

Human review remains mandatory before promoting a new adversity profile into a canonical dataset release when the transformation can materially alter visual interpretation.

Automated plausibility checks are necessary but not sufficient.

## Versioning

The canonical generator and adversity engine should be versioned independently so that a dataset can identify both components, for example:

- canonical renderer version;
- adversity-engine version;
- adversity-profile version;
- dataset manifest version.

This allows the project to improve environmental simulation without rewriting the historical meaning of earlier canonical baselines.

## Relationship to the project mission

The long-term value of Grom Synthetic BR is not simply the ability to render a large number of Brazilian plates. Its research value lies in producing **high-fidelity labeled sources and then subjecting them to controlled, known and reproducible difficulty**, enabling OCR robustness to be trained, measured and compared with evidence.