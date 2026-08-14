# Weakness-Driven Adaptive Training

## Purpose

Grom Synthetic BR is intended to do more than generate large quantities of synthetic Brazilian license plates. Its long-term research model is to identify where OCR systems fail, reproduce those weaknesses under controlled synthetic conditions, intensify the relevant difficulty gradually, and use the resulting evidence to guide new training rounds.

The project therefore treats difficulty as a measurable research variable rather than a random augmentation side effect.

## Core loop

The intended cycle is:

1. generate a high-fidelity canonical plate with known ground truth;
2. apply a versioned sequence of controlled adversities;
3. evaluate one or more OCR systems at each adversity level;
4. determine the point at which exact interpretation begins to degrade;
5. attribute the failure to adversity family, glyph, position, confusion pair or interaction when possible;
6. record the weakness in a structured weakness registry;
7. generate additional synthetic examples targeted at the underperforming region;
8. retrain or fine-tune under a controlled curriculum;
9. evaluate again on independent validation material;
10. determine whether the failure boundary moved toward more difficult conditions.

Conceptually:

`Generate -> Stress -> Evaluate -> Diagnose -> Rank weaknesses -> Targeted generation -> Retrain -> Re-evaluate`

## Failure boundary

For a plate `X`, the research question is not only whether OCR can read it, but **how far the plate can be degraded before interpretation becomes unreliable**.

A plate may progress through a reproducible path such as:

- L0: canonical;
- L1: mild adversity;
- L2: moderate adversity;
- L3: severe adversity;
- L4: stress/research.

The transition between stable interpretation, partial degradation, abstention and failure forms a measurable failure boundary.

Training improvement should be demonstrated by moving this boundary toward more difficult conditions without degrading clean or previously mastered cases.

## Weakness registry

Every meaningful failure should be representable as structured evidence. Suggested fields include:

- source plate identifier;
- ground truth;
- OCR engine and model version;
- canonical renderer version;
- adversity engine version;
- adversity profile and ordered transformations;
- intensity parameters;
- seed;
- OCR prediction;
- confidence;
- exact-match status;
- per-character errors;
- confused pair, when applicable;
- glyph position;
- adversity family;
- interaction families;
- failure level;
- reproducibility status;
- review status;
- priority score;
- remediation/training cohort identifier.

The weakness registry should support aggregation by character, pair, position, adversity, severity and OCR model.

## Weakness-driven sampling

Training should not simply maximize the number of examples. It should allocate additional synthetic coverage to underperforming regions while preserving a stable mixture of:

- clean canonical samples;
- previously mastered adversities;
- random broad-distribution adversities;
- newly discovered hard cases;
- targeted weakness cohorts.

This reduces the risk of training primarily on easy cases while also avoiding catastrophic forgetting or overfitting to a narrow family of synthetic artifacts.

## Independent evaluation

Independent evaluation material must remain isolated from training.

If an independent evaluation set reveals a weakness, the project may use the **class of weakness** to generate new synthetic training examples, but should not copy the evaluation image, its pixels, texture, mask or exact acquisition artifacts into training.

Example:

`evaluation reveals glare-related O/0 failures -> generate new independent synthetic O/0 + glare cohorts -> retrain -> re-evaluate on untouched evaluation material`

## Metrics

The adaptive loop should track more than aggregate accuracy. Recommended metrics include:

- exact plate accuracy;
- per-character accuracy;
- confidence calibration;
- abstention/inconclusive rate;
- failure boundary by adversity family;
- failure boundary by glyph and position;
- confused-pair error rates;
- interaction effects between adversities;
- robustness gain after targeted training;
- clean-condition regression;
- previously-mastered-condition regression;
- synthetic-to-independent-evaluation generalization gap.

## Promotion rule

A targeted training change should not be considered an improvement merely because it raises performance on the hard cases that motivated it.

Promotion requires evidence that:

1. the targeted weakness measurably improved;
2. clean performance did not regress beyond the accepted tolerance;
3. previously mastered adversity families did not materially regress;
4. independent evaluation remains isolated;
5. the gain is reproducible;
6. the training cohort and generation parameters are fully traceable.

## Research principle

The project should spend progressively more research effort on **measured weaknesses**, not on producing ever larger volumes of samples that the OCR already reads reliably.

This principle is called **Weakness-Driven Adaptive Training**.

Its goal is not to make synthetic data artificially difficult for its own sake. The goal is to create a controlled feedback system in which failure evidence guides the next generation and training cycle, allowing robustness improvements to be measured instead of assumed.