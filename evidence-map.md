# Evidence map

Report update: 2026-11-09. Accepted application source:
`5ba376f904b58caf2072ce807cc822db7a90ccc6` (T2 squash merge).
All versioned application files were read at this SHA from a temporary archive;
the live application checkout was not changed. Evidence was read, not rerun.

Full file sizes/SHA-256 identities are in `evidence/provenance.json`. Paths
starting `reports/` below refer to the separate project evidence archive;
the private originals are not copied here. `evidence/summary.json` contains
selected aggregates only. Hashes identify evidence but do not make inaccessible
evidence publicly reproducible. Public code links in the bibliography remain
pinned to the frozen source SHA.

## Revisions

| Role | Full SHA | Interpretation |
| --- | --- | --- |
| Accepted source main | `5ba376f904b58caf2072ce807cc822db7a90ccc6` | Scope of this report. |
| T0 corrected audit | `49a38c9089cfa992287f832ef48b57f6089f72ef` | Produces corrected `summary-after.json`; see `analyze-result.json`. |
| T0 squash merge | `997f07b712427b6ca801f9d9540612a5b7c94f22` | Preserves accepted T0 findings and fixtures. |
| Snapshot producer | `e96ae051380b927f20017dfb98f411f0a3ffe38e` | Produces candidate v1, not the corrected verifier revision. |
| T2 corrected verifier | `e4d484d62e52b85bb1642b2ed78d4b7af3c01290` | Final local checks/CI, subsequently squash-merged. |

## Claims and selectors

| Report location / claim | Source artifact and selector | Revision / limitation |
| --- | --- | --- |
| Ch. 1, current implementation | `src/pitstop/cli.py`, `ingest.py`, `contracts.py`; later-stage module bodies; `reports/T2-merge-evidence.json` | Frozen main. Merge is distinct from snapshot acceptance. |
| Ch. 2, 92/92 races, 101290 raw rows; seasons 22/22/24/24 | `reports/T2-evidence/coverage-summary.json`: `rows`, `requested_by_season`; `reports/T2-fix1-evidence/local-data-summary.json`: `results.valid.stdout`; canonical `manifests/data.json` | Snapshot produced at e96ae05, checked at e4d484d. Raw coverage is not labelled-dataset size. |
| Ch. 2, 18 whole-race exclusions | T0 `summary-after.json`: `coverage.excluded`; T2 coverage summary: count distinct `whole_race_exclusions` | Wet/red screening only; preserve driver-level exclusions separately. |
| Ch. 2, 780 Compound nulls | T0 summary: sum `races[].driver_coverage[].missing.Compound`; T2 summary: `missing.Compound` | T0 correction on 49a38c9; normalization is not imputation. |
| Ch. 2, Miami driver 4: 24/57 | T0 summary: race `2025_06_R`, driver `4`, `missing.Compound`, `rows` | Missing history stays unknown; does not explain every unmatched visit. |
| Table 2.2, penalty example | Frozen `docs/fastf1-findings.md`, Penalty evidence; T0 summary: race `2022_08_R`, `b_penalties`, driver `6`, context laps 3–4 | Stop/go context; Stint 1→2, TyreLife 3→4, PitInTime 4131.373 / PitOutTime 4160.747. No new label rule. |
| Ch. 2, 3339 matched / 3363 Jolpica visits; 24 unmatched | T0 summary: sum `races[].e_pit_matches.{fastf1_changes,jolpica_stops,matched}` and lengths of `unmatched_jolpica` | Audit proxy, after generated rows removed; not model precision/recall or confirmed tyre changes. |
| Table 2.3, announcement counts and times | T0 summary: races `2023_14_R` and `2025_11_R`, `c_total_laps.announcements` non-null `TotalLaps`, final such `Time`, `earliest_recorded_lap_end` | 53→52→51 at 4411.732 before 5028.992; 71→70 at 3485.191 before 4357.412. Mutable metadata, no demonstrated future-count leak into these samples. |
| Ch. 2, Japan 53 / 29 / 28 | T0 summary: race `2022_18_R`, `c_total_laps.{loader,maximum_recorded_lap,maximum_non_generated_lap}` | Distinct populations; do not replace planned count with observed maximum. |
| Ch. 3, versions and roles | Frozen `uv.lock`, `pyproject.toml`, `.github/workflows/ci.yml` | Lock versions, not proof of model use. CI uv 0.11.7; Python 3.12; CPU torch variant recorded separately. |
| Ch. 3, actual CI scope | `.github/workflows/ci.yml` and `tests/test_contracts.py` socket checks | Setup can fetch dependencies; tests are offline. No training smoke test exists yet. |
| Ch. 4, schemas / 15 features / 15×16 windows | Frozen `contracts.py` constants and validators; `docs/contracts.md` | Configured dimensions and shape checks, not built features/windows. |
| Ch. 4, exporter, preservation and failure handling | Frozen `ingest.py`: `raw_laps`, `coverage`, `offline_cache`, `cached_call`, `load_race`, `build_snapshot`; `tests/test_ingest.py` | D3 metadata over raw rows. All requested races must succeed before local publication. |
| Ch. 4, independent verifier and exit codes | `verify_data`, `safe_payload`; CLI `main`; snapshot and negative tests | Hashes plus content checks; incomplete=2, malformed=1, valid complete=0. Optional legacy streams remain supported. |
| Ch. 4, unknown acquisition times and T2c pending | `ingest.py` source metadata; frozen runbook; `reports/T2-merge-evidence.json`: `data_acceptance`; private handoff orientation | Null timestamps are explicit, not invented. Human legacy allowance does not complete the contract PR. |
| Ch. 5, fixture rows 1125 / 820 | T0 summary: `races[].lap_rows` for `2022_01_R` / `2022_02_R`; `docs/fastf1-findings.md`; `tests/test_fixtures.py` | Complete raw tables, no models. |
| Ch. 5, 209 tests and Ruff pass | `reports/T2-fix1-evidence/final-acceptance.log`; result JSON; `final-integrity.json` | Author's checks on e4d484d, not new R0 checks or a second independent review. |
| Ch. 5, green CI and merged T2 | `reports/T2-merge-evidence.json`: `pre_merge.statusCheckRollup`, `merged_pr` | CI run 42613253212, head e4d484d, merge 5ba376f. |
| Ch. 5, fresh extraction and negative cases | `reports/T2-fix1-evidence/local-data-summary.json`: `results.{valid,missing_streams,null_status,null_messages}`; `final-local-data-result.json` | Valid=0, invalid copies=1; FastF1/cache/auth/network blocked. Original archive unchanged. |
| Ch. 5, 97 in-lap timestamp offsets | T0 summary: sum lengths of `races[].a_pit_boundaries.PitInTime.outside_lap`; frozen findings | Raw marker association retained; no label reassignment. |
| Ch. 5, exact/mixed SC distinction | T0 summary: sum `races[].g_accuracy.after_exact_sc.{observations,accurate}` = 2806/0; `after_any_sc` = 5260/1017 | Exact previous `4` is narrower than strings containing `4`. |
| Ch. 5, leakage implementation vs plan | Existing validators/tests versus unimplemented `features.py`, `windows.py`, `labels.py`; `docs/decisions.md` Leakage rules | No truncation/features pipeline yet. A decision heading is not test evidence. |
| Ch. 5, future split/models/metrics | D1, D4–D8; configs and contracts | Protocol only. No training, val/test model results, locked-test evaluation or model comparison. |
| Ch. 6–7, conclusions | Above code/evidence and unresolved D2/D5/T2c | Partial goal completion, no optimal strategy or causal adviser. |

## Data and template identity

Candidate archive: `data/raw/v1.tar.gz`, 1438537 bytes,
SHA-256 `f30bff5fdaa6c280203483e878ca1e4c9189761e7471b6e8a83d74d93e09c410`.
The canonical manifest's SHA-256 is
`36ee960d81516ad4c3a4b514686d13da667f393868d567b39c99557ea376f0f5`.
The manifest hash was checked directly against the frozen source copy;
archive identity is taken from existing T2 evidence, without rebuilding data.
Neither candidate archive nor private logs are published with this report.

Original template: `Sieci_neuronowe_projekt`, five files inspected 2026-11-09;
individual hashes in `evidence/provenance.json`. Supplied title block and logo
are preserved. No new author metadata, institutional affiliation details,
licence or third-party research citations were invented.

## Course requirement checklist

| Supplied requirement | Report coverage | Current limitation |
| --- | --- | --- |
| Description of completed project | Ch. 1 and status table | Only accepted stages. |
| Implementation / realization | Ch. 2 and 4, data-flow figure | Later stages explicitly pending. |
| Technologies used | Ch. 3 and lock-version table | Installed future model dependencies identified as such. |
| Tests and conclusions | Ch. 5–6 | Existing checks, not predictive experiments. |
| Possible applications | Ch. 1 and 6 | Historical analysis; no optimal-strategy claim. |
| Degree of goal completion | Ch. 7 and status table | Partially achieved. |
| Reproducibility and references | Appendix A, bibliography, README, provenance | Private evidence requires separate access; Overleaf not connected. |
