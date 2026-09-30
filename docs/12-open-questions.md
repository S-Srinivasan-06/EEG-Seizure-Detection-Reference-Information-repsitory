# 12. Open questions

[Contents](../README.md) | [Previous: Part 11](11-research-gaps.md) | [Next: Part 13](13-research-program.md)

### Key terms

- **Natural prevalence:** the proportion of monitored time or recordings that contain seizures in the intended clinical setting, rather than in a seizure-enriched sample.
- **Adjudication:** a documented process for resolving differences among expert readers, while retaining uncertain or indeterminate cases when needed.
- **Scalp visibility:** whether an intracranial seizure produces a detectable pattern in scalp EEG. A scalp-invisible event is not automatically a detector error for a scalp EEG task.

### Understudied questions

- Which patient and event strata account for misses and excess alarms, especially rare morphologies, short seizures, age groups, and medication/state changes?
- How well do models transfer to actual new sites, amplifiers, reference schemes, montages, missing electrodes, and true behind-ear/subscalp signals?
- What is the calibration and clinical utility at natural prevalence over prolonged non-seizure exposure, and how often does the operating point drift by site or patient?
- How much review time, recognition delay, treatment change, harm, or cost changes when alarms enter a real workflow? What is the user and action for each intended use?
- Can external validation use consistent continuous annotations, blinded adjudication, and governed access while keeping patients/sites disjoint?
- Which explanations help experts find or reject events, and which merely reflect model shortcuts?
- What are the full-system compute, power, network, uptime, cybersecurity, and privacy requirements on intended hardware?

### Conflicting scientific claims to resolve

- **Generalization:** patient-independent clip tests exist, but performance is task- and dataset-dependent; one Siena study found a marked segment-to-subject drop. They do not conflict with external event studies because they measure different endpoints and cohorts. [Brookshire](https://doi.org/10.3389/fnins.2024.1373515), [Peh](https://doi.org/10.1142/S0129065723500120)
- **External validity:** cross-dataset event testing and Yang's large frozen external scalp inference refute a blanket claim that external evaluation is absent. Their retrospective design, setting coverage, false-alarm burden, and lack of broad prospective outcome evaluation still leave deployment validity open. [Peh](https://doi.org/10.1142/S0129065723500120), [Yang](https://doi.org/10.1016/j.eswa.2022.118083)
- **Clinical benefit:** prospective EMU software evaluation and ANSeR's randomized neonatal support trial exist. ANSeR improved seizure-hour recognition, not individual-neonate sensitivity or demonstrated long-term outcomes; Yang's workload reduction belongs to a separate 66-session [V] review pilot. These are bounded benefits, not a contradiction of uncertainty about broad adult outcomes. [Fürbass](https://doi.org/10.1016/j.clinph.2014.09.023), [Pavel](https://doi.org/10.1016/S2352-4642(20)30239-X), [Yang](https://doi.org/10.1016/j.eswa.2022.118083)
- **Scalp versus intracranial events:** low scalp visibility for some SEEG seizures in a selected surgical cohort is a signal-visibility limitation, not simply model error. It does not establish a universal ceiling or show that models can detect all scalp-negative events. [Casale](https://doi.org/10.1097/WNP.0000000000000739)
- **Architecture:** dynamic graphs, transformers, and foundation models offer plausible representations, but comparisons across different splits, labels, and metrics do not establish a winner. DGDCN's reported clip performance is not an event detector comparison. [DGDCN](https://doi.org/10.3390/bioengineering12080832)
- **Challenge performance:** use the revised, non-peer-reviewed SzCORE v2 result (best event F1 32%, sensitivity 37%, precision 29% [V]) and do not mix it with superseded v1 figures; the cause of the version change remains unverified. [SzCORE v2](https://arxiv.org/abs/2505.18191v2)

### Deployment unknowns

Before threshold selection, a clinical team must specify the population, user, alert action, allowable false-alarm burden, missed-event consequences, and escalation path. The relevant latency clock begins at signal acquisition and ends at a visible actionable alert; model execution time alone is insufficient. Workflow improvement, patient benefit, regulatory status, and cost-effectiveness each need their own evidence and jurisdiction-specific analysis.

---

[Contents](../README.md) | [Previous: Part 11](11-research-gaps.md) | [Next: Part 13](13-research-program.md)
