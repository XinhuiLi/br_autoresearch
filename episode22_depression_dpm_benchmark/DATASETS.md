# Dataset contract -- Episode 22 draft

EP22 is a proposed private-data benchmark of disease progression and
latent-continuum modelling in depression. The four intended datasets are:

1. Chinese MDD.
2. UCLA-UNM ECT.
3. LA5C.
4. MDD DIRECT.

Neither dataset is currently mounted, qualified, or executable in this
workspace. Counts, feature spaces, acquisition details, and clinical endpoints
remain unknown until structural manifests are supplied. All private empirical
payloads are read-only once provisioned.

## Dataset roles

| Dataset | Planned role | Current availability | Required before execution |
| --- | --- | --- | --- |
| Chinese MDD | Primary MDD subtype/stage discovery and/or cross-dataset training source. | Not provisioned. | Structural manifest, data-use approval, feature dictionary, participant/visit table, control or normative reference definition, clinical measures, site/scanner labels, missingness masks. |
| UCLA-UNM ECT | Treatment-response and site-transport benchmark; potential longitudinal DPM branch. | Not provisioned. | Structural manifest, UCLA/UNM site labels, ECT timing, baseline and post-treatment symptom scores, relapse/remission definitions, medication and electrode-placement metadata where available, imaging/clinical feature dictionary. |
| LA5C | Additional depression cohort for cross-dataset transport, robustness, and site/cohort generalization. | Not provisioned. | Structural manifest, data-use approval, cohort/site labels, feature dictionary, participant/visit table, clinical measures, controls or reference definition, missingness masks. |
| MDD DIRECT | Additional depression cohort for cross-dataset transport, robustness, and independent endpoint evaluation where compatible. | Not provisioned. | Structural manifest, data-use approval, cohort/site labels, feature dictionary, participant/visit table, clinical measures, controls or reference definition, missingness masks. |

Published UCLA-UNM ECT work suggests this family of data has included UCLA and
UNM depression patients, healthy controls, HDRS assessments before and after
ECT, and site-specific MRI acquisition. These are literature context only, not
authenticated EP22 row counts or variables. They must be verified from the
private dataset manifests before use.

## Permitted pre-execution visibility

At the design and qualification stages, EP22 may use only outcome-blind
structural information:

- dataset and release identifiers;
- participant, site, scanner, session, visit, and treatment-time IDs;
- diagnosis and control-role labels;
- demographic variables needed for balancing and normative modelling;
- feature names, shapes, dtypes, valid/NaN masks, missingness, and coverage;
- planned clinical endpoint names and timepoints without outcome values;
- data-use terms, IRB/DUA constraints, and permitted compute locations;
- public papers or documentation describing the cohorts at aggregate level.

The following remain forbidden before a protocol lock and separate execution
authorization:

- private imaging, clinical, or biomarker values;
- symptom change, response, remission, relapse, or outcome summaries;
- model fits, subtype assignments, stages, event orderings, predictions, or
  candidate rankings;
- selected plots or reports derived from private outcomes;
- sibling episode outputs or scratch trees as model-selection inputs.

## Required structural manifest fields

Each dataset should provide a redacted manifest under `inputs/` before any
empirical scoring:

| Field group | Required content |
| --- | --- |
| Identity | Dataset name, version, provider, private/public status, data-use restrictions, permitted compute environment. |
| Participants | Subject IDs or pseudonymous IDs, diagnosis/control role, family or repeated-measure grouping if applicable. |
| Visits | Baseline and follow-up timepoints, days from baseline or ECT course, repeated-measure links. |
| Sites | Site, scanner, acquisition protocol, UCLA/UNM indicator where applicable, batch/harmonization variables. |
| Clinical | Available scales such as HAMD/HDRS/MADRS, symptom domains, response/remission/relapse definitions, treatment timing, medication and ECT parameters where available. |
| Imaging/features | Modality, preprocessing state, parcellation/ROI definitions, functional network connectivity availability, static/dynamic FNC definitions, feature names, shapes, missingness, quality masks, motion summaries, harmonization status. |
| Controls/norms | Healthy-control availability, normative model inputs, reference cohort boundaries, and whether controls are site matched. |
| Data roles | Proposed development, calibration, and held-out split labels, with grouping constraints and exact leakage checks. |

The manifest may report counts, masks, and Boolean validity checks. It may not
contain clinical outcomes, biomarker values, private image-derived summaries,
or preliminary DPM results.

## Feature compatibility

Cross-dataset transport is evaluable only for features with compatible meaning
and preprocessing. If Chinese MDD, UCLA-UNM ECT, LA5C, and MDD DIRECT have
different modalities, parcellations, or FNC definitions, the design should use
one of these prelocked routes:

- a common ROI feature space with documented atlas compatibility;
- a common static and/or dynamic FNC feature space for the VAE branch;
- a site-balanced subset of features present in both datasets;
- separate within-dataset models plus transport only of model-derived
  low-dimensional summaries when justified without outcome inspection;
- no cross-dataset transport claim if compatibility cannot be established.

Harmonization must be fitted within declared training roles only. Held-out
clinical endpoints and held-out biomarker rows cannot influence ComBat,
normative z-scoring, feature selection, model selection, subtype count, or
threshold choices.

## Longitudinal, treatment, and VAE roles

UCLA-UNM ECT is the natural anchor for treatment-response endpoints. If it has
baseline and post-treatment observations, the primary treatment question is
whether baseline DPM subtype/stage predicts later symptom change beyond
baseline severity, demographics, site, and treatment variables.

If repeated imaging or repeated clinical biomarkers exist, longitudinal DPM
methods may be evaluated. Their claims must be phrased carefully:

- a monotone disease-stage model may be inappropriate if ECT induces reversible
  biomarker changes;
- treatment response can be a prognostic endpoint without being a disease
  progression endpoint;
- longitudinal improvement after ECT should be treated as a possible
  falsifier of irreversible progression, not hidden as noise.

Chinese MDD, LA5C, and MDD DIRECT follow-up, if available, may support
naturalistic longitudinal stability or symptom-course tests. If a cohort is
cross-sectional only, it supports EBM, SuStaIn, VAE latent-continuum, and
baseline prediction benchmarks but not individual continuous trajectory claims.

The VAE branch requires static and/or dynamic functional network connectivity
features with matched definitions across all datasets included in a shared
model. It may learn a nonlinear control-to-patient or low-symptom-to-high-
symptom continuum, reconstruct held-out FNC, impute missing FNC entries where
prelocked, and assign latent coordinates. It does not by itself establish
disease progression, temporal ordering, or treatment mechanism.

## Split and exposure rules

Before execution, EP22 needs an outcome-blind split plan:

- group all visits from the same participant on one side of any train/test
  split unless the analysis is explicitly a within-person longitudinal model
  with a valid forecasting design;
- hold out at least one site or site-balanced partition when site transport is
  a claim;
- keep UCLA and UNM labels available for site-stratified evaluation;
- reserve a final held-out clinical endpoint opening for ECT response if
  response/remission is a primary claim;
- report cross-dataset transfer among Chinese MDD, UCLA-UNM ECT, LA5C, and
  MDD DIRECT only when feature and covariate support are compatible.

If any private datasets share participants, acquisition pipelines,
preprocessing teams, prior analyses, or model-derived feature pipelines, their
dependence must be recorded in an exposure ledger before they can be called
independent evidence.

## Input interfaces

Expected stable interfaces are documented in `inputs/README.md`. No empirical
payload has been added.
