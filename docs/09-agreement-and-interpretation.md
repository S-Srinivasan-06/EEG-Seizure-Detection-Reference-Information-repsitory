# 9. Agreement, conflict, and interpretation across studies

[Contents](../README.md) | [Chapter 8: Clinical validation](08-clinical-validation.md) | [Next: State of the art](10-state-of-the-art.md)

## Findings that converge

**The evaluation unit determines what a score means.** Clip AUC, event sensitivity, record sensitivity, seizure-hour recognition, patient-level recognition, and review time answer different questions. Clip tasks such as CBraMod and LaBraM cannot be ranked against EMU event alarms. Ganguly’s expanded-cohort record PPV is not event precision, and ANSeR’s seizure-hour gain is not improved individual-neonate sensitivity.

**Alarm burden is part of performance.** Commercial EMU and wearable studies show sensitivity/precision trade-offs. A system that identifies more candidates can create more review work. Expert filtering may improve precision while reducing sensitivity.

**Patient, site, and time separation matter.** Random-window splits can inflate apparent generalization. Patient separation is needed for a population-generalization claim; external hospital and device tests add a stronger transport test. CBraMod’s CHB cases 01 and 21 create train/validation identity overlap, but this does not establish test leakage. DGDCN’s authors describe a patient-disjoint split, but the subject manifest was not independently verified.

**A detector is one link in a care pathway.** Acquisition, alert generation, expert confirmation, staff response, treatment, and outcome need separate timestamps and endpoints. Rommens supports earlier retrospective algorithm signals; Yang reports a small retrospective review pilot; ANSeR reports a randomized intermediate recognition outcome. None establishes broad adult outcome or staffing-cost benefits.

**Reference labels are imperfect.** In one small ICU EEG excerpt study, mean pairwise expert seizure agreement was κ=.58 [V] ([Halford et al.](https://doi.org/10.1016/j.clinph.2014.11.008)). Ambiguous periodic or rhythmic patterns, annotation policy, and signal visibility affect apparent false-positive and false-negative counts.

## Conflicts and unresolved evidence

Several conflicts remain in the DGDCN primary report: its table and prose disagree on specificity; an EEGNet 60-second accuracy/sensitivity/specificity combination cannot come from one pooled confusion matrix; and several ablation arithmetic statements disagree with the tables. For CBraMod, validation identity overlap is established, but possible overlap of target cases with its large pretraining corpus is not. REVE’s abstract mentions seizure detection, but the enumerated task suite does not identify a seizure-specific protocol, so its seizure endpoint is unverified. Yang’s journal abstract reports external and pilot headline results; the pilot must not be substituted for the full external cohort, and the false-alarm interval notation remains unclear. Preserve each narrow verified observation and mark the missing evidence rather than infer a favorable interpretation.

The claim that “there is no prospective or external evidence” is too broad. Fürbass reports prospective multicenter EMU performance; Yang reports external inference; ANSeR is randomized neonatal assistance; and Peh reports multi-dataset external event evaluation. The narrower conclusion supported here is that independent prospective evidence for routine adult staffing reduction, improved adult patient outcomes, and broadly transportable alarm performance remains limited.

## Architecture and protocol comparisons

Most apparent disagreements reflect different tasks or protocols, rather than matched evidence that one model family is better. “No direct comparison identified” means no matched comparison in this reviewed set, not that no such comparison exists.

| Comparison | Evidence and interpretation |
|---|---|
| CNN vs RNN/ConvLSTM | CNNs, recurrent hybrids, and ConvLSTMs appear on different cohorts and endpoints. Yang’s ConvLSTM external inference and DGDCN’s CNN-like temporal convolutions were not trained and tested on matched data. The comparison is confounded; no family winner is established. Differences could arise from cohort, labels, preprocessing, postprocessing, or split. |
| Graph vs non-graph | DGDCN and other graph baselines report clip results on their own protocols; CNN/TCN systems often report event metrics on different datasets. No causal graph advantage is established. A comparison needs the same split, feature budget, tuning, and event pipeline. |
| Fixed vs dynamic graph | DGDCN’s implementation modulates a fixed Chebyshev basis with sample-specific attention. It does not compare a newly recomputed per-sample Laplacian against a fixed-support model. The benefit of a “dynamic” graph is not isolated. This reading is based on static inspection, not reproduction. |
| Raw waveform vs time-frequency features | DGDCN uses FFT log-amplitudes; several end-to-end CNN methods use filtered waveform. Their cohorts, window definitions, and labels differ, so no general representation winner is established. Compare both on identical continuous records and with matched causal latency. |
| Window length and context | DGDCN reports 12- and 60-second clips; CBraMod uses 10-second clips; Hogan’s neonatal inputs are 16 seconds with smoothing; event systems use different windows and postprocessing. This is a task-dependent trade-off. Longer context can change evidence and latency; matched stream evaluation is needed to identify benefit. |
| Personalized vs universal | Patient-specific iEEG systems such as Laelaps use prior seizures from the same person. Patient-independent and cross-site models target transfer. These are different deployment objectives. Report adaptation data, calibration time, and the pre-adaptation score. No universal best strategy is established. |
| Transformer vs CNN | CBraMod, BIOT, and iEEG transformers use different cohorts and often clip tasks; CNN baselines also vary. CBraMod’s later seizure appendix includes a fine-tuned LaBraM clip baseline, a task-specific comparison. There is no general architecture winner. LaBraM’s original reported tasks remain TUAB abnormality and TUEV event type, not continuous seizure detection. |
| Pretrained vs task-specific | CBraMod compares pretrained representations on CHB-MIT clips; PPi pretrains specifically for SEEG seizure detection; generic LaBraM and REVE task lists differ. Data scale, modality, target exposure, fine-tuning, and evaluation unit vary, so these do not form one controlled comparison. No universal causal pretraining effect is established. |

Confidence is high that the audited studies do not provide matched comparisons for most rows, and moderate for broader explanations of cross-paper score differences as confounding. Endpoint and split distinctions are directly verified; the confounding interpretation is methodological inference where matched experiments are absent.

## Interpretability: inspection is not physiological proof

Attention matrices and learned graph edges can help diagnose model behavior, but they are not self-validating explanations. DGDCN’s spatial matrix is dense, asymmetric, and unthresholded, and its implementation modulates fixed graph supports. Neither attention weights nor the Chebyshev operator establish functional or anatomical connectivity. A scalp topography can reflect reference choice, montage, artifacts, or correlated channels. A high-frequency or regional saliency map does not prove a seizure mechanism.

Interpretability claims should include attribution stability across seeds and nearby windows; channel/time occlusion or counterfactual perturbation comparisons; artifact and montage stress tests; comparison with independent expert localization when appropriate; and evidence that an explanation predicts a measurable failure mode. State whether the explanation is post-hoc or part of the model, and do not equate attention with causal relevance. For clinical use, the practical question is whether a reviewer can verify a flagged event and understand its uncertainty.
