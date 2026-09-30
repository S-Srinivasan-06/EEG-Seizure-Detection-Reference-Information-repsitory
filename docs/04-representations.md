# How EEG representations shape detector design

Evidence date: 30 September 2026. This is a bounded search, not an exhaustive systematic review. See the [evidence policy](evidence-policy.md).

[Previous chapter](03-history.md) | [Chapter index](../README.md) | [Next chapter](05-datasets-and-evaluation.md)


A seizure detector's representation of electroencephalography (EEG) data answers what information it can use and what assumptions it makes. Categories can be combined in one system.

| Representation | Encoded information | Strengths | Failure modes / controls needed |
|---|---|---|---|
| **Raw waveform** | Voltage sequences from channels over time. | Preserves morphology and avoids fixed feature selection; supports convolution, attention, and self-supervised objectives. | Sensitive to montage, gain, filter, sampling rate, artifact, and channel order. Report preprocessing and test channel/montage robustness. |
| **Engineered temporal/spectral features** | Band power, line length, energy, entropy, wavelet coefficients, spectral ratios, amplitude/morphology summaries. | Compact, interpretable, can work with small cohorts and transparent baselines. | Feature thresholds depend on acquisition and artifact; segmentation can lose context. Compare against raw models at equal splits and tuning. |
| **Time-frequency images or tokens** | Spectrogram/scalogram patches, learned quantized spectral tokens, or channel-time patches. | Makes evolving rhythmic structure explicit and enables CNN/transformer encoders. | Resolution/windowing can blur onset; overlapping samples can leak across splits; ensure causal inference if alarms are claimed. |
| **Spatial montage / channel stack** | Electrode or bipolar derivation signals organized by channel identity, sometimes 2-D layout. | Learns topography and cross-channel propagation; clinically recognizable montage structures. | Channel identity and missing electrodes vary. Interpolation/zero-fill can create artifacts; montage reference changes signal. Test realistic dropout, re-reference, and unseen layouts. |
| **Graph** | Nodes are electrodes, contacts, or regions. Edges may encode physical distance, correlation, coherence/phase relation, a fixed montage prior, learned adjacency, or sample-specific adjacency. Message passing may use graph convolutional network (GCN)/Chebyshev supports, graph attention (GAT), spatiotemporal graph blocks, or graph transformers. | Handles irregular contact layouts and explicit spatial neighborhoods; separates local from long-range interactions and can represent channel relations separately from local waveform features. | These choices encode different engineering assumptions. Distance is not functional coupling; correlation/coherence can arise from volume conduction or reference choice; phase-derived relations depend on filtering and estimator. Dynamic learned edge weights do not establish neural communication or causal spread. Ablate fixed vs dynamic edges, distance/correlation/coherence/phase choices, and compare matched non-graph controls. |
| **Sequence and state representation** | Windows, recurrent states, temporal attention/convolution, or event-level transitions. | Captures evolution and persistence; supports onset/offset segmentation and alarm smoothing. | Noncausal context may use future samples; postprocessing can hide false alarms or add delay. Publish receptive field, buffer, state reset, and end-to-end latency. |
| **Cross-channel/token representations** | Channel tokens with shared encoding, set transformers, or variable-channel attention. | Can accommodate different channel counts and sensor configurations. | Flexibility can erase electrode semantics; absent channels can be confused with quiet channels. Include channel identity, missingness encoding, and montage-shift tests. |
| **Multimodal representation** | EEG plus ECG, motion, EMG, video, event markers, or reports. | Auxiliary signals may expose clinical correlate or reduce ambiguity; supports candidate review. | Sensor availability and artifacts differ; gains may be subgroup-specific; fusion risks missing events when a sensor fails. Report EEG-only and modality ablations, missing-modality behavior, and separate seizure types. |
| **Pretrained latent representation** | Embedding from large unlabeled/mixed EEG, masked prediction, contrastive context, or reports. | May reduce labeled-data needs and improve transfer. | Pretraining provenance/identity overlap may be unknown; abnormality or event-type tasks are not seizure detection; target exposure must be explicit. |

### What a fair architecture comparison requires

1. Same source cohort, label ontology, patient/site split, continuous exposure, operating-point selection, and postprocessing.
2. Equal tuning opportunity and clear baseline implementation for feature, CNN/temporal convolutional network (TCN), transformer, static graph, dynamic graph, and pretrained encoder.
3. Distinguish personalization (and labeled/unlabeled target exposure) from zero-shot transfer; report calibration time and performance before/after adaptation.
4. Lock causal or offline use. An offline detector may help review, but latency claims must include future context and buffering delay.
5. Report event sensitivity and precision with alarms/day, plus sample metrics, seizure-burden coverage, onset delay, duration error, and patient/site uncertainty. Avoid treating correlated windows as independent observations.

**Moderate confidence - representation novelty is a weaker clinical claim than demonstrated transportability.** The mathematical form of a distance edge, correlation graph, phase graph, learned adjacency, GAT, or graph transformer does not itself validate a neurophysiological interpretation. A dynamic graph, transformer, or foundation model merits attention when it improves a relevant endpoint under a fair patient/site-independent protocol. Its name or pretraining scale alone does not establish clinical utility.
