# 13. Research program and integrated successor

[Contents](../README.md) | [Previous: Part 12](12-open-questions.md) | [Next: Part 14](14-direct-answers.md)

### Key terms

- **Patient-disjoint split:** no patient contributes data to more than one of training, validation, or test sets.
- **Causal streaming:** processing signals in time order using only data available up to the alert time; no future samples or retrospective normalization are used.
- **Baseline:** a reference method used for comparison. A graph-free baseline uses channel signals without a graph representation.
- **Ablation:** removing or changing one model component while holding other conditions as constant as possible, to test that component's contribution.
- **Falsifier:** an observed result that would count against the stated hypothesis.
- **RTF (real-time factor):** processing time divided by the duration of signal processed; values below 1 indicate processing faster than the signal arrives.
- **CNN / RNN / TCN / GNN:** convolutional neural network / recurrent neural network / temporal convolutional network / graph neural network. **DGDCN** is the named model studied in this dossier.
- **EMU / ICU / iEEG / SEEG:** epilepsy monitoring unit / intensive care unit / intracranial EEG / stereoelectroencephalography. **TUSZ** is the Temple University Hospital Seizure Corpus.

Every experiment below is a proposed protocol. Any numerical design threshold or target introduced below is explicitly a **proposed design**, not a published or validated clinical standard. Set operating points with intended users before test access; use the same locked threshold on each held-out cohort unless a prospectively specified recalibration study is being evaluated.

### Minimum diversity and shared comparator requirements

The following are **proposed design commitments**, not empirically proven minimum sample sizes. For N2-N4 and the integrated successor, recruit at least four hospitals: two development hospitals, a third reserved for development-stage transport checks, and a fourth untouched external test hospital; require at least two acquisition vendors and naturally occurring montage differences. Include continuous artifact-heavy and seizure-free records, and analyze EMU and ICU populations separately. L1 uses at least four hospitals with site allocation fixed before enrollment; L2 uses at least three target-setting centers per separately powered program; L3 uses at least three surgical centers, with any ICU annotation arm separately recruited from at least three ICU services. N1 is deliberately a single-corpus reproduction; its transport claim is tested only through N2. Patient counts require clustered power calculations from pilot variability and clinically agreed margins, rather than window counts.

For N2 and the successor, require a handcrafted detector, strong CNN, causal TCN, transformer, static GNN, DGDCN/dynamic GNN, and pretrained encoder under equal tuning opportunities. Explicitly ablate distance, correlation, shuffled, fixed learned, and sample-conditioned graphs, matching parameters and temporal receptive fields. Compare pretraining versus training from scratch over matched labeled-patient budgets and publish exposure manifests. N3/N4 carry forward the strongest locked models from each family rather than selecting one family on test results. For every graph experiment, compare adjacency stability across seeds, seizures within people, background periods, rereferencing, artifacts, and channel removal; compare independent connectivity and clinician-reviewed propagation where available, retaining a predictive rather than causal interpretation. Use these requirements together with each study's specific protocol below.

### Near-term studies

#### N1. Reproduction and protocol repair for DGDCN (scalp TUSZ)

**Hypothesis**

A fully specified implementation of DGDCN will reproduce the reported clip-level signal and retain any advantage under patient-disjoint evaluation; if it does not, its architecture claim is not supported.

**Population/data**

Versioned TUSZ records with the paper's eligible subjects and labels, with a frozen patient-level manifest; exact cohort counts and exclusions to be published before training.

**Centers**

Public corpus provenance; no claim of multicenter validation.

**Training and splits**

Pin paper, repository commit, dependencies, channel order, reference, resampling, FFT windows/bins, normalization, label/window mapping, adjacency, threshold, checkpoint, and aggregation. Keep person-level train/validation/test disjoint, seal test labels, and audit pretraining/data provenance. Resolve reported code concerns (entrypoint syntax, graph/input dimension mismatch, evaluation loader use) without assuming they prove test tuning.

**Baselines/ablations**

Reproduction target, parameter-matched temporal CNN, graph-free channel mixer, fixed-topology graph, static learned graph; remove temporal attention, spatial attention, graph convolution, and dilated convolution one at a time.

**Endpoints**

Primary endpoint is patient-held-out reproduction of prespecified clip metrics, secondary to a later event study; publish confusion matrix, sensitivity, specificity, AUROC/AUPRC, patient-level distribution, and confidence intervals.

**Robustness/compute**

Channel reorder/mask, reference and preprocessing sensitivity; report parameter count, FLOPs, hardware, memory, inference time.

**Statistics and interpretation**

Multiple seeds and patient-cluster bootstrap; report deviations and uncertainty, no cross-paper ranking.

**Falsifier**

Reported advantage cannot be reproduced from documented choices or disappears versus matched baselines under patient-disjoint splits. **Sources:** [paper](https://doi.org/10.3390/bioengineering12080832), [repository](https://github.com/Open-EXG/DGDCN-EEG-Seizure-Detection).

#### N2. Continuous, patient-independent and external scalp benchmark

**Hypothesis**

A useful detector can preserve event sensitivity at a user-accepted alarm burden across new patients and at least one untouched external cohort; dynamic graphs improve on matched baselines only if they add independent event-level value.

**Population/data**

Long continuous scalp EEG with adjudicated onset/offset, seizure and non-seizure exposure, and age/setting labels; use datasets with sufficiently detailed annotations, keeping each dataset's case mix separate.

**Centers**

Train/development cohort plus at least one external institution/device cohort; minimum diversity follows the proposed four-hospital plan above; it is a study commitment rather than an established universal requirement.

**Training/splits**

Patient-disjoint training, validation and internal test; external test locked by cohort/site and never used for threshold or calibration selection. Audit pretraining and derived-window overlap.

**Baselines**

Strong graph-free temporal CNN/RNN, fixed sensor topology, static learned graph, DGDCN, and relevant established detector where license permits.

**Ablations**

Parameter/FLOP-matched graph removal, temporal context, channel identity/mask, and onset/offset smoothing; equal tuning budget.

**Metrics**

Primary: event sensitivity at a prespecified false alarms/24 h operating point (**proposed design:** determine acceptable value with target users, not by importing one universal number). Report event precision/F1, sample sensitivity/precision/F1, false alarms/24 h including negative hours, onset delay, duration overlap, per-patient and per-site values, and denominators under SzCORE scoring with deviations declared.

**Calibration/robustness**

Reliability plots, Brier score and calibration error; evaluate site/device/montage, missing channels, noise/artifact, seizure type/duration, and age strata without test retuning.

**Compute**

Streaming and offline RTF, latency distribution, memory, model size, named hardware.

**Statistics**

Patient-cluster bootstrap or hierarchical model, paired model comparisons, multiplicity plan, pilot-informed sample size and clinically meaningful margin.

**Interpretation**

Cohort transfer supports external validity only for those cohorts; no universal generalization claim.

**Falsifier**

Sensitivity/alarm trade-off misses the preregistered utility target, external performance materially degrades, or graph gain is within uncertainty. **Sources:** [SzCORE](https://doi.org/10.1111/epi.18113), [Peh](https://doi.org/10.1142/S0129065723500120), [Yang](https://doi.org/10.1016/j.eswa.2022.118083).

#### N3. Channel, montage, artifact, and subgroup robustness

**Hypothesis**

A channel-aware model with explicit missing-channel handling is less sensitive to acquisition shifts than a model trained on one fixed montage, without unacceptable loss of seizure-event coverage.

**Population/data**

Patient-disjoint continuous adult scalp recordings spanning multiple acquisition configurations; include clinician-reviewed signal-quality and artifact strata plus event morphology, age, and relevant demographic groups where consented metadata permit.

**Centers**

Development site(s) and independent sites/devices, with each held out in turn where numbers support it.

**Training/splits**

Lock a reference model; train channel-mask/montage augmentation only on development data; external site/device untouched.

**Baselines/ablations**

Fixed-montage model, explicit mask/identity model, reduced channel model, waveform versus spectral input, graph-free versus fixed/dynamic graph. Ablate augmentations individually and in prespecified combinations.

**Metrics**

Event sensitivity, false alarms/24 h, precision, delay, duration overlap and calibration by channel count, montage, artifact class, event type, patient, site, age and available demographic strata.

**Robustness**

Natural missing electrodes plus controlled masks, reference changes, channel permutation, noise/artifacts; distinguish synthetic stress test from real device evaluation. Test actual behind-ear recordings separately from scalp-derived two-channel subsets.

**Compute/statistics**

Report preprocessing and inference latency, memory and failure rate; clustered uncertainty and interaction tests only when adequately powered; multiplicity correction and subgroup denominators.

**Interpretation**

A subgroup disparity is a signal for investigation, not evidence of cause; absence of significance in sparse strata is not equivalence.

**Falsifier**

External montage/device or artifact strata show clinically material degradation or masked-channel handling creates excess alarms/misses. **Sources:** [SeizeIT1](https://doi.org/10.48804/P5Q0OJ), [SeizeIT2](https://doi.org/10.1038/s41597-025-05580-x), [Hartmann](https://doi.org/10.1111/epi.17259).

#### N4. Causal streaming, calibration, and operational burden

**Hypothesis**

A locked event detector can emit calibrated, timely alerts in causal streaming operation at a prespecified alarm burden, with operational performance close to its offline evaluation.

**Population/data**

Long continuous recordings from held-out patients with extensive negative exposure and independently annotated seizures; include real acquisition dropouts and signal-quality failures.

**Centers**

Internal patient-held-out plus external site/device cohorts from N2.

**Training/splits**

Freeze model, threshold, calibration transform, alarm merging/suppression, and quality rules before test; no future samples, retrospective normalization, or test adaptation.

**Baselines/ablations**

N2 strongest graph-free and graph model; compare causal model to offline upper-bound only as a diagnostic. Ablate calibration, smoothing/alarm logic, and signal-quality gate.

**Metrics**

Event/sample sensitivity and precision, false alarms/24 h, onset-to-alert delay, missed-event counts, alarm duration, calibration, and per-patient/site distributions.

**Latency/compute**

Record acquisition timestamp, buffer/window close, inference, transport, UI display and acknowledgment; report median and tail latency, throughput, RTF, peak memory, model size, energy if instrumented, and uptime on named CPU/edge/GPU.

**Robustness**

Packet loss, channel loss, clock drift, artifacts, long negative intervals, and recovery after restarts.

**Statistics**

Patient-cluster confidence intervals, repeated-run stability, predeclared comparisons and operating limits.

**Interpretation**

Faster inference alone is not clinical benefit; any alarm target is a **proposed design** chosen with users.

**Falsifier**

End-to-end latency, alarm burden, calibration, or failure rates breach the preregistered intended-use limits. **Sources:** [SzCORE](https://doi.org/10.1111/epi.18113), [DGDCN result limitations](https://doi.org/10.3390/bioengineering12080832).

### Longer-term studies

#### L1. Prospective multicenter silent-mode validation followed by assisted workflow trial

**Hypothesis**

A locked scalp detector that passes external silent-mode gates can reduce expert time to verified seizure recognition or review effort without unacceptable missed/delayed events, inappropriate responses, or site-specific failure.

**Population/data**

Consecutive eligible adult scalp EEG recordings from the prespecified intended service; exclude or separately analyze populations outside the intended use. Include prolonged negative monitoring and sufficient event diversity.

**Centers**

Diverse hospitals, devices, and referral pathways; designate site-held-out validation before enrollment.

**Training/splits**

Freeze model, threshold, and alarm policy; blinded independent multi-reader annotation/adjudication on a sample with an indeterminate category; separate development sites from final external sites. No adaptation during silent mode.

**Baselines**

Usual expert review and, if appropriate, currently used commercial workflow; do not infer baseline performance from a different cohort.

**Phases/metrics**

Phase 1 silent mode: event sensitivity, false alarms/24 h, precision, delay, calibration, subgroup/site results, uptime, failures and drift. Proceed only after safety gates agreed with clinicians. Phase 2 randomized individual, cluster, or stepped-wedge assisted review selected to limit contamination: primary workflow endpoint expert review time or time to verified event recognition; secondary event misses/delays, actions, alert overrides, treatment changes, adverse events, workload and cost. Direct time-motion measurement distinguishes real savings from data reduction.

**Ablations/interpretation**

Keep algorithm fixed; compare alert interface and alarm policy only if separately randomized/prespecified. A gain in recognition/time does not establish patient outcome benefit.

**Compute/privacy**

Log end-to-end reliability; protocolize data access, security, retention, and auditability.

**Statistics**

Power from pilot site/patient variance and a clinician-defined meaningful effect; account for clustering, multiplicity, missingness and sequential safety monitoring.

**Falsifier**

No meaningful workflow benefit, unacceptable safety trade-off, or reproducible site failure. Numeric pass thresholds and minimum effect are **proposed design** choices requiring stakeholder agreement. **Sources:** [Yang](https://doi.org/10.1016/j.eswa.2022.118083), [Rommens](https://doi.org/10.1016/j.yebeh.2018.04.026), [ANSeR](https://doi.org/10.1016/S2352-4642(20)30239-X).

#### L2. Separate setting-specific prospective translation programs

**Hypothesis**

The evaluation and alarm-design principles transfer across modalities, but a separately trained or adapted and prospectively validated model is needed for each target population and sensor setup.

**Populations/protocols**

(a) adult ICU prolonged scalp EEG with blinded multi-reader adjudication and explicit ictal-interictal/indeterminate labels; (b) neonatal EEG with seizure-hour, infant-level recognition, false detections, treatment decisions and safety as distinct outcomes; (c) behind-ear/wearable using actual intended sensors, adherence and usable recording hours; (d) iEEG/SEEG with modality-specific channel maps and annotations. Do not pool these cohorts into a single performance claim.

**Centers**

Multiple target hospitals and devices per program when feasible; independent site holdout.

**Training/splits**

Separate patient-, site-, and device-level splits; protocolized pretraining exposure; lock the external test.

**Baselines**

Setting-appropriate current clinical tools and graph-free signal models, plus the integrated successor only if relevant.

**Metrics**

Event and sample sensitivity/precision/F1, false alarms/24 h or clinically appropriate time unit, calibration, delay, event duration overlap, subgroup/site performance, signal-quality failures, workflow actions and harms; preserve setting-specific metrics and denominators. Neonatal ANSeR endpoints must not be collapsed into adult event metrics.

**Ablations/robustness**

Test modality-specific inputs, channel loss, artifact, prevalence, annotation uncertainty and clinical workflow interface.

**Compute/privacy**

Measure device-side latency, battery/energy, data transfer, uptime, security and governance as appropriate.

**Statistics/interpretation**

Power each protocol from its own event and cluster structure; adjudicate disagreement; distinguish signal visibility from model error where simultaneous scalp/SEEG permits.

**Falsifier**

Any target setting fails its prespecified safety/utility gate; success in one setting does not rescue another. Numeric gates are **proposed design**, set with users and regulators for each intended use. **Sources:** [ACNS terminology](https://doi.org/10.1097/WNP.0000000000000806), [ANSeR](https://doi.org/10.1016/S2352-4642(20)30239-X), [SeizeIT2](https://doi.org/10.1038/s41597-025-05580-x), [PPi](https://doi.org/10.52202/075280-3047), [Casale](https://doi.org/10.1097/WNP.0000000000000739).

#### L3. Reference uncertainty and scalp visibility substudy

**Hypothesis**

Separating label disagreement and scalp visibility from model discrimination will better characterize the errors attributable to the detector and the limits attributable to the recorded modality.

**Population/data**

A nested, consented multicenter cohort with simultaneous scalp EEG and clinically indicated SEEG where available, plus standard adult ICU scalp cases enriched for ambiguous patterns; no invasive procedure solely for model evaluation.

**Centers**

Surgical epilepsy and ICU sites analyzed separately.

**Training/splits**

The detector remains frozen and patient/site-disjoint; independent blinded readers annotate EEG with confidence and indeterminate options; consensus adjudication occurs without model output.

**Baselines/ablations**

Compare model to each reader, consensus, and a graph-free baseline; ablate model explanation interface only in a reader study.

**Metrics**

Against scalp annotations: event sensitivity, false alarms/24 h, delay and calibration. In simultaneous subset: separately report detection among scalp-visible SEEG events and all SEEG-confirmed events; reader agreement, disagreement patterns, and confidence intervals. Do not treat SEEG sampling as whole-brain truth.

**Robustness/compute**

Stratify artifact and montage; report causal latency and resource measurements if run online.

**Statistics**

Patient-clustered intervals; prespecify event and visibility strata; no claims from underpowered rare strata.

**Interpretation**

Disagreement is itself a result; a scalp-negative intracranial event is not automatically a model false negative for a scalp-visible task.

**Falsifier**

Adjudication/visibility separation does not improve error interpretation or reveals that claimed performance depends on uncertain labels. **Sources:** [Casale](https://doi.org/10.1097/WNP.0000000000000739), [Halford](https://doi.org/10.1016/j.clinph.2014.11.008), [ACNS](https://doi.org/10.1097/WNP.0000000000000806).

### Analysis and reporting rules for every study

- Register the target population, label ontology, primary endpoint, threshold procedure, exclusions, subgroup analyses, stopping rules, and multiplicity plan before test access.
- Keep patients disjoint at every stage; for external claims hold out whole sites/devices or cohorts. Audit overlapping windows, fitted normalization, prior pretraining exposure, and data-release version.
- Use event- and sample-based scoring, publish event counts and negative hours, false alarms per 24 hours, onset delay, overlap tolerance, calibration, and patient-level distributions. Follow SzCORE or declare deviations. [SzCORE](https://doi.org/10.1111/epi.18113)
- Use patient-cluster bootstrap or appropriate hierarchical inference; determine sample size from pilot variance and a prespecified meaningful margin. Do not infer equivalence from a nonsignificant subgroup result.
- Release code, configs, split manifests where permitted, preprocessing, threshold, model artifact, and hardware details. State whether results are reproduced or reimplemented; mark uninspected fields as not found in inspected material.
- Keep data reduction, annotation labor, workflow time, clinical action, and patient outcomes as separate endpoints. Yang's published abstract reports external inference on 1,006 sessions [V] and a separate 66-session review pilot [V]; do not turn this into a general workload claim. [Yang](https://doi.org/10.1016/j.eswa.2022.118083)

### Integrated successor: DGDCN-S (provisional research system)

Build a scalp-streaming successor whose objective is better patient-held-out and external event utility at a declared false-alarm burden. Architectural novelty is not the objective. Use a causal waveform or spectral front end with fixed, documented windows and normalization; an optional dilated temporal block; explicit channel identities and missing-channel masks; and a graph branch that is either fixed sensor topology or input-conditioned and regularized, alongside a graph-free residual path. Emit calibrated event scores into explicit onset/offset smoothing, signal-quality checks, and alarm-state logic. Compare each branch against parameter-matched graph-free baselines on N1-N4 splits; remove any component without independent external improvement and acceptable calibration/robustness. Model adjacency is a predictive representation, not a physiological network claim. Keep inference causal and measure the full system on named hardware. Train, validation, internal test, and external site cohorts remain disjoint. Develop ICU, neonatal, wearable, and iEEG variants separately rather than calling one model universal. Numeric acceptance criteria are **proposed design** and require intended-user agreement before evaluation.

---

[Contents](../README.md) | [Previous: Part 12](12-open-questions.md) | [Next: Part 14](14-direct-answers.md)
