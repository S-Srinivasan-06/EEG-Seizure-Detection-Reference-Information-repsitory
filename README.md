# Automated EEG Seizure Detection

A critical research review of methods, datasets, evaluation practices, and clinical evidence through **30 September 2026**.

Electroencephalography (EEG) records electrical activity using electrodes on the scalp or, in selected clinical circumstances, inside the skull. Automated seizure detection aims to identify seizures in these recordings. The practical challenge is to identify clinically relevant events during prolonged monitoring without producing an excessive number of false alarms or requiring more review than the system saves.

This repository explains the field's development, examines the strength of its evidence, and proposes studies that could establish whether promising algorithms are useful in clinical practice. It is intended for students, researchers, clinicians, and readers approaching EEG signal processing for the first time. Technical terms are explained in the [glossary](docs/glossary.md).

## Main findings

**Detecting labeled clips and detecting seizures during continuous monitoring are different tasks.** A clip is a short section of EEG. A classifier may separate selected seizure and nonseizure clips well while producing many false alarms during a full recording. Clinical evaluation needs seizure-event sensitivity, false alarms per monitored day, detection delay, and the time required for human review. The [SzCORE framework](https://doi.org/10.1111/epi.18113) provides standardized research evaluation conventions.

**Useful detection and review assistance have been demonstrated in particular settings.** Evidence includes prospective epilepsy monitoring unit studies, external testing across datasets, an adult review pilot, and a randomized neonatal assistance trial. Each supports a defined claim about its population and endpoint. Evidence for a universal detector, sustained staffing reductions, or improved patient outcomes is less established. See [clinical validation](docs/08-clinical-validation.md).

**Evaluation design often matters more than the architecture's name.** Test recordings should come from people who were not used to train or select the model. Testing at a different hospital provides a further assessment of generalization. Randomly distributing windows from the same patient between training and testing can produce misleading results, as illustrated by [Brookshire and colleagues](https://doi.org/10.3389/fnins.2024.1373515).

**A scalp EEG benchmark does not measure detection of every intracranial seizure.** Some seizures identified by intracranial recording have no visible scalp ictal pattern in simultaneous recordings. This is a signal-visibility issue that must be distinguished from an algorithm error. The [simultaneous scalp and SEEG study](https://doi.org/10.1097/WNP.0000000000000739) supports this distinction in a selected surgical population.

**No architecture is established as the best detector across all settings.** Convolutional networks, recurrent networks, graph models, transformers, and pretrained encoders address different representation and learning problems. Comparisons are meaningful only when datasets, patient separation, labels, tuning, and metrics are compatible. See [state of the art by category](docs/10-state-of-the-art.md).

These conclusions are qualified individually in the chapters. They should not be interpreted as a single performance ranking.

## Suggested reading paths

| Reader | Suggested sequence |
|---|---|
| New to EEG or machine learning | [Executive synthesis](docs/01-executive-synthesis.md), [clinical problem](docs/02-clinical-problem.md), [glossary](docs/glossary.md), then [direct answers](docs/14-direct-answers.md) |
| Student studying detection methods | [History](docs/03-history.md), [representations](docs/04-representations.md), [datasets and evaluation](docs/05-datasets-and-evaluation.md), then [methods and pretraining](docs/07-methods-and-pretraining.md) |
| Researcher planning a study | [Evaluation](docs/05-datasets-and-evaluation.md), [seed-paper appraisal](docs/06-seed-paper-appraisal.md), [research gaps](docs/11-research-gaps.md), and [research program](docs/13-research-program.md) |
| Clinician or professor assessing practical value | [Clinical problem](docs/02-clinical-problem.md), [clinical validation](docs/08-clinical-validation.md), [agreement and interpretation](docs/09-agreement-and-interpretation.md), and [open questions](docs/12-open-questions.md) |
| Reader checking a claim or reference | [Evidence policy](docs/evidence-policy.md), [master evidence table](docs/15-evidence-and-references.md#master-evidence-table), [claim-evidence matrix](docs/15-evidence-and-references.md#claim-evidence-matrix), and [annotated bibliography](docs/15-evidence-and-references.md#annotated-bibliography) |

## Complete dossier

The chapters follow a single argument: define the clinical task, understand the methods and data, assess the evidence, and design the studies needed to resolve the remaining questions.

| Part | Chapter | Central question |
|---:|---|---|
| 1 | [Executive synthesis](docs/01-executive-synthesis.md) | What is established, and how confident should we be? |
| 2 | [Clinical problem](docs/02-clinical-problem.md) | Why is prolonged EEG review difficult, and what assistance is needed? |
| 3 | [Historical roadmap](docs/03-history.md) | Why did methods change, and were the proposed improvements demonstrated? |
| 4 | [Representations](docs/04-representations.md) | What information does each representation preserve or assume? |
| 5 | [Datasets and evaluation](docs/05-datasets-and-evaluation.md) | What do dataset construction, patient separation, and scoring change? |
| 6 | [Seed-paper appraisal](docs/06-seed-paper-appraisal.md) | What does DGDCN demonstrate, and where is its evidence incomplete? |
| 7 | [Methods and pretraining](docs/07-methods-and-pretraining.md) | What have successive model families contributed? |
| 8 | [Clinical validation](docs/08-clinical-validation.md) | What has been measured in actual clinical populations and workflows? |
| 9 | [Agreement and interpretation](docs/09-agreement-and-interpretation.md) | Which findings agree, which conflict, and what do explanations mean? |
| 10 | [State of the art by category](docs/10-state-of-the-art.md) | Which studies provide the strongest evidence for each use case? |
| 11 | [Research gaps](docs/11-research-gaps.md) | Which gaps matter most, and which technologies plausibly address them? |
| 12 | [Open questions](docs/12-open-questions.md) | What is understudied, disputed, or blocking deployment? |
| 13 | [Research program](docs/13-research-program.md) | Which experiments would resolve the most important uncertainties? |
| 14 | [Direct answers](docs/14-direct-answers.md) | How should the field's main questions be answered concisely? |
| 15 | [Evidence and references](docs/15-evidence-and-references.md) | Which sources support the claims, and what are their limitations? |

## Scope

The central topic is **EEG-based seizure detection**, which identifies a seizure that is occurring or has occurred. Seizure prediction and forecasting estimate future risk. Diagnosis determines whether a person has epilepsy; seizure classification identifies seizure types; localization estimates where a seizure begins. These related tasks are included only when their methods, datasets, or evidence directly inform detection.

The following settings are assessed separately:

| Setting or recording modality | Why it requires separate evaluation |
|---|---|
| Scalp EEG in an epilepsy monitoring unit | Full electrode coverage, supervised acquisition, and a selected epilepsy population |
| Intracranial EEG, including stereo-EEG | Contacts sample selected brain regions, with different signal characteristics and patient selection |
| Adult intensive care EEG | Acute illness, artifacts, rhythmic or periodic patterns, and uncertain seizure boundaries |
| Neonatal EEG | Different developmental physiology, seizure morphology, annotation, and treatment context |
| Ambulatory, wearable, or subscalp EEG | Reduced coverage, prolonged recording, device-specific signals, and variable acquisition conditions |
| Multimodal monitoring | Additional signals such as ECG or movement can change both detection coverage and failure modes |

A result from one setting is not assigned to another without direct validation.

## How to interpret the evidence

Quantitative claims retain the following labels:

| Label | Meaning |
|---|---|
| [V] | Checked in a primary publication, primary abstract, original dataset record, or official source during preparation of this review |
| `[S: source]` | Taken from the named secondary source |
| `[U]` | Unverified or recalled; it is not accepted as a factual performance result |
| `Not found` or `NR` | Not located in the material inspected; this does not establish absence from the full paper |
| `Proposed design` | A recommended study requirement or target, rather than an observed result |

Primary-source verification confirms what a source reports. It does not mean that a model was independently reproduced. An abstract can verify a headline result while leaving the split, operating point, or scoring rules unresolved.

Conclusions carry **high**, **moderate**, **low**, or **emerging** confidence, with reasons. Confidence depends on independent replication, patient numbers, patient separation, external or prospective validation, and access to data and code. It does not depend simply on the number of papers reporting similar scores. The full policy and access limitations are in [evidence policy](docs/evidence-policy.md).

## The seed paper

Zhang X, Dai C, and Guo Y. *Dynamic Graph Convolutional Network with Dilated Convolution for Epilepsy Seizure Detection*. Bioengineering. 2025;12(8):832. [DOI: 10.3390/bioengineering12080832](https://doi.org/10.3390/bioengineering12080832). [PMCID: PMC12383979](https://pmc.ncbi.nlm.nih.gov/articles/PMC12383979/).

The paper provided the starting point for citation chaining and a detailed case study. The review covers the wider field. Its [critical appraisal](docs/06-seed-paper-appraisal.md) separates reported clip-classification gains from untested continuous detection and clinical claims.

## Repository contents

```text
README.md
CONTRIBUTING.md
.gitignore
docs/
 01-executive-synthesis.md
 02-clinical-problem.md
 03-history.md
 04-representations.md
 05-datasets-and-evaluation.md
 06-seed-paper-appraisal.md
 07-methods-and-pretraining.md
 08-clinical-validation.md
 09-agreement-and-interpretation.md
 10-state-of-the-art.md
 11-research-gaps.md
 12-open-questions.md
 13-research-program.md
 14-direct-answers.md
 15-evidence-and-references.md
 evidence-policy.md
 glossary.md
```

The public material is documentation. It does not contain an implemented detector, model weights, patient recordings, or performance reproduction results. Local drafting notes and maintenance scripts are excluded from version control.

## Corrections and contributions

Corrections should identify the affected claim and provide the primary source and the relevant table, figure, or passage. New performance claims need the population, split, scoring unit, and false-alarm denominator. Please follow the [contribution guide](CONTRIBUTING.md).

This edition is fixed to the evidence search completed on **30 September 2026**. It is a critical literature dossier with documented access limits; it is not an exhaustive systematic review. Cite the original publication when using a study's findings.
