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

## Claims in the consolidated PDF

Section numbers refer to the five-chapter editorial revision. This pass reuses
existing evidence and the same application SHA; no scientific results were added.

| PDF location / claim | Source artifact and selector | Qualification |
| --- | --- | --- |
| Ch. 1, scope and goal | Frozen decisions/contracts and source tree | Next-lap tyre-change prediction; no optimal-strategy claim. |
| §2.1, 92/92 races and 101290 raw rows | `reports/T2-evidence/coverage-summary.json`: `rows`, `requested_by_season`; `reports/T2-fix1-evidence/local-data-summary.json`: `results.valid.stdout` | Produced at e96ae05, verified at e4d484d. Raw coverage, not labelled-dataset size. |
| §2.1, 780 Compound nulls | T0 `summary-after.json`: sum `races[].driver_coverage[].missing.Compound`; T2 summary: `missing.Compound` | Corrected on 49a38c9; normalization is not imputation. |
| §2.1, 18 whole-race exclusions and cleaning rules | T0 `coverage.excluded`; T2 `whole_race_exclusions`; frozen D3/configuration | Three-lap/10% thresholds are policy, not predictive measurements. Exclusions remain raw metadata. |
| Table 2.1, penalty example | Frozen `docs/fastf1-findings.md`, Penalty evidence; T0 summary: race `2022_08_R`, `b_penalties`, driver `6`, context laps 3–4 | Stop/go context; Stint 1→2, TyreLife 3→4, Compound MEDIUM. No approved replacement classifier. |
| §2.2, 3339 matches / 3363 Jolpica visits | T0 summary: sum `races[].e_pit_matches.{fastf1_changes,jolpica_stops,matched}` | Matching tolerance ±1 lap. Audit proxy after generated rows removed; not model accuracy or verified tyre changes. |
| §2.2, raw pit marker association / timing offsets | T0 `a_pit_boundaries`; frozen findings | Preserve association; do not repair labels by moving timestamps between laps. |
| §2.3, retained announcement examples | T0 races `2023_14_R` and `2025_11_R`, `c_total_laps.announcements` and `earliest_recorded_lap_end` | Final announcements precede all recorded lap ends in both cases; mutable metadata, no demonstrated future-count leak into those samples. |
| §2.3, exact/mixed SC accuracy | T0 `g_accuracy.after_exact_sc` / `after_any_sc`; frozen findings | Previous exact `4` is narrower than strings containing `4`. Full counts retained below. |
| §2.3, planned causal checks | Frozen Leakage rules and unimplemented feature/window/label modules | No feature truncation test or causal builder exists yet; raw format checks do not prove causality. |
| §3.1, pipeline and implemented scope | Frozen `ingest.py`, `cli.py`, source tests; `reports/T2-merge-evidence.json` | Export/verifier accepted as code; no snapshot acceptance implied. |
| §3.1, content verification and clocks | `verify_data`, `load_race`, `build_snapshot`; frozen runbook | Bytes plus content checks; session seconds versus UTC retained; lap-count archive has no approved consumer semantics. |
| §3.1, unknown acquisition times | Source metadata and frozen runbook; merge evidence `data_acceptance`; original handoff orientation | Legacy timestamps explicitly unknown. T2c and human acceptance remain pending. |
| §3.2, 15 features, 15 steps, 16 channels | `contracts.py` constants/validators; `docs/contracts.md` | Defined interfaces and existing shape/padding checks, not implemented inputs. |
| §3.3, technology roles and planned models | Frozen lock, pyproject, configs and model stubs | Exact pins in `runbook.md`; dependency presence does not prove model implementation. |
| §4.1, fixtures / representative checks | `tests/test_fixtures.py`, `tests/test_contracts.py`, `tests/test_ingest.py` | Real raw fixtures plus synthetic negative cases. |
| §4.1, 209 tests and Ruff pass | `reports/T2-fix1-evidence/final-acceptance.log`, result JSON, `final-integrity.json` | Author checks on e4d484d, not new R0 tests or an independent second review. |
| §4.1, green CI / accepted code | `reports/T2-merge-evidence.json`: `pre_merge.statusCheckRollup`, `merged_pr` | [CI run 42613253212](https://github.com/lyskawson/f1-pitstop/actions/runs/42613253212), [PR 3](https://github.com/lyskawson/f1-pitstop/pull/3); head e4d484d → merge 5ba376f. |
| §4.1, archive verification / invalid copies | `reports/T2-fix1-evidence/local-data-summary.json`: `results.{valid,missing_streams,null_status,null_messages}`; `final-local-data-result.json` | Valid copy accepted, altered copies rejected with FastF1/cache/auth/network blocked. |
| §4.2, short experiment plan | Frozen D4/D8 and contracts | Chronological split, val AP/F1, event windows and race bootstrap are protocol only. Test remains locked. |
| Ch. 5 / Table 5.1, completion and conclusions | Above code/evidence and unresolved D2/D5/T2c | Interpretation and partial completion, no invented experiments or approvals. |

## Technical details retained outside the PDF

`runbook.md` retains file/schema inventories, dependency pins, CLI behavior,
verification mechanics, clock distinctions, provenance limitations and evaluation
controls. The frozen application's contracts and runbook remain authoritative.
The following numeric evidence is preserved here and in `evidence/summary.json`
where previously extracted; removing it from the narrative does not discard it.

| Evidence detail | Original selector / interpretation |
| --- | --- |
| Seasons 22/22/24/24 races | T2 coverage summary `requested_by_season`. |
| Miami driver 4: 24 missing Compound / 57 rows | T0 summary, race `2025_06_R`, driver `4`, `missing.Compound` / `rows`. Does not explain every unmatched visit. |
| Penalty marker times | Same Baku context: PitInTime 4131.373 s, PitOutTime 4160.747 s. |
| 24 unmatched Jolpica visits | Sum lengths of `races[].e_pit_matches.unmatched_jolpica`; no automatic negative labels. |
| Retained lap-count examples | `2023_14_R`: 53→52→51, last announcement 4411.732 s before earliest lap end 5028.992 s; `2025_11_R`: 71→70, 3485.191 s before 4357.412 s. |
| Japan 2022: 53 planned / 29 raw max / 28 non-generated max | T0 race `2022_18_R`, `c_total_laps.{loader,maximum_recorded_lap,maximum_non_generated_lap}`; do not conflate populations. |
| Fixture sizes 1125 / 820 rows | T0 `races[].lap_rows` for `2022_01_R` / `2022_02_R`. |
| 97 in-lap offsets beyond recorded lap end | Sum lengths of `races[].a_pit_boundaries.PitInTime.outside_lap`. |
| Exact SC: 2806 observations / 0 accurate | Sum `g_accuracy.after_exact_sc.{observations,accurate}`. |
| Any status containing SC: 5260 / 1017 accurate | Sum `g_accuracy.after_any_sc.{observations,accurate}`; includes mixed statuses. |

## Data and template identity

Candidate archive: `data/raw/v1.tar.gz`, 1438537 bytes,
SHA-256 `f30bff5fdaa6c280203483e878ca1e4c9189761e7471b6e8a83d74d93e09c410`.
Canonical manifest SHA-256:
`36ee960d81516ad4c3a4b514686d13da667f393868d567b39c99557ea376f0f5`.
The first report checked the manifest hash against frozen source; archive
identity comes from existing T2 evidence. Neither was rebuilt for this revision.
Neither candidate archive nor private logs are published with this report.

Template: `Sieci_neuronowe_projekt`, five files inspected 2026-11-09; hashes in
`evidence/provenance.json`. Title-page metadata/layout and logo are unchanged.
No additional personal metadata, licence or research citation was added.

## Editorial baseline and structure

The reviewed 15-page PDF SHA-256 is
`61a6dcc393a4124ec8e532ea9b6255c1db308c9d631cf40c698cbde1322dee77`,
from report commit `66efab4fb519aa255afd1961a1991f13d41d6dd7`.
The read-only editorial assessment is `reports/R0-editorial-review.md` in the
private evidence archive; its identity is recorded in provenance. Before editing,
the report working tree was clean and local/remote main both matched that commit.
Prior PDF and source ZIP are retained locally under ignored
`build/archive/pre-edit1/`; previous sources also remain in Git history.

Old chapters 3/4 became chapter 3; old chapters 6/7 became chapter 5.
Data interpretation has its main home in chapter 2, software checks and the short
experiment plan in chapter 4. The appendix/build instructions moved to README
and runbook; exact historical evidence remains in this map/provenance.
The bibliography now contains two consulted project sources, with CI/merge
links retained here instead of separate bibliography entries.

## Course requirement checklist

| Supplied requirement | Current primary section | Limitation |
| --- | --- | --- |
| Description of completed project | Ch. 1, §3.1 and Table 5.1 | Accepted code distinguished from pending dataset decisions. |
| Implementation / realization | Ch. 2–3, data-flow figure | Feature/model stages remain planned. |
| Technologies used | §3.3 and Table 3.1 | Model dependencies identified as planned use. |
| Tests and conclusions | §4.1 and §5.1 | Software/data checks, not predictive accuracy. |
| Possible applications | §5.2 | Historical analysis; no optimal-strategy/causal adviser. |
| Degree of goal completion | Table 5.1 and conclusions | Partially achieved; one completion summary. |

Reproducibility is supported by the pinned bibliography, README, runbook,
evidence map and original artifact hashes. Private evidence still requires
separate access. Overleaf service compilation/sync is not claimed.
