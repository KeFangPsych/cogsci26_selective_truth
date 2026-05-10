# Selective Truth — CogSci 2026

Code, experiments, and anonymized data for the CogSci 2026 paper on
**selective-truth communication** under the Rational Speech Act (RSA)
framework. The project covers three pieces:

- a simulation pipeline (`model/`) that runs literal, informative,
  persuade-up, and persuade-down speakers against credulous and vigilant
  listeners over the same world,
- two web-based jsPsych experiments (`experiment/`) that elicit human
  speaker and listener behavior in the same world, and
- the anonymized participant data plus the model-fitting notebooks that
  reproduce the paper's analyses (`data/`).

The shared "world" used by all three is **n=1 generator, m=5 patients,
8-utterance quantifier × predicate vocabulary**.

## Layout

```
cogsci26_selective_truth/
├── model/                    # RSA simulation pipeline
│   ├── rsa_core.py             # Worlds, speakers, listeners (the model library)
│   ├── world/, speakers/, listeners/   # pipeline stages 1–3
│   ├── simulation_experiments/n1m5_T60_obs5000_seed42/
│   │     run.py, io.py, analyze.ipynb  # paper figures
│   └── notebooks/inspect_small_example.ipynb
├── experiment/               # jsPsych experiments deployable on Prolific
│   ├── cogsci26_rsa_speaker_experiment_n5/
│   └── cogsci26_rsa_listener_experiment_n5/
├── data/                     # Anonymized data + model fits
│   ├── cogsci26_rsa_speaker_experiment_n5/
│   └── cogsci26_rsa_listener_experiment_n5/
└── requirements.txt
```

Each of the six subdirectories has its own README with full details on
inputs, run order, and outputs.

## Setup

```bash
git clone https://github.com/KeFangPsych/cogsci26_selective_truth.git
cd cogsci26_selective_truth
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
```

Tested with Python 3.11+.

## Reproducing the paper

### Simulation pipeline (`model/`)

Run the simulation experiment from the repo root. The driver writes its
output to `raw_do_not_track/` next to itself, which is gitignored:

```bash
python model/simulation_experiments/n1m5_T60_obs5000_seed42/run.py
```

Then open
[`model/simulation_experiments/n1m5_T60_obs5000_seed42/analyze.ipynb`](model/simulation_experiments/n1m5_T60_obs5000_seed42/analyze.ipynb)
and run all cells. The notebook saves the simulation figures to
`model/simulation_experiments/n1m5_T60_obs5000_seed42/figures/`.

For a quick walk-through of the model on a small example, see
[`model/notebooks/inspect_small_example.ipynb`](model/notebooks/inspect_small_example.ipynb).

### Behavioral analyses (`data/`)

The fitted artifacts ship pre-built, so reviewers can reproduce all
analyses without re-running data processing:

- Speaker model fits — [`data/cogsci26_rsa_speaker_experiment_n5/model_fitting.ipynb`](data/cogsci26_rsa_speaker_experiment_n5/model_fitting.ipynb)
- Listener effectiveness fits — [`data/cogsci26_rsa_listener_experiment_n5/fit_effectiveness.ipynb`](data/cogsci26_rsa_listener_experiment_n5/fit_effectiveness.ipynb)
- Listener speaker-type fits (Table 1) — [`data/cogsci26_rsa_listener_experiment_n5/fit_speaker_type.ipynb`](data/cogsci26_rsa_listener_experiment_n5/fit_speaker_type.ipynb)

To re-run from raw participant CSVs, see the corresponding subdir
README.

### Running the experiments (`experiment/`)

Both experiments ship with **credentials redacted** so a fresh clone
cannot write to anyone's data store. To deploy, fill in your DataPipe
experiment ID and Prolific completion codes following the setup section
in each experiment's README, then host as a static site.

For local testing without Prolific or DataPipe:

```bash
cd experiment/cogsci26_rsa_speaker_experiment_n5   # or _listener_
python -m http.server 8000
# open http://localhost:8000
```

## Data and PII

All committed CSVs are anonymized — `participant_id` (`P001`, …) is the
only identifier, with no link back to Prolific. Raw per-subject CSVs and
any intermediate files containing identifiers (Prolific PID, subject_id,
session_id, timestamps) live exclusively under `raw_do_not_track/`,
which is `.gitignored` project-wide.

## Citation

If you use this code or data, please cite the accompanying CogSci 2026
paper (citation TBD).

## License

To be determined — likely CC BY 4.0 for the data and MIT for the code.
