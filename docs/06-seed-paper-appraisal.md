# 6. Critical appraisal of the seed study

[Contents](../README.md) | [Chapter 5: Datasets and evaluation](05-datasets-and-evaluation.md) | [Next: Methods and pretraining](07-methods-and-pretraining.md)

## Study question and scope

Zhang, Dai, and Guo’s “Dynamic Graph Convolutional Network with Dilated Convolution for Epilepsy Seizure Detection” (*Bioengineering* 12(8), 832, 2025; [DOI](https://doi.org/10.3390/bioengineering12080832)) is the seed study. The full 13-page paper, including its methods, equations, tables, limitations, and references, was inspected. It asks whether a model can classify labeled TUSZ EEG segments as seizure-containing or non-seizure. It does not test continuous alarm detection, seizure-onset localization, or clinical response.

## Data, inputs, and protocol

| Field | Study details |
|---|---|
| Source and split | TUSZ v2.0.1. The authors describe patient-disjoint training, development, and evaluation cohorts. Exact subject counts and the mapping between tables and splits were not found in the inspected report. The paper’s dataset version should not be inferred from a later TUSZ release. |
| Signal | 22-channel scalp EEG resampled to 200 Hz. The stated model input is 22 channels × 100 spectral features × T, where T is 12 or 60. |
| Task and labels | Nonoverlapping 12-second or 60-second segments. A segment is ictal if it contains at least one second of annotated seizure. This is a positive-window rule; it does not redefine the minimum clinical event duration. |
| Features | FFT log-amplitude features, z-normalized with the training-set mean and standard deviation. The precise mapping from each segment to 100 features and T time positions is underspecified, which limits exact reproduction. |
| Model | Temporal attention, spatial attention, Chebyshev graph convolution with K=3, temporal dilated convolution with dilation 2, then a fully connected classifier. The spatial attention matrix is dense, unthresholded, and not constrained to be symmetric. |
| Training | Adam, initial learning rate 1e-4, batch size 20, 64 graph filters and 64 temporal filters, cosine learning-rate schedule, and early stopping after five consecutive validation-loss non-improvements. Augmentation uses amplitude scaling of 0.8-1.2 and scalp-midline reflection. |
| Metrics | Segment-level accuracy, sensitivity, specificity, and ROC AUC. Threshold, averaging unit, checkpoint selection, repeated-run procedure, confidence intervals, and event-matching rules were not found. False alarms per hour/day, onset delay, seizure-burden coverage, and continuous-stream performance were not reported. |

The authors’ [repository](https://github.com/Open-EXG/DGDCN-EEG-Seizure-Detection) is available. Static inspection found an apparent syntax error in the training entrypoint; a 19-vertex model setup despite the paper’s 22 channels; evaluation-loader use during validation cycles; and a final lookup for a `test` loader although the loader creates `train`, `dev`, and `eval`. Code inspection did not establish patient membership in marker files. The code was not run and results were not reproduced. These are reproducibility concerns, not proof that the published findings are invalid.

## Reported results and inconsistencies

Table 1 reports DGDCN AUC of 88.7% for 12-second segments and 90.4% for 60-second segments. For 12-second segments, the table gives accuracy/sensitivity/specificity/AUC of 79.7/84.8/76.6/88.7%; the prose instead gives specificity as 78.6%. Both values remain unresolved. With the table value of 76.6%, DCRNN’s 77.5% specificity is higher, which conflicts with the prose claim that DGDCN leads on every 12-second criterion. The alternative prose value reverses that comparison. For 60-second DGDCN segments, the table gives accuracy/sensitivity/specificity/AUC of 73.5/94.3/68.7/90.4% [V].

Other reporting conflicts concern EEGNet and ablation arithmetic. EEGNet’s reported 60-second accuracy of 52.8% exceeds both its sensitivity (35.3%) and specificity (45.3%), which cannot result from one pooled confusion matrix. The paper calls these “average performance” but does not explain the aggregation, so the conflict is unresolved. From the tables, the 12-second temporal-attention sensitivity gap is 7.6 percentage points (84.8 minus 77.2), not 1.7; the graph-convolution AUC gap is 0.3 points, not 3; and the dilated-convolution sensitivity gap is 0.9 points, not 0.7. Other checked ablation comparisons match the tables. Augmentation raises 12-second sensitivity by 3.5 points while AUC falls by 0.6 points, so it does not improve every metric. These values and arithmetic were checked against Tables 1-3 and the discussion [V].

## What “dynamic graph” means in the implementation

The paper describes its graph operation as a Chebyshev polynomial support modulated by sample-specific spatial attention. The [public model source](https://github.com/Open-EXG/DGDCN-EEG-Seizure-Detection/blob/main/model/DGDCN/model/DGDCN_r.py) shows that the scaled Laplacian and Chebyshev basis are constructed once from a supplied adjacency. Each forward pass applies sample-specific attention elementwise to those fixed supports. The implementation therefore modulates fixed Chebyshev graph supports; it does not estimate a new Laplacian or physiological connectivity graph for each example [V].

Temporal attention uses an unmasked, full time-by-time score matrix, and the selected temporal convolution uses centered padding. The posted model is therefore noncausal across its input segment. Online use would require a separately designed trailing-buffer implementation and measured delay [V].

## Components and their interaction

The method combines operations that answer different engineering questions. The table below explains their roles; it does not assign the overall result to any one component.

| Component | Role in the model | Limit of interpretation |
|---|---|---|
| Temporal attention | Weights relationships among time positions within the input clip | Access to the full clip is not evidence of timely causal streaming |
| Spatial attention | Produces sample-specific weights between channels | A changing weight is not independently measured physiological connectivity |
| Graph convolution | Combines channel features through fixed Chebyshev supports modulated by spatial attention | Graph mixing can contribute capacity as well as a spatial assumption |
| Dilated temporal convolution | Combines temporal features over a wider receptive field using spaced kernel positions | More temporal context can affect both discrimination and delay |
| Combined architecture | Uses temporal weighting, spatial weighting, graph feature mixing, and temporal convolution within one classifier | Removing a block changes several properties; unmatched ablations cannot establish a unique causal explanation for the gain |

The implementation distinction matters for the proposed follow-up: compare fixed supports with and without sample-specific attention, and match the temporal receptive field and model capacity. A better result from the combined network would otherwise leave the contribution of graph representation unresolved.

## Interpretation

The study demonstrates a reported comparison of clip-level seizure/non-seizure classification on TUSZ v2.0.1 under the authors’ stated split, an attention-plus-graph-plus-dilated-convolution architecture, and ablations whose tabulated differences can be stated with the corrections above. Its results suggest these modules may be useful for this segment-classification setup, but do not establish general advantages.

The study does not demonstrate event sensitivity, false alarms per hour/day, useful onset delay, alarm workload, calibration at deployment prevalence, patient-independent performance confirmed from subject manifests, external-site or device generalization, real-time causality, clinical benefit, or superiority over event-based clinical systems. “Patient-disjoint” is the authors’ description; the split was not independently reconstructed. The authors also identify latency, computational efficiency, continuous-stream evaluation, and realistic clinical validation as unassessed.

Treat DGDCN as a promising but internally inconsistent segment-classification study, not a clinical-detector leaderboard result. A replication should publish immutable patient and file manifests, exact preprocessing and feature axes, aggregation and threshold rules, metrics for every split, and a causal event postprocessor. It should compare fixed-graph, attention-only, and dynamic-support variants under matched tuning budgets.

**Confidence:** High for the task/metric distinction and table/prose inconsistencies, based on inspection of the full paper and model source. Moderate for code-related reproducibility concerns because those observations were static and no execution or reproduction was performed.
