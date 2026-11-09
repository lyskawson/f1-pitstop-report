# F1 pit stop prediction: academic report

Polish living report for the Neural Networks course, based on accepted
application source `5ba376f904b58caf2072ce807cc822db7a90ccc6` (2026-11-09).
The report describes the implemented data infrastructure and FastF1 audit;
models, training and predictive evaluation have not been implemented.

This is an independent source repository. The application is
[lyskawson/f1-pitstop](https://github.com/lyskawson/f1-pitstop).
The report repository is
[lyskawson/f1-pitstop-report](https://github.com/lyskawson/f1-pitstop-report).
The PDF and ZIP are generated deliverables, excluded from source control.

## Build

Use a standard TeX Live installation with **pdfLaTeX and BibTeX**. From this
directory:

```sh
latexmk -pdf -interaction=nonstopmode -halt-on-error -outdir=build main.tex
```

Output: `build/main.pdf`. No shell escape, downloaded assets, external fonts,
application checkout, cache or datasets are needed. Without latexmk, run
`pdflatex -output-directory=build main.tex`, then
`bibtex build/main`, then pdfLaTeX twice more. Create `build/` first.
For a source archive after committing:

```sh
git archive --format=zip --output=build/f1-pitstop-report-source.zip HEAD
```

`main.tex` preserves the supplied report class and title page (authors, student
numbers, supervisor, course, PWr logo and application URL). `chapters/` contains
the narrative; `bibliography.bib` is the sole bibliography. New standalone
figures belong in `figures/`. The current data-flow figure is native LaTeX.
The preamble retains Polish support, serif typography, chapter numbering,
A4/25 mm margins and restrained headers. Unused minted/svg, duplicate packages
and decorative listing/table styles were removed. No license is asserted for
the supplied template or university logo.

## Overleaf: source import

Upload `build/f1-pitstop-report-source.zip` as a new project, or upload its source
files. Select **main.tex** as Main document and **pdfLaTeX** as Compiler.
Compile with the normal Overleaf build. The source project is self-contained;
the ZIP must not include `.git/`, build output, private notes or application data.
Actual compilation in the Overleaf service has not been tested in this session.

## Overleaf: optional synchronization

Both direct Git and GitHub synchronization require premium access; institutional
access may qualify. No Overleaf account or Git endpoint has been connected.
Use one synchronization route and coordinate edits before transferring changes.

**GitHub route:** the project owner links the appropriate GitHub account in
Overleaf, then chooses New project → GitHub repo and imports this report
repository. Overleaf cannot connect an existing Overleaf project to an existing
GitHub repository. Transfers are manually triggered under Integrations → GitHub:
pull GitHub changes before editing and push Overleaf edits when ready. See the
[official GitHub synchronization guide](https://docs.overleaf.com/integrations-and-add-ons/git-integration-and-github-synchronization/github-synchronization).

**Direct Git route:** obtain the actual project's Git URL from Integrations → Git.
Authenticate using Overleaf's supported credential flow; never put a token in
the remote URL, README, logs or repository. For the initial connection, clone
that endpoint separately, inspect its default branch with
`git ls-remote --symref <PROJECT_GIT_URL> HEAD`, and compare its files with this
report. Integrate both histories/content before pushing: the existing template
may have independent history and human edits. Do not blindly overwrite it or
force-push. Only after that reconciliation, the report repository may use an
`overleaf` remote and push to the **observed** remote branch. New projects use
`main`; older ones may use `master`. See the
[official Git integration guide](https://docs.overleaf.com/integrations-and-add-ons/git-integration-and-github-synchronization/git-integration).

Keep the public report repository as the GitHub side, never the application
repository. Before later publication fetch report-remote changes, merge
colleagues' edits, resolve conflicts, build and inspect the PDF, then push
normally. Neither integration is an automatic background updater of this report.

## Evidence and maintenance

- `runbook.md`: technical details, exact versions, CLI behavior and operational
  controls kept outside the academic narrative.
- `evidence-map.md`: claims, selectors, revisions, limitations and course checklist.
- `evidence/provenance.json`: SHA-256 identities of consulted source/evidence
  artifacts and the template, with no private machine paths.
- `evidence/summary.json`: selected existing aggregate findings, no raw data/logs.
- `report-state.json`: frozen application revision, covered stages, unresolved
  questions and exact build/visual-check status.

The current PDF uses five chapters: goal/scope, data/preparation,
methods/implementation, experiments/results, and discussion/conclusions.
Label uncertainty and causal availability have one main home in chapter 2;
the compact completion summary is in chapter 5. Existing software checks and
a short future experiment plan share chapter 4. Replace plans with realized
methods/results as work lands rather than keeping both accounts.

Write the PDF as a continuous account of the project that grows with completed
work. Do not introduce it with a code SHA, update date or snapshot description,
and do not frame status captions around a source revision. Keep exact code
revisions, update records and editorial notes in this README, the evidence map
and report-state.json. Preserve scientific limitations and identify experimental
inputs where needed for interpretation.

The first 15-page version is retained in Git history at `66efab4`, with a local
PDF/source backup in ignored `build/archive/pre-edit1/`. The current editorial
target is 9–11 pages without changing fonts or margins. For the final report,
18–22 pages is only a planning envelope: reserve roughly half the body for
models, experimental method, results and interpretation, and adjust when actual
content warrants it. All course requirements take precedence over page counts.

On a later request to update the report, resolve accepted local application
`main` once and read files at that exact SHA (read-only `git show` or an archive).
Compare against `report-state.json`, inspect changed implementation/tests and
existing evidence, then rewrite the affected narrative and conclusions. Preserve
human edits and integrate report-remote changes. Never train or evaluate the
locked test split merely to fill the report. Update provenance/state, rebuild,
inspect **every page**, commit and push only this repository. A new source ZIP
is template input: compare it and preserve existing narrative/human edits.

No automatic monitoring or active Overleaf sync is claimed. No raw race data,
private evidence logs, cache, tokens, models or additional personal metadata
belong here. Title-page metadata is the explicitly supplied exception.

## Template provenance and validation

Base: the five-file Overleaf project supplied by Alek, extracted as
`Sieci_neuronowe_projekt`, inspected on 2026-11-09. No newer ZIP was supplied
during this update. Original input hashes are in `evidence/provenance.json`;
the original copy was left unchanged. Only the necessary logo was copied as a
binary asset, without foreign Git metadata.

Local compilation, portability and page inspection are recorded in
`report-state.json`. The source PDF is intentionally a report on work in
progress: D2, D5, T2c, data acceptance and predictive results remain open.
