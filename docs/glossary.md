# Glossary

[Repository overview](../README.md) | [Datasets and evaluation](05-datasets-and-evaluation.md)

This glossary defines terms as they are used in the dossier. Clinical event definitions depend on the population and annotation protocol. Adult critical-care terminology should not be substituted for neonatal definitions or a dataset's original labels.

## Clinical terms and recordings

| Term | Meaning |
|---|---|
| EEG | Electroencephalography: recording electrical voltage differences using electrodes. It measures activity indirectly and depends on electrode placement and reference. |
| Scalp EEG | EEG recorded using electrodes on the head's surface. |
| Intracranial EEG or iEEG | EEG recorded using electrodes placed inside the skull for a clinical indication. |
| SEEG | Stereo-electroencephalography: intracranial recording using depth electrodes that sample selected brain regions. |
| ECoG | Electrocorticography: recording from electrodes placed on the brain's surface. |
| Subscalp EEG | Recording from electrodes implanted beneath the scalp; it is not intracranial recording from brain tissue. |
| EMU | Epilepsy monitoring unit, where patients undergo supervised recording to characterize seizures and inform care. |
| ICU | Intensive care unit. Its EEG populations and abnormal patterns differ from those in an EMU. |
| cEEG | Continuous EEG, generally used to refer to prolonged monitoring rather than a brief routine examination. |
| Neonatal EEG | EEG in newborn infants, with age-specific background activity, seizure patterns, and clinical interpretation. |
| Electrographic seizure | A seizure defined by EEG features under a specified clinical or research annotation policy. |
| Electroclinical seizure | A seizure with EEG activity and a qualifying clinical correlate under the relevant terminology. |
| Ictal / interictal | During a seizure / between seizures. |
| Ictal-interictal continuum or IIC | Critical-care EEG patterns that require contextual interpretation and are not automatically equivalent to definite seizures. |
| BIRDs | Brief potentially ictal rhythmic discharges, a distinct category in critical-care EEG terminology. |
| Focal seizure | A seizure associated with activity in a limited network rather than generalized onset; clinical features vary. |
| Seizure burden | The amount of time occupied by seizures within a defined monitoring interval. |
| Artifact | Recorded activity arising from sources such as movement, muscle, electrodes, or electrical interference that can obscure or resemble brain activity. |
| Montage | The arrangement of displayed or analyzed voltage differences between electrodes. |
| Reference | The electrode or combination of electrodes against which a signal is measured. Changing the reference can change apparent channel relationships. |
| Bipolar derivation | The voltage difference between two recording electrodes. It is a signal channel, not a direct measurement of one anatomical region. |
| Channel | One recorded or derived voltage time series. A channel may combine more than one electrode. |
| Sampling rate | How frequently a signal is measured. Resampling changes this rate and requires appropriate signal processing. |
| Annotation | A label assigned to a recording, interval, event, or channel by a reader or defined procedure. |
| Adjudication | Resolution of annotation disagreements using an explicit review procedure. |
| Signal visibility | Whether an event has identifiable information in the recorded modality. Lack of a visible scalp pattern and failure to detect a visible event are different limitations. |

## Signal processing and model families

| Term | Meaning |
|---|---|
| Waveform | Voltage values plotted or represented over time. |
| Spectral feature | A description of a signal's frequency content, such as power within a frequency band. |
| FFT | Fast Fourier transform, a computational method for estimating a signal's frequency representation. |
| Spectrogram | A representation showing how frequency content changes over time. Window length affects its time and frequency resolution. |
| Wavelet / scalogram | A way to represent signal structure at different time scales; a scalogram displays wavelet coefficients. |
| Entropy feature | A numerical summary of variability or irregularity defined by a particular formula. It does not directly measure seizure causation. |
| CNN | Convolutional neural network, which learns local filters over a signal or its representation. Multichannel CNNs can learn spatial mixing. |
| RNN | Recurrent neural network, which maintains a state while processing a sequence. |
| LSTM / GRU | Long short-term memory / gated recurrent unit: recurrent architectures designed to control information retention. Their use does not establish an advantage for long EEG context without comparison. |
| TCN | Temporal convolutional network, which models sequences using convolutions over time. |
| Dilated convolution | A convolution with spaced input positions, expanding the range of context available to a layer. |
| Receptive field | The range of input samples that can influence a model output. |
| Attention | A learned operation that weights information from positions, channels, or tokens. Attention weights are not automatically a causal explanation. |
| Transformer | A neural architecture that uses attention to combine information across a sequence or set of tokens. |
| GNN | Graph neural network, which combines information from nodes according to defined or learned relationships. |
| GCN / GAT | Graph convolutional network / graph attention network: families of graph models using convolution-like aggregation or attention-based weighting. |
| Node / edge | An entity and a relationship in a graph. In EEG, a node may be a channel, electrode, contact, or region; these choices are not interchangeable. |
| Adjacency matrix | A matrix specifying the connections or weights between graph nodes. |
| Static / dynamic graph | Relationships held fixed during an evaluation / relationships changing with input or time. Authors use these terms differently, so the implemented operation must be checked. |
| Chebyshev graph support | A polynomial representation of a graph operator used in graph convolution. In the inspected DGDCN implementation, attention modulates fixed supports. |
| Functional connectivity | A statistical relationship between recorded signals. Correlation, coherence, or phase-based measures can be used; none alone establishes causal communication. |
| Volume conduction | Spread of electrical activity through conducting tissue. It can create similar scalp signals without demonstrating direct interaction between the recorded locations. |
| Self-supervised learning or SSL | Training from structure in unlabeled data, for example predicting masked signal portions or contrasting related examples. |
| Weak supervision | Training using imperfect labels from rules, reports, or other indirect sources rather than fully adjudicated labels. |
| Pretraining / fine-tuning | Learning representations on an initial corpus / subsequently adapting the model to a target task. |
| Foundation model | A model pretrained to support multiple downstream tasks. The term does not establish performance on continuous seizure detection. |
| Domain adaptation | Adjusting a model to a target data distribution. Use of target data must be stated when assessing independence. |
| Personalization | Adapting a model to an individual, potentially using that person's previous seizures or background EEG. |
| Causal streaming | Processing that uses only information available at the time an output is produced. Here, "causal" concerns time access, not biological causation. |

## Evaluation and evidence

| Term | Meaning |
|---|---|
| Sample | A single signal value or evaluation time point, depending on the study. The term must be defined. |
| Window / clip | A short interval used as one input example. A positive clip may contain only part of a seizure. |
| Event | A reference seizure or detected alarm episode under specified onset, offset, and matching rules. |
| Record | A recording or recording file. Record-level sensitivity concerns whether seizure-containing records are identified. |
| Sensitivity / recall | The fraction of reference positives identified. A sample-level fraction and a seizure-event fraction answer different questions. |
| Specificity | The fraction of reference negatives classified as negative under the stated evaluation unit. |
| Precision / positive predictive value | The fraction of detected positives judged correct. It depends on the prevalence and evaluation unit. |
| Accuracy | The fraction of examples correctly classified. It can be dominated by common nonseizure examples. |
| Balanced accuracy | An average of class-specific recall measures; it addresses class imbalance differently from ordinary accuracy. |
| AUROC / AUC | Area under the receiver operating characteristic curve, summarizing discrimination across score thresholds. It does not specify a clinically acceptable alarm threshold. |
| AUPRC / AUCPR | Area under the precision-recall curve. It is sensitive to the proportion of positive examples. |
| F1 score | A combination of precision and recall. The definition of positives and event matching still matters. |
| FA/24 h or FA/day | False alarm episodes divided by the stated monitored exposure and expressed per day. Alarm merging and the time denominator must be reported. |
| Latency | Delay between defined times, such as seizure onset and alert. Model computation time alone excludes buffering, filtering, and notification delay. |
| Calibration | Agreement between predicted probabilities and observed frequencies. Good discrimination does not imply good calibration. |
| Operating point | The selected threshold and associated sensitivity, false-alarm burden, and other measures. |
| Patient-independent testing | Testing on people separated from model training. Validation patients must also be separated when making that claim. |
| External validation | Testing using a distinct source, such as another hospital or dataset, with target exposure and tuning clearly identified. |
| Leakage | Information from evaluation data or correlated identities influencing training or selection in a way that invalidates the intended generalization claim. |
| Patient-macro result | A summary giving patients equal weight rather than allowing people with more windows or seizures to dominate. |
| Confidence interval or CI | An interval expressing uncertainty under a statistical method and its assumptions. |
| Standard deviation or SD | A measure of observed variability. Variation across training seeds is not a patient-level confidence interval. |
| Noninferiority | A prespecified test that a difference is no worse than an agreed margin. A nonsignificant difference alone does not establish it. |
| Retrospective / prospective | Analysis of existing data / evaluation organized ahead of data collection or clinical use. Prospective collection alone does not establish live alarm utility. |
| Silent mode | A system running without its outputs directing clinical care, allowing operational performance to be evaluated. |
| Ablation | Removing or changing a model component to study its contribution. Changes in capacity or tuning can complicate interpretation. |
| Falsification criterion | A result specified in advance that would count against the study's hypothesis or proposed system. |
| SzCORE | Seizure Community Open-Source Research Evaluation, a framework for harmonizing EEG seizure detection data, validation, and scoring. It is not a clinical device clearance. |
| TUSZ / TUH / TUEG | The Temple University Hospital seizure corpus / hospital EEG resource family / broader EEG corpus. Release versions and subsets must be identified. |
| TUAB / TUEV | TUH abnormal EEG / EEG event-type datasets. Their classification tasks should not be described as continuous seizure detection. |
| CHB-MIT | A pediatric scalp EEG resource. Some recording cases refer to the same person, so case identifiers should not be assumed to be unique patient identifiers. |
| DOI / PMID / PMCID | Persistent identifiers for publications, PubMed records, and PubMed Central full texts. |

For formal critical-care event definitions, consult [ACNS terminology](https://doi.org/10.1097/WNP.0000000000000806). For dataset and metric conventions, consult [SzCORE](https://doi.org/10.1111/epi.18113) and [Chapter 5](05-datasets-and-evaluation.md).
