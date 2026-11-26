# Technical companion to the report

Application scope: `1c76890244470588e5778fe38ad63579aca4f9d4` (T12b), previously
`5ba376f904b58caf2072ce807cc822db7a90ccc6`. This companion collects details kept
out of the academic PDF. It is not an application contract: the authoritative
`docs/contracts.md`, `docs/decisions.md` and `docs/runbook.md` live in the
application repository at that revision. Producing revisions are in `evidence-map.md`.

## Environment and entry points

Python 3.12 with uv. The lock records FastF1 3.9.0a0, pandas 2.3.3, NumPy 2.5.4,
PyArrow 26.0.0, PyYAML 6.0.3, scikit-learn 1.9.1, PyTorch 2.14.1 (CPU), LightGBM 4.7.0,
pytest 9.1.1, pytest-socket 0.8.1 and Ruff 0.17.1 (checked against `uv.lock` at the source SHA). Streamlit 1.67.0 is in the
`app` dependency group, with `app/requirements.txt` exported from the lock.
On Debian/WSL LightGBM needs `libgomp1`.

For the application checkout, not this repository:

```sh
uv sync --locked --extra train --group app
uv run --offline --no-sync ruff check . && uv run --offline --no-sync ruff format --check .
uv run --offline --no-sync pytest
```

These are reproduction instructions. They were not executed for this report update.
Environment installation needs network access; tests block Internet sockets.

## Pipeline commands (application)

```sh
uv run --offline --no-sync pitstop verify-data --manifest data/raw/v1/manifest.json
uv run --offline --no-sync pitstop build-dataset --config configs/data.yaml --snapshot v1
uv run --offline --no-sync pitstop train --config configs/<model>.yaml
uv run --offline --no-sync pitstop predict --run-dir runs/<run_id>
uv run --offline --no-sync pitstop evaluate --predictions ... --samples ... --data-manifest manifests/data.json --output-dir runs/<run_id>
uv run --offline --no-sync python -m pitstop.experiments tune|final|summary
uv run --offline --no-sync pitstop export-demo --output-dir demo
PITSTOP_DEMO_DIR=demo uv run --group app streamlit run app/streamlit_app.py
```

Data: the `data-v1` release (92 Race sessions, 101290 raw rows). Runs: the `runs-v1`
release (36 run directories). Never repeat the locked test evaluation (T12) and never
retrain the frozen runs. `export-demo` only reads saved files.

## Formats and clocks

Raw key `(race_id, DriverNumber, LapNumber)`; downstream key
`(race_id, driver_number, lap_number)`. Race IDs `YYYY_RR_R`. Raw times are float
seconds (session time); `race_control.parquet` uses ISO UTC text. Snapshot files:
`laps.parquet`, `events.json`, `track_status.parquet`, `race_control.parquet`,
`lap_count_evidence.json` (timestamped LapCount announcements, used for the lap
count known at T) and `manifest.json`.

Processed dataset: `samples.parquet` (row_id, key, y, split, sc, vsc,
special_tyre_rule), `tabular.npz` (float32 [N, 15]), `sequences.npz` (float32
[N, 15, 16], last channel `is_pad`, left padding), `preprocess.json` (feature order,
train-only scaling, hashes). Feature order: tyre_age, compound_soft, compound_medium,
compound_hard, stops_done, laps_remaining, mandatory_done, lap_time_rel, stint_deg,
position, gap_ahead, gap_behind, sc, vsc, rival_pits_2.

Demo pack (`demo/`): `predictions.parquet`, `samples.parquet`, `metrics.json`,
`thresholds.json`, `metadata.json` (rule `seed42_one_run_per_model` v1, run IDs,
race IDs and event names, payload SHA-256). Races 2025_14_R, 2025_15_R, 2025_16_R.

## Integrity and provenance limitations

Export records exclusions without deleting evidence and fails on unavailable
schedules or suppressed cache misses. The verifier compares file inventories,
sizes and SHA-256, rejects unsafe paths, symlinks and duplicate payloads, checks raw
schemas and recomputes coverage and exclusions. Legacy cache sources have
`acquired_at_utc` null with an explanatory note (T2c); build time never
substitutes. Formal human acceptance of data-v1 is not recorded in the sources
this update read.

Labels (D2): unresolved visits are omitted with reasons and all v1 confirmation
mappings are empty. The model comparison is therefore on resolvable visits.

## Predictive evaluation controls

Chronological split by season and round before exclusions, no refit on train+val,
train-only preprocessing. Selection and early stopping on val AP, threshold
maximizing val F1 (larger threshold on ties), frozen for test. Event windows k=1
(main), k=2 (auxiliary). Race-level bootstrap, B=1000, paired for comparisons;
a CI containing 0 is a tie. AP or F1 above 0.6 raises the leak alarm. The test
split was evaluated once (approval in `reports/test-v1-freeze.json`). The 363-test
figure is the CI result of the T12b merge commit 1c76890 (GitHub Actions run
45828093141, success).

## Evidence and document reproduction

`evidence-map.md` lists claims with selectors; `evidence/provenance.json` and
`evidence/summary.json` list hashes and aggregates. Use the README for the build.
No local PDF was built for the update through T12b; the first full build is
Overleaf's. After compiling, inspect every page and record the page count in
`report-state.json`.
