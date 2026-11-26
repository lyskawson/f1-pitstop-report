# Evidence map

Report update: 2026-11-26. Accepted application source:
`1c76890244470588e5778fe38ad63579aca4f9d4` (T12b squash merge, PR #23).
Previous documented source: `5ba376f904b58caf2072ce807cc822db7a90ccc6` (T2).
All versioned application files were read at this SHA with read-only `git show`;
the live application checkout was not changed. Evidence was read, not rerun.
No PDF was built locally (human decision: the team compiles in Overleaf).

`evidence/provenance.json` holds sizes and SHA-256 of the consulted source files
at the SHA above, plus the retained hashes of the earlier private T0/T2 evidence.
`evidence/summary.json` holds selected aggregates only. Evidence paths starting
`reports/T0-`, `reports/T2-` refer to the private evidence archive of the first
report. Paths `reports/validation-v1.*`, `reports/test-v1*` and `demo/` are in
the application repository at the source SHA. The PDF bibliography cites project
links (repository, findings, data-v1 release, validation and test reports).

## Revisions

| Role | Full SHA | Interpretation |
| --- | --- | --- |
| Accepted source main | `1c76890244470588e5778fe38ad63579aca4f9d4` | Scope of this report (T12b, PR #23). |
| Previous source main | `5ba376f904b58caf2072ce807cc822db7a90ccc6` | T2, scope of the first report. |
| T11 runs produced at | `f91914766761844937bd1e83e77740daa5af8a17` | Code revision of all 36 runs; validation study. |
| T12 test evaluation at | `75b39bc7d1c3ceaf01cd53ae904dabbaf81041f9` | Single locked test evaluation. |
| T12 merge | `19cfcb01341da7d938cab5484e28415694778f39` | PR #21, 359 tests in CI per handoff. |
| Demo export commit | `0b0b55e5fa32b604fc8548dd127337e89e85b8e6` | `demo/metadata.json`: `export_git_commit`. |
| T0 corrected audit | `49a38c9089cfa992287f832ef48b57f6089f72ef` | Corrected `summary-after.json`. |
| Snapshot producer | `e96ae051380b927f20017dfb98f411f0a3ffe38e` | Produced data-v1 candidate. |
| T2 corrected verifier | `e4d484d62e52b85bb1642b2ed78d4b7af3c01290` | Verifier before merge 5ba376f. |

Stage merge commits (HANDOFF.md): T4c b2f2db0, T4d f1b3676, T4 c4bde16, T6 9859030,
T8 2f97102, T5b ac26cf4, T9 ad4ce2a, T10 67460cd, T11 228f320, T7 38a17d3,
T12 19cfcb0 (full SHAs and PR/CI links in `docs/handoff/HANDOFF.md` at the source SHA).
Earlier: T2c PR #4 (`3de4c38`), T3 `515e72e`, T5 `4927786`.

## Claims in the PDF

Chapter and section numbers refer to the five-chapter report.

| PDF location / claim | Source artifact and selector | Qualification |
| --- | --- | --- |
| 1, goal, sample key, time cutoff | `docs/decisions.md` Context, D2, D5 | Next-lap tyre change; no optimal-strategy claim. |
| 2.1, 92 races, 101290 raw rows, shared archive | `HANDOFF.md` "Shared data"; T2 `coverage-summary.json`; data-v1 release | Archive SHA-256 `f30bff5f...c09`. Raw coverage, not dataset size. |
| 2.1, 780 Compound nulls, 18 whole-race exclusions, cleaning thresholds | T0 `summary-after.json`; D3 | Unchanged from the first report. |
| 2.1, legacy acquisition times unknown | `docs/contracts.md` raw snapshot section and contract change log 2026-11-08 (T2c) | Scoped allowance for legacy cache sources only. Formal human acceptance of data not recorded in sources read. |
| 2.2, label rule | `docs/decisions.md` D2 (revised 2026-11-24); `labels.py`; `accepted-work.md` T3 | Unresolved visits omitted by design; all v1 confirmation mappings are empty. |
| 2.2, 1706 positives, 1167 omitted | `accepted-work.md` T3 (`training-label-summary.json` of the author) | Training split only, transcribed author aggregates. |
| 2.2, 3339 / 3363 Jolpica matches | T0 `summary-after.json` sums of `e_pit_matches` | Audit proxy for the old indicator, includes penalties. |
| 2.3, Table 2.1 splits and sample counts | `HANDOFF.md` "Shared data"; `reports/validation-v1.md`, `reports/test-v1.md` (val and test lap and positive counts); D4; `manifests/splits.json` | Train counts only from HANDOFF/accepted-work. Race counts 53/10/11 of 68/12/12. |
| 2.4, causality rules | `docs/decisions.md` leakage rules, D5; `docs/contracts.md` feature rules 1-11, lap-count section | `lap_time_rel` 0 until first accurate lap (approved 2026-11-25). |
| 2.4, lap-count announcement examples | T0 summary races `2023_14_R`, `2025_11_R` | Final announcements precede all recorded lap ends; no leak shown. |
| 3.1, pipeline, manifest, verifier | `ingest.py`, `cli.py`, `docs/runbook.md` T2, T4, T12b | Content checks beyond hashes. |
| 3.2, 15 features, window 15, 16 channels | `contracts.py`, `docs/contracts.md`, D5 | Standardized: nine features, per contract rule 7. |
| 3.3, model descriptions | `src/pitstop/models/*.py`, `configs/final/*.yaml`, D6, D7, runbook T8-T10 | MLP 64-32, GRU 2 layers, TCN receptive field 29; patience 8. |
| 3.3, tuning grid, seeds, per-run times | `validation-v1.md` Tuning; HANDOFF environment notes | Grid lr x hidden at seed 42; times at 12 threads. |
| 3.4, metric and threshold procedure | D8; `metrics.py`, `evaluate.py`; runbook T5 | Synthetic verification only for perfect scores. Leak alarm at 0.6. |
| 3.5, dashboard and demo pack | `app/streamlit_app.py`, runbook T7/T12b, `demo/metadata.json` | Pack: 3 races, seed-42 runs, event names, SHA-256 of payloads. Heatmap, comparison view and deployment not implemented. |
| 4.1, tests and 359 CI | `HANDOFF.md` stage table; `accepted-work.md` | 359 refers to the T12 merge. CI of 1c76890 not inspected. Review finding: rival PitInTime leak fixed in T4. |
| 4.2, validation results | `reports/validation-v1.md` Tuning, Models, Green vs SC/VSC, paired bootstrap | Means over seeds 42-44; val bias caveat quoted from the report. |
| 4.3, Table 4.2 test results | `reports/test-v1.md` Models val vs test, test groups, paired bootstrap | Single evaluation; no ranking claimed; 11 races. |
| 4.3, val to test comparison | Both reports | Interpretation: selection bias plus different races. Not tested separately. |
| 5, conclusions and completion table | Above; HANDOFF "Continue after T12b" | Remaining: T13a, T13, T14. |

## Technical details retained outside the PDF

`runbook.md` retains environment pins, commands, file formats and operational
controls. Numeric detail removed from the PDF stays in `evidence/summary.json`.
Additional items from the first report:

| Evidence detail | Original selector / interpretation |
| --- | --- |
| Seasons 22/22/24/24 races | T2 coverage summary `requested_by_season`. |
| Miami driver 4: 24 missing Compound / 57 rows | T0 summary, race `2025_06_R`, driver `4`. |
| Penalty example Baku 2022, driver 6, laps 3-4 | T0 summary race `2022_08_R`, `b_penalties`; Stint 1 to 2, TyreLife 3 to 4, MEDIUM. Table removed from the PDF. |
| 24 unmatched Jolpica visits | Sum of `e_pit_matches.unmatched_jolpica`. |
| Lap-count examples | `2023_14_R`: 53, 52, 51; `2025_11_R`: 71, 70. |
| Japan 2022: 53 planned / 29 raw max / 28 non-generated max | T0 race `2022_18_R`. |
| 97 in-lap offsets beyond recorded lap end | Sum of `a_pit_boundaries.PitInTime.outside_lap`. |
| Exact SC 2806 / 0 accurate; any SC 5260 / 1017 | T0 `g_accuracy`. |
| T3 omission reasons 1025 / 61 / 78 / 2 / 1; evidence 1337 + 369 | `accepted-work.md` T3. |
| Tuning grid per model | `validation-v1.md` Tuning table. |
| Group metrics (green / SC-VSC / special tyre rule) | Both reports; `evidence/summary.json`. |

## Data and template identity

Archive `data/raw/v1.tar.gz`: 1438537 bytes, SHA-256
`f30bff5fdaa6c280203483e878ca1e4c9189761e7471b6e8a83d74d93e09c410`
(data-v1 release). Manifest SHA-256 (processed-data hash used by all runs):
`36ee960d81516ad4c3a4b514686d13da667f393868d567b39c99557ea376f0f5`.
runs-v1 release archive SHA-256:
`122b60c80561a9f32d361b2431c44b854ba16691b44d41e2acbb954bc94986f6`.
Template identity and title-page metadata are unchanged (`evidence/provenance.json`).
The retained data archive and runs are not part of this repository.

## Course requirement checklist

| Supplied requirement | Current primary section | Limitation |
| --- | --- | --- |
| Description of the project | Ch. 1, 3.1 | Complete up to the demo pack. |
| Implementation / realization | Ch. 2-3, Figure 3.1 | Dashboard has the race view only. |
| Technologies and their roles | 3.6, Table 3.1 | |
| Tests and conclusions | Ch. 4, 5.1 | Test has 11 races; no ranking. |
| Possible applications | 5.3 | Historical analysis only. |
| Degree of goal completion | Table 5.1 and text | Main goal achieved; presentation part partial. |

Local PDF compilation and the visual check were not performed in this update;
Overleaf compilation is the first full build of the new sources.
