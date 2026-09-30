# Evidence Policy and Review Limitations

[Repository overview](../README.md) | [Evidence and references](15-evidence-and-references.md)

## Scope and search date

The evidence search for this edition ended on **30 September 2026**. The review concerns EEG seizure detection. Related prediction, diagnosis, classification, and localization studies are included only where they contribute directly relevant methods, datasets, or evidence.

The search followed references and citing studies from the seed paper, SzCORE, major TUH corpus publications, graph methods, clinical validation studies, reviews, and dataset papers. Original publications, original dataset records, official conference proceedings, author repositories, and official regulatory records support the claims. Citation discovery and bibliographic verification are distinguished from full-text methodological appraisal.

Authenticated Web of Science and Scopus searches were unavailable. A reproducible search export and a complete screening flow were not prepared. Consequently, the repository presents a targeted critical research review rather than a complete systematic review or meta-analysis. It does not provide a global count of all recent claims evaluated under SzCORE.

## Evidence labels

[V] identifies a number checked in a primary paper, primary abstract, original dataset record, or official source during preparation. Abstract-only checks are marked where relevant. `[S: source]` identifies a number obtained from the named secondary source. `[U]` identifies unverified information and cannot support a factual performance conclusion.

"Not found" and `NR` mean that a field was not located in the inspected material. They do not mean that the full paper omitted it. Code marked "not found" may exist elsewhere. Conversely, a public repository establishes availability without establishing that its results can be reproduced.

"Proposed design" identifies a research requirement or target recommended in the agenda. It is not an empirical result, regulatory standard, or universally accepted clinical threshold.

## Confidence in conclusions

| Confidence | Interpretation |
|---|---|
| High | Strong support for the precisely stated claim, with relevant primary evidence, reliable methods, and corroboration where needed |
| Moderate | Support exists, but cohort size, independence, external testing, access, or replication restricts the conclusion |
| Low | Evidence is sparse, indirect, inconsistent, or insufficient for the intended inference |
| Emerging | A developing approach has relevant early results but limited independent or clinical validation |

Confidence is assigned to conclusions, not to architecture names or journal prestige. Several papers using the same weak split do not establish strong independent replication. A highly reliable statement that a metric is inappropriate should not be mistaken for strong evidence that every evaluated model fails.

Guidelines and consensus inform definitions and validation expectations. Systematic reviews summarize the literature but cannot repair shared weaknesses in the underlying studies. Prospective, multicenter, external, and patient-independent studies generally support stronger transport or utility claims than selected internal benchmarks, provided their endpoints match the claim.

## Distinctions retained throughout the review

Performance figures are not pooled across scalp, intracranial, neonatal, ICU, wearable, or multimodal settings. Sample classification, clip classification, seizure-event detection, record screening, seizure-hour recognition, and patient-level recognition remain distinct endpoints.

Clinical benefit is also assessed in stages: an algorithm emits a candidate; a clinician confirms it; staff respond; treatment may change; outcomes may improve. Evidence at an earlier stage does not establish benefit at a later one. Reader time, selected data duration, earlier retrospective flags, and actual response times are reported separately.

Sensitivity must be interpreted with continuous negative exposure, false alarms, delay, uncertainty, and patient distributions. AUC and accuracy alone do not establish useful alarm performance. Thresholds and postprocessing selected using an external test set cannot support a claim of fully frozen external testing.

## Important qualifications

| Topic | Qualification retained in the public chapters |
|---|---|
| DGDCN | The public implementation uses sample-specific attention to modulate fixed graph supports. Code inspection is not result reproduction; reported table and prose discrepancies remain explicit. |
| CHB-MIT and CBraMod | Cases 01 and 21 represent the same person. Their inclusion across training and validation establishes that overlap; it does not by itself establish test-person leakage. |
| Foundation models | TUAB abnormality and TUEV event-type tasks do not establish continuous seizure detection. Original LaBraM tasks and later seizure clip evaluations are distinguished. |
| Clinical workflow | Yang's large external inference evaluation and separate review pilot have different populations and endpoints. Rommens' retrospective algorithm comparison does not demonstrate an actual improvement in staff response. |
| Neonatal CNN validation | Hogan's reported binary sensitivity and false-event detections have different units. Summed channel-hours are not unique monitoring-hours. Nonsignificant agreement differences do not establish formal noninferiority. |
| Physiological interpretation | Learned edges, correlation, and attention do not establish anatomical connectivity or biological causation. Scalp visibility is distinguished from algorithm failure. |
| Regulatory status | A clearance is tied to a device, version, population, intended use, jurisdiction, and date. It is not evidence of improved patient outcomes. |

The corresponding chapters provide primary citations and the evidence table records each study's strengths and limitations.

## Access and reproduction limits

Some journal full texts and supplements were inaccessible. Some PMC and PubMed pages returned access challenges; indexed primary abstracts, official proceedings, and author-hosted publications were used when available. Those alternatives do not resolve uninspected methods or supplements.

Models were not independently trained or reproduced. Raw patient identities and pretraining provenance were not exhaustively audited across every dataset release. Detailed demographic fairness, the economic cost of false alarms, and global regulatory coverage remain incompletely documented. These limits constrain the conclusions and are retained as research questions.

## Updating the review

Updates should retain release versions, primary identifiers, endpoint units, and access status. When a correction changes a conclusion, the affected prose, table, and bibliography should be updated together. Later publications should carry their own search date and should not silently alter the historical cutoff of this edition.
