# EP22 inputs

EP22 has no provisioned empirical input as of 2026-10-06. The paths below are
stable interfaces for a future private-data benchmark of disease progression
models in depression. They do not imply that data access, mounting, scoring, or
compute has been authorized.

## Expected interfaces

| Input ID | Expected path | Permitted content before protocol lock |
| --- | --- | --- |
| `chinese_mdd_structural_manifest` | `inputs/chinese_mdd_structural_manifest.json` | Redacted participant, visit, site, scanner, feature/FNC, clinical-variable, role, shape, dtype, missingness, and data-use metadata; no private values or outcomes. |
| `ucla_unm_ect_structural_manifest` | `inputs/ucla_unm_ect_structural_manifest.json` | Redacted UCLA/UNM participant, visit, ECT timing, endpoint-name, site/scanner, feature/FNC, role, shape, dtype, missingness, and data-use metadata; no private values or outcomes. |
| `la5c_structural_manifest` | `inputs/la5c_structural_manifest.json` | Redacted LA5C participant, visit, site/scanner, feature/FNC, clinical-variable, role, shape, dtype, missingness, and data-use metadata; no private values or outcomes. |
| `mdd_direct_structural_manifest` | `inputs/mdd_direct_structural_manifest.json` | Redacted MDD DIRECT participant, visit, site/scanner, feature/FNC, clinical-variable, role, shape, dtype, missingness, and data-use metadata; no private values or outcomes. |
| `public_method_sources` | `inputs/public_method_sources/` | Optional local copies of public method documentation or citation metadata, if licenses permit. |
| `qualified_feature_handoff` | `inputs/qualified_feature_handoff/` | Post-lock, read-only feature matrices and manifests for the exact authorized analysis roles. |
| `qualified_clinical_handoff` | `inputs/qualified_clinical_handoff/` | Post-lock, read-only clinical variables and endpoint values for authorized training/evaluation roles. |

The manifest interfaces are for qualification. Feature and clinical handoffs
remain unavailable until a protocol lock exists and the scientist separately
authorizes empirical execution.

## Manifest rules

Structural manifests may contain counts, IDs, role labels, shapes, missingness,
finite/NaN masks, and Boolean split/leakage checks. They may not contain:

- imaging or clinical feature values;
- symptom-change, response, remission, or relapse values;
- private outcome summaries;
- subtype assignments, stages, event orders, predictions, scores, or plots.
- VAE latent coordinates, reconstructions, interpolation outputs, or generated
  FNC samples.

Every empirical payload later added under `inputs/` is read-only. Generated
features created by EP22, model fits, predictions, logs, figures, and reports
belong under `outputs/` or the episode-specific scratch path.
