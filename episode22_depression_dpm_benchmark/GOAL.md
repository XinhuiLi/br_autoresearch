# Benchmarking disease progression models in depression

This draft episode asks whether disease progression modelling (DPM) and
latent-continuum methods developed largely outside depression produce stable,
clinically useful subtype, stage, and trajectory structure in four private
depression datasets:

1. Chinese MDD, a planned major depressive disorder cohort.
2. UCLA-UNM ECT, a planned electroconvulsive-therapy depression cohort.
3. LA5C, a planned depression cohort.
4. MDD DIRECT, a planned depression cohort.

The scientific question is:

> Which disease-progression modelling assumptions are supported by private
> depression neuroimaging and clinical data, and do inferred subtype/stage
> or latent-continuum coordinates generalize across Chinese MDD, UCLA-UNM ECT,
> LA5C, and MDD DIRECT strongly enough to predict symptom burden or treatment
> trajectory?

This is a methods-benchmark and boundary-test episode, not a claim that
depression has a single irreversible neurodegenerative course. The central
comparison is between models that infer event sequences or subtype-stage
structure from cross-sectional biomarker abnormality, models that exploit
longitudinal observations or treatment response, and a VAE-based functional
connectivity interpolation model that treats patient-control variation as a
latent continuum.

## Why this matters

DPM is designed to combine clinical knowledge with data-driven progression
patterns when observations are heterogeneous and longitudinal follow-up is
limited. The DPM community site emphasizes this balance, and highlights
event-based models, Disease Course Mapping, Gaussian Process Progression
Models, and SuStaIn as major families:
<https://disease-progression-modelling.github.io/pages/main.html>.

Depression is a demanding test case for those methods. Major depressive
disorder is heterogeneous, fluctuating, recurrent, and treatment-sensitive;
therefore a high-performing DPM must separate stable patient structure from
scanner/site effects, severity, treatment state, and reversible change. A
benchmark across Chinese MDD, UCLA-UNM ECT, LA5C, and MDD DIRECT can show
whether DPM-derived or VAE-derived coordinates behave as reusable disease-state
descriptions, treatment-state markers, or dataset-specific summaries.

Recent work already applies SuStaIn to large MDD imaging cohorts. For example,
a REST-meta-MDD analysis used cross-sectional ALFF from 833 MDD patients and
834 healthy controls and reported two functional-abnormality trajectories:
<https://pmc.ncbi.nlm.nih.gov/articles/PMC12136716/>. A Nature Mental Health
study reported three structural MDD subtype trajectories in 1,789 patients and
2,151 controls:
<https://www.nature.com/articles/s44220-023-00139-4>. This episode therefore
does not claim novelty from applying SuStaIn to depression. Its contribution is
the private-data benchmark: testing transport, treatment-response relevance,
and model-family robustness in cohorts not used by the recent public
large-sample MDD DPM literature.

The fifth benchmarked method is a variational autoencoder inspired by Li et
al.'s functional network connectivity interpolation framework, which used
static and dynamic FNC from controls and patients with schizophrenia or autism
to generate neuropsychiatric continua and estimate where individuals fall
along those continua:
<https://pmc.ncbi.nlm.nih.gov/articles/PMC13125074/>. In EP22, this VAE is a
latent-continuum comparator and possible depression-continuum model, not a
classical DPM. Its inclusion tests whether a nonlinear generative latent space
captures clinically relevant depression heterogeneity beyond subtype/stage
models.

## Study logic

The benchmark has four linked questions.

| Question | Decision target |
| --- | --- |
| Do cross-sectional subtype-stage models recover reproducible MDD structure? | Stability of EBM/SuStaIn subtype and stage estimates within Chinese MDD and within UCLA-UNM baseline data. |
| Does a VAE latent continuum recover reproducible depression heterogeneity? | Reconstruction, interpolation, latent-coordinate stability, and clinical association of FNC-derived latent dimensions. |
| Do inferred stages or latent coordinates align with clinical severity without collapsing into severity alone? | Association with HAMD/HDRS/MADRS or available symptom domains after age, sex, site/scanner, medication, motion, and baseline severity controls. |
| Does longitudinal information change the answer? | Whether ECT follow-up and any other repeated measures support DCM/GPPM-style trajectories or falsify purely cross-sectional stage orderings. |
| Do coordinates transport across datasets? | Train/fit on one dataset or site split, assign stages/subtypes or latent coordinates in the others, and evaluate held-out symptom/treatment endpoints without refitting to outcomes. |

The primary empirical claim, if supported, is limited:

> A specified method among EBM, DCM, GPPM, SuStaIn, and VAE yields
> reproducible, outcome-relevant subtype, stage, trajectory, or latent-continuum
> coordinates for depression under prospectively separated training,
> calibration, and held-out evaluation roles.

It will not claim causal disease progression, neurodegeneration, treatment
mechanism, or clinical readiness unless those claims are explicitly added with
new data and a stronger design.

## Benchmarked methods

The benchmark should include exactly five primary methods, with each model
evaluated only when its data assumptions are satisfied.

| Model family | Intended role | Required data support |
| --- | --- | --- |
| Event-Based Model (EBM) | Single-trajectory ordering of abnormal biomarkers; baseline cross-sectional benchmark. | Case/control or normative abnormality definitions and baseline biomarkers. |
| Disease Course Mapping (DCM) | Continuous average and individual disease-course trajectories; longitudinal benchmark where repeated visits exist. | Repeated within-person clinical or imaging measures over meaningful time. |
| Gaussian Process Progression Model (GPPM) | Uncertainty-aware latent disease time and trajectory estimation. | Longitudinal biomarker measurements and enough temporal support for individualized time warping. |
| Subtype and Stage Inference (SuStaIn) | Subtype and stage inference under heterogeneous trajectories; main cross-sectional subtype/stage benchmark. | Baseline biomarkers, enough participants per subtype/stage, control-derived or normative z-scoring. |
| Variational autoencoder (VAE) FNC interpolation | Nonlinear latent-continuum and imputation/interpolation comparator inspired by Li et al. | Static and/or dynamic functional network connectivity features, controls or low-symptom references, and enough samples for training/held-out reconstruction and latent-coordinate validation. |

pySuStaIn is a practical implementation anchor because it exposes mixture,
z-score, ordinal, and mixed-type SuStaIn variants:
<https://ucl-pond.github.io/pySuStaIn/>.

Non-DPM calibration guardrails remain required but are not counted as primary
benchmarked methods: severity-only, site/scanner-only, linear latent-space, and
ordinary supervised prediction baselines.

## Primary endpoints

The episode should lock endpoints after structural manifests are available and
before empirical outcomes are inspected. Candidate endpoints are:

- subtype/stage stability under site-, sex-, and age-stratified resampling;
- VAE reconstruction error, latent-coordinate stability, and interpolation
  plausibility on held-out FNC observations;
- held-out association between baseline DPM stage and depression severity;
- held-out prediction of ECT symptom change, response, remission, or relapse;
- transport of fitted subtype/stage or latent-continuum coordinates among
  Chinese MDD, UCLA-UNM ECT, LA5C, and MDD DIRECT where feature spaces are
  compatible;
- longitudinal consistency: whether subjects with follow-up move in the
  expected direction or show reversible treatment-related shifts that violate
  a progression interpretation.

The most decisive contrast is not whether a flexible model achieves the best
score, but whether a DPM-specific subtype/stage representation or VAE latent
continuum adds value over severity, demographics, site/scanner, baseline
biomarker norms, linear latent models, and ordinary supervised models.

## Minimum falsifiers

The DPM interpretation should be weakened or rejected if:

- inferred stage is mostly a proxy for scanner/site, motion, age, medication,
  or baseline depression severity;
- subtypes are unstable under outcome-blind resampling or disappear under
  site-balanced splits;
- stages do not transport across datasets with compatible features;
- ECT follow-up shows large reversible shifts that contradict the locked
  interpretation of a monotone disease-progression axis;
- treatment response is predicted no better than by severity and site controls;
- VAE latent axes are unstable, reconstruct site/scanner rather than clinical
  variation, or fail to improve over a linear PPCA/PCA-style latent baseline;
- feature harmonization decisions materially change the selected subtype count
  or event ordering.

## Current status

No empirical payload is provisioned in this repository for this episode. This
draft creates a local study design only. It does not authorize data mounting,
outcome inspection, model fitting, compute launch, canonical Brain Researcher
registration, or a formal campaign action.

Before execution, the scientist must provide or authorize structural manifests
for all four private datasets, including participant counts, visits, feature
spaces, symptom measures, treatment timing where applicable, site/scanner
labels, controls or normative references, FNC availability for the VAE branch,
and permitted data-use boundaries.
