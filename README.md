# Grom Synthetic BR

**Auditable synthetic Brazilian license plate generation and research framework for OCR, computer vision and reproducible experimentation.**

> Project status: **Research / Experimental**. This repository is not a homologated identification system and is not intended to replace human review, official records, or legally required validation procedures.

[Português do Brasil](README.pt-BR.md)

## Mission

Grom Synthetic BR was created to address a practical and research gap encountered during the development of Brazilian OCR and vehicle-identification systems: there is a large body of open material for synthetic data, OCR, ANPR/ALPR and computer vision, but comparatively little reusable, auditable and openly documented material centered on **Brazilian license plates**, their regulatory context and the specific geometry and visual constraints of Brazilian PIV/Mercosul plates.

During the development process, the project found extensive international material intended for research and experimentation, but no sufficiently complete open Brazilian dataset or implementation that could simply be adopted for the required purpose. As a result, a substantial part of the methodology, validation logic, audit trail and synthetic-generation pipeline had to be developed from first principles, with a strong focus on reproducibility and traceability.

The project therefore aims not only to support its own OCR research, but also to leave a useful, transparent and reusable contribution to the academic, scientific and open-source communities.

## Core purpose: fidelity first, then controlled adversity

Grom Synthetic BR is **not merely a generator of visually correct plates that follow Brazilian regulatory geometry**.

Its central research purpose is to create a high-fidelity canonical Brazilian plate first, then transform that canonical reference into controlled, reproducible and measurable difficult-reading scenarios for OCR training and robustness research.

The intended pipeline is conceptually:

`normative / high-fidelity canonical plate -> controlled visual adversity -> labeled synthetic sample -> OCR training / robustness evaluation`

The project is being designed to model adverse visual conditions such as:

- strong or insufficient illumination;
- glare, highlights, shadows and uneven exposure;
- rain and wet-surface effects;
- fog, haze and atmospheric loss of contrast;
- dirt, mud, dust and partial contamination;
- aging, degradation and material wear;
- faded or damaged characters;
- partial occlusion;
- blur and acquisition-related loss of detail;
- perspective and viewing-angle variation;
- difficult combinations of glyphs and character spacing;
- controlled synthetic anomalies representing degradation or possible tampering/adulteration patterns for **defensive OCR robustness research**.

These transformations must not be arbitrary image corruption. Each adversity should be parameterized, versioned and recorded in metadata so that experiments remain reproducible and the exact conditions that produced a sample can be reconstructed.

The objective is deliberately demanding: an OCR system should not learn only from clean, ideal plates. It should be exposed to progressively harder but traceable examples so that robustness can be measured instead of assumed.

The project therefore distinguishes two layers:

1. **Canonical fidelity layer** — generate the cleanest and most defensible Brazilian plate representation possible from documented evidence.
2. **Adversity / robustness layer** — apply controlled degradations after canonical generation, without silently changing the underlying ground truth.

A degradation must never redefine the plate identity. The original clean plate, its text label, generation parameters and every applied transformation must remain traceable.

## Why this project exists

Grom Synthetic BR focuses on a problem that is frequently underrepresented in open research assets: **Brazilian-domain synthetic license plate data with explicit methodological, normative and provenance controls**.

The project is designed around the following principles:

- reproducible generation;
- high-fidelity canonical generation before degradation;
- controlled and traceable adversity simulation for OCR robustness;
- strict separation between normative facts, engineering assumptions and experimental hypotheses;
- auditability of every relevant decision;
- deterministic tests and frozen historical baselines;
- human review before promotion of new baselines;
- no silent geometric deformation to make difficult samples pass;
- explicit licensing and provenance of third-party assets;
- isolation of synthetic training data from independent evaluation/golden sets;
- preservation of research limitations rather than hiding them through filtering.

## Research contribution

The intended contribution is broader than a dataset generator. The project is building a documented research framework for:

- synthetic Brazilian license plate generation;
- high-fidelity canonical reference generation;
- controlled environmental and visual adversity simulation;
- curriculum-style generation from clean to progressively difficult samples;
- OCR-oriented glyph and layout analysis;
- reproducible stress cases for wide, narrow and easily confused characters;
- normative-source traceability;
- spatial-fit and plate-occupancy analysis;
- generation metadata capable of reconstructing each synthetic transformation;
- license and asset provenance;
- deterministic regression testing;
- controlled progression from laboratory evidence to human-reviewed baselines.

Researchers, developers, students and institutions are encouraged to reuse the methodology, reproduce experiments, challenge assumptions, report limitations and contribute improvements that can benefit the wider community.

## Brazilian regulatory context

The current research references the Brazilian PIV/Mercosul regulatory framework, including CONTRAN Resolution 969/2022 and its annexes. Regulatory material is treated as a **source of evidence**, not as a substitute for engineering validation.

The project explicitly distinguishes:

- **NORMATIVE_FACT** — directly supported by an official normative source;
- **DOCUMENTED_REFERENCE** — supported by a relevant technical or documentary source;
- **MEASURED_FROM_NORMATIVE_FIGURE** — only when the figure is metrologically defensible;
- **DERIVED_VALUE** — reproducible derivation from documented evidence;
- **ENGINEERING_INFERENCE** — engineering interpretation, not a legal requirement;
- **UNKNOWN** — evidence is insufficient and must not be silently invented.

## Current research status

The current line of work includes:

- canonical Brazilian Mercosul plate generation;
- historical/legacy plate isolation;
- normative geometry review;
- character-width and confused-pair studies;
- spatial-fit analysis around QR and OCR regions;
- typographic candidate evaluation;
- adversity-model architecture for future synthetic robustness generation;
- deterministic audit reports and regression tests.

Important current constraints:

- no third-party font is accepted solely because it visually resembles FE Engschrift;
- third-party fonts with uncertain or restrictive licensing are not redistributed;
- the current renderer remains an experimental approximation subject to documented review;
- difficult strings are preserved as stress cases rather than removed from the problem space;
- large-scale adversity generation must not begin until canonical geometry is sufficiently stable and reviewed.

## Validation philosophy

Automated tests are necessary but **not sufficient** for promotion of a new canonical baseline.

A successful automated gate does not automatically authorize:

- a new font;
- a new baseline;
- large-scale dataset generation;
- training;
- normative claims.

Human review remains an explicit stage in the research process.

The same rule applies to adversity simulation: visually dramatic samples are not automatically useful training data. Each transformation must preserve label correctness, remain plausible enough for its research purpose and be represented in metadata.

## Research sentinels

The project uses controlled stress strings to expose different classes of failure. Examples include:

- `ABC1D23` — historical control;
- `IJI1I11` — narrow-glyph sentinel;
- `BOS5S68` — intermediate control;
- `ZGQ2G62` — spatial/QR/descender sentinel;
- `MWQ8O01` — wide-glyph stress case;
- `QMW0O80` — wide-glyph and O/0-confusion stress case.

These are synthetic research controls. Their inclusion does not imply that a specific plate with that exact registration was observed in circulation.

## Relationship with other Grom projects

Grom Synthetic BR is designed as an **independent research component** that may support other systems without becoming tightly coupled to them.

Related systems under development include:

- **Grom OCR Core 3.0** — OCR and license-plate processing;
- **Grom Vehicle Vision (GVV)** — vehicle make/model/generation, color and visual characteristics;
- **GIP** — integrated investigation-support environment intended to orchestrate independent analysis systems.

Public links will be added as individual repositories become suitable for public release. Independence between projects is intentional: this repository should remain useful to researchers who do not use the rest of the Grom ecosystem.

## Repository policy

The public repository is intended to contain only material whose publication is compatible with the project licensing and provenance rules.

Do **not** assume that an asset is redistributable merely because it was used in a local laboratory experiment.

In particular:

- fonts require explicit license review;
- third-party datasets require explicit provenance and license review;
- official or third-party imagery is not automatically redistributable;
- local research candidates may remain outside Git even when technically useful;
- generated datasets may have a licensing policy distinct from the source code.

See `docs/licensing/` for the licensing model.

## Licensing

The project uses a layered licensing approach:

- **source code:** Apache License 2.0;
- **original project documentation:** CC BY 4.0 unless a file states otherwise;
- **third-party assets, fonts, models and datasets:** governed by their own licenses and provenance records;
- **generated datasets:** subject to the dataset policy and the licenses/provenance of all inputs used to produce them.

The Apache License 2.0 is intentionally permissive and supports reuse, modification and redistribution while preserving notices and contribution terms. The project also maintains a `NOTICE` file and dedicated third-party licensing documentation.

## Academic citation

A `CITATION.cff` file is provided so that GitHub and academic tools can generate a consistent software citation.

If this work contributes to a paper, thesis, dataset, benchmark, software project or technical report, please cite the repository and identify the version or commit used.

## Contribution and reciprocity

Open source works best when improvements flow in both directions.

The license does not impose an additional non-standard obligation to publish every derivative work. However, researchers and developers who benefit from this project are **strongly encouraged** to:

- cite the project when used in research or publication;
- document relevant modifications;
- report reproducible defects and limitations;
- contribute corrections and improvements upstream when possible;
- share benchmark methodology and non-sensitive evaluation results;
- preserve attribution and provenance;
- avoid presenting experimental results as homologated or official conclusions.

See `CONTRIBUTING.md` for contribution requirements.

## Security, privacy and responsible use

This repository is intended for research, software engineering and lawful computer-vision experimentation.

Contributions must not include personal data, real-world sensitive investigative records, confidential case material, credentials, private keys or restricted datasets.

Synthetic degradation or tampering-like scenarios are intended exclusively to improve defensive OCR robustness and evaluation; they are not guidance for physically defeating or evading identification systems.

See `SECURITY.md` for vulnerability reporting and responsible disclosure.

## Governance

Technical decisions should be evidence-based, reproducible and reviewable. Changes that affect canonical geometry, normative interpretation, provenance, licensing or baseline validity require stronger review than ordinary implementation changes.

See `GOVERNANCE.md`.

## Roadmap

Near-term work includes:

1. consolidate the public repository and licensing inventory;
2. complete spatial-policy and plate-occupancy review;
3. preserve and publish auditable normative-study artifacts where redistribution is appropriate;
4. stabilize the canonical generator before large-scale generation;
5. implement a versioned, parameterized adversity engine for illumination, weather, contamination, degradation, wear, blur, perspective and related robustness conditions;
6. define versioned dataset releases and reproducible manifests;
7. publish benchmark methodology independently from training data;
8. integrate, when appropriate, with Grom OCR Core 3.0 and other independent consumers.

## Disclaimer

This project is experimental research software. It is not an official implementation of a Brazilian public authority, is not endorsed by CONTRAN or any licensing authority, and must not be presented as a homologated evidentiary or identification system.

Normative references are cited for research and traceability. Interpretations, engineering policies and synthetic approximations remain the responsibility of the project and must be identified as such.

## Author

**Josuel Grom**

Project origin and research direction: Grom Synthetic BR / Grom OCR Training Lab.

---

If you find this work useful, reproducibility, citation and constructive contribution are the best forms of support.