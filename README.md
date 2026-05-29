# From Viewing to Doing — Replication Code

Replication code for:

> Harket, D. (2026). *From Viewing to Doing? A Randomised Evaluation of
> Avatar-Led Onboarding in an AI Video Platform.* MSc thesis, Social Data
> Science, University of Copenhagen.

This repository contains the analysis code that produces the tables, figures, and
summary statistics reported in the thesis. It is the **code only**: the
underlying telemetry is pseudonymised personal data under the GDPR and is not
distributed here (see [Data and GDPR](#data-and-gdpr)).

A small, fully synthetic sample is bundled so reviewers can run the entire
pipeline end-to-end without access to the real data.

## What the study did

A live, randomised field experiment in the onboarding flow of an AI video
platform. New users were allocated server-side to one of two conditions:

- **Avatar** — onboarding video presented by a visible synthetic presenter.
- **Control / matched non-avatar** — the *same* onboarding video with the avatar
  removed: identical script, audio, screen demonstration, runtime, call to
  action, and destination.

First-party telemetry measured two outcome layers:

1. **Observed playback progression** — watch-depth thresholds and maximum
   observed playback position.
2. **Instructed task completion** — primarily pressing **Create video**, with a
   consolidated task-entry trigger retained as a lower-intent routing check.

Generated-video render completion is reported only as a downstream boundary check
because it depends on payment/access state, rendering conditions, and follow-up
time beyond the onboarding intervention itself.

The headline result is suggestive but inconclusive. Overall watch depth was
broadly similar across conditions. The avatar condition showed a higher
full-video completion rate and a descriptively higher Create-video press rate,
but neither hypothesis was statistically supported and the design is underpowered
for modest behavioural effects.

### Study snapshot (real data)

| Item | Value |
| --- | ---: |
| Consent-valid analysed episodes | 164 first eligible onboarding episodes |
| Avatar condition | 80 episodes |
| Matched non-avatar condition | 84 episodes |
| Assignment window | 2026-05-06 to 2026-05-19 |
| Design | Server-side hash-randomised intention-to-treat field experiment |
| Primary instructed-task endpoint | Create-video press |
| Lower-intent task check | Consolidated task-entry trigger |
| Full-video completion | 20.0% avatar vs. 13.1% matched non-avatar |
| Create-video press | 31.2% avatar vs. 23.8% matched non-avatar |
| Create-video press difference | +7.4 percentage points |
| 95% CI for Create-video press difference | −6.2 to 21.1 percentage points |
| Approx. 80%-power MDE for Create-video press | ~18.9 percentage points |

These numbers are reproduced by the code in this repository when run against the
real export. They are quoted here only for orientation. The bundled synthetic
sample is random, non-personal, and intended only to demonstrate that the
pipeline runs end-to-end.

## Repository layout

```
analysis/run_analysis.py            # the full analysis pipeline (one file)
data/README.md                      # how to supply real data; GDPR rationale
data/synthetic_sample/              # random, non-personal sample + its generator
docs/data_dictionary.md            # schema of the five required CSV exports
requirements.txt
LICENSE                             # MIT
```

Outputs are written to `outputs/` (git-ignored): LaTeX tables, PNG figures,
`model_estimates.csv`, `results_summary.md`, `current_results.json`, and a rebuilt
SQLite database.

## Quick start

```bash
git clone https://github.com/davidharket/from-viewing-to-doing.git
cd from-viewing-to-doing
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt

# Run the whole pipeline on the bundled synthetic sample (no LaTeX needed):
python analysis/run_analysis.py --input-dir data/synthetic_sample --skip-figures
```

This prints a results summary and writes tables and statistics to `outputs/`.
**The synthetic numbers are meaningless** — they only demonstrate that the code
runs. To reproduce the thesis results, supply the real data (below).

### Reproducing the thesis results

1. Obtain the five CSV exports (see [Data and GDPR](#data-and-gdpr)) and place
   them in `data/`.
2. Run:

   ```bash
   python analysis/run_analysis.py            # add --skip-figures to skip PNGs
   ```

3. The generated `outputs/tables/*.tex` and `outputs/model_estimates.csv`
   correspond to the tables in the thesis.

### Figures (optional)

By default the script also renders PNG figures, which requires a system LaTeX
toolchain providing `pdflatex` and `pdftoppm` (e.g. TeX Live + Poppler). If you
do not have these, pass `--skip-figures`; all tables and statistics are still
produced.

## Data and GDPR

The real dataset is **not** included and will not be published. It consists of
first-party behavioural telemetry from a live product. Although direct
identifiers are excluded from the thesis export, the event-level records remain
pseudonymised personal data under the GDPR.

The thesis analysis uses a smaller consent-valid first-party export. A larger
operational export was excluded because explicit analytics-consent status could
not be verified for a substantial portion of records and because the same export
contained internal testing and bot traffic.

This repository therefore publishes the analysis code, schema documentation, and
a fully synthetic sample with the same expected structure, but not the real
event-level telemetry. See [`data/README.md`](data/README.md) for the required
filenames and [`docs/data_dictionary.md`](docs/data_dictionary.md) for the full
schema (column names and types only). Access to the underlying data for
verification can be requested from the author under an appropriate
controlled-access agreement.

## Environment

Tested with Python 3.9.6, pandas 2.3.3, numpy 2.0.2, scipy 1.13.1. See
`requirements.txt` for version ranges.

## How to cite

If you use this code, please cite the thesis and this repository:

> Harket, D. (2026). *Replication code for "From Viewing to Doing? A Randomised
> Evaluation of Avatar-Led Onboarding in an AI Video Platform"* (Version
> v1.0-thesis-submission) [Computer software]. GitHub.
> https://github.com/davidharket/from-viewing-to-doing

See [`CITATION.cff`](CITATION.cff) for machine-readable metadata.

## License

MIT — see [`LICENSE`](LICENSE).
