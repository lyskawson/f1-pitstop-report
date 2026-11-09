# Technical companion to the report

Application scope remains frozen at
`5ba376f904b58caf2072ce807cc822db7a90ccc6`.
This companion collects implementation details removed from the academic PDF
during the editorial consolidation. It is not a new application contract.
Authoritative contracts and the application runbook remain in that frozen
application revision; producing/verifying revisions are in `evidence-map.md`.

## Environment and entry points

Python 3.12; CI uv 0.11.7. The lock records FastF1 3.9.0a0, pandas 2.3.3,
NumPy 2.5.4, PyArrow 26.0.0, PyYAML 6.0.3, scikit-learn 1.9.1,
PyTorch 2.14.1 (also a 2.14.1+cpu platform variant), LightGBM 4.7.0,
pytest 9.1.1, pytest-socket 0.8.1 and Ruff 0.17.1. Model dependencies and
configurations are not evidence of model implementation or training.
Streamlit is planned and not installed as an application component yet.

For the application checkout, not the report repository:

```sh
uv sync --locked --extra train
uv run --offline ruff check .
uv run --offline ruff format --check .
uv run --offline pytest
uv run --offline pitstop --help
```

These are reproduction instructions, not commands executed for the editorial
revision. Environment installation may need network access; tests block Internet
sockets and allow local Unix sockets. Current CI has no training smoke test.

The implemented CLI paths are cache-only export and snapshot verification:

```sh
uv run --offline --no-sync pitstop download --config configs/data.yaml --from-cache --snapshot v1
uv run --offline --no-sync pitstop verify-data --manifest data/raw/v1/manifest.json
```

Do not rerun export over the immutable candidate. A build needs the existing,
human-populated cache; verification only needs the unpacked snapshot. Verify
the manifest inside the version directory, not its separate canonical copy.
Verification exits 0 for valid complete input, 2 for internally consistent but
incomplete coverage, and 1 for malformed/corrupt input. Bare commands and later
pipeline commands remain stubs with exit 1.

## Formats and clocks

The raw key is `(race_id, DriverNumber, LapNumber)`; the downstream key is
`(race_id, driver_number, lap_number)`. Race IDs use `YYYY_RR_R`, driver
numbers are nonempty strings, lap numbers positive int64, and raw times float64
seconds. Nullable measurements remain null/NaN, never infinity or textual
placeholders. The existing contract defines exact types and feature order.

| Snapshot file | Meaning |
| --- | --- |
| `laps.parquet` | Raw laps; elapsed-session times, including pit markers. |
| `events.json` | Race metadata and last non-null announced planned lap count. |
| `track_status.parquet` | Timestamped status stream in session seconds. |
| `race_control.parquet` | Messages with ISO UTC wall time; requires a time-basis conversion before comparison with elapsed seconds. |
| `lap_count_evidence.json` | Archival announcements in source order, including null updates; not an approved downstream consumer contract. |
| `manifest.json` | Version, producer, source records, hashes, coverage, exclusions and errors. |

The future sample table aligns stable `row_id` values with both NumPy archives.
Tabular X is float32 `[N, 15]`; sequence X is float32 `[N, 15, 16]`, with the
last channel `is_pad`. Padding is contiguous on the left, padded features are
zero, and the final step is real. Existing validators cannot recover lap
provenance/causality from an array alone.

## Integrity and provenance limitations

Export normalizes raw values, records exclusions without deleting evidence,
and fails on unavailable schedules/races or suppressed cache misses. It stages,
verifies, packs, extracts and verifies again before local publication. Existing
destinations are immutable. A publication lock serializes the final step.

Verification compares declared and actual file inventories, sizes and SHA-256;
rejects unsafe paths, symlinks, duplicate/undeclared/missing payloads; checks raw
schemas, events and stream content; and recomputes coverage/exclusions.
Every declared stream must have a payload declaration. Essential Status/Message
values must be text or null, with at least one usable nonblank value per race.
Partial nulls remain allowed; undeclared legacy optional streams are not made
universally mandatory by the correction.

Source acquisition times for the legacy cache are explicitly unknown/null with
notes. Build time must not substitute for acquisition time. The approved scoped
legacy allowance still needs dedicated contract work (T2c), followed by separate
human snapshot acceptance. D10's approved FastF1 3.9 pin differs from stale 3.8.x
manifest prose in the protected contract. This report does not resolve it.
D2 (tyre-change labels), D5 (lap-count consumer/pre-first-announcement policy),
exact/mixed SC wording and uncertain tyre histories remain open.

## Predictive evaluation controls

The fixed chronological split, no train+val refit and train-only preprocessing
are the agreed protocol, not a built modelling dataset. Selection uses val AP;
threshold maximizes val F1, with the larger threshold on ties. The test split
stays locked until explicit human approval after configurations are frozen.
AP or F1 above 0.6 on val/test requires a leakage audit before publication.
Event windows are asymmetric (k=1 main, k=2 auxiliary); uncertainty uses
race-level bootstrap, with paired resamples for comparisons. Exact definitions
remain in the application decisions/contracts.

## Evidence and document reproduction

`evidence-map.md` preserves exact selectors, producing revisions, CI links,
secondary counts and artifact identities. `evidence/provenance.json` preserves
the original evidence hashes; `evidence/summary.json` retains aggregate findings
removed from the PDF. No private logs or raw datasets are bundled here.

Use the report README for its pdfLaTeX/BibTeX build and Overleaf setup.
The pre-edit PDF/source ZIP is retained locally under ignored
`build/archive/pre-edit1/`; the previous public sources remain in Git history
at `66efab4`. Future models and experiments should replace the brief plan in
chapters 3–4, with roughly half the eventual body reserved for methods,
experiments, results and interpretation. The initial 18–22-page planning envelope
can change with the material; neither it nor the current 9–11-page target is a
course requirement.
