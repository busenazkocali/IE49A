# IE49A — Iterated Prisoner's Dilemma

A self-contained Jupyter notebook exploring how simple interaction rules can
sustain cooperation in the Iterated Prisoner's Dilemma (IPD).

Prepared for **IE49A Collective Computation, Fall 2026**.

## Project files

| File | Purpose |
|---|---|
| `IPD_Analysis.ipynb` | Complete simulation, analysis, saved outputs and interactive dashboard |
| `requirements.txt` | Python dependencies |
| `.gitignore` | Excludes environments and temporary files |
| Written report PDF | Separate 1–2 page report, added when completed |
| `notebook_results/`  | Exported figures, tables and interactive results |


## Setup

Python **3.11 or newer** is required; **Python 3.12** is recommended.
Run the following commands from the repository directory.

If you have not created the Conda environment yet:

```bash
conda create -n ie49a python=3.12 -y
```

Activate the environment and install the dependencies:

```bash
conda activate ie49a
python -m pip install -r requirements.txt
```

The requirements file should include:

```text
numpy==2.3.5
matplotlib==3.10.8
jupyterlab>=4,<5
ipykernel>=6,<8
ipywidgets>=8,<9
```

## Run in VS Code

1. Install Microsoft's **Python** and **Jupyter** extensions in VS Code.
2. Open the repository folder and `IPD_Analysis.ipynb`.
3. Choose **Select Kernel → Python Environments → ie49a**.
4. Restart the kernel and select **Run All** to reproduce the analysis.
5. Save the notebook after execution so its tables and figures are visible on GitHub.

If the environment is missing from the kernel list, run:

```bash
conda activate ie49a
python -m ipykernel install --user --name ie49a --display-name "Python (ie49a)"
```

Then reload VS Code and select the registered kernel. VS Code runs the notebook
directly; a separate browser-based JupyterLab server is not required.

Alternatively, open it in JupyterLab with:

```bash
python -m jupyter lab
```

## Model

Players choose simultaneously between cooperation (C) and defection (D).
The default payoff matrix gives the following rewards, written as
**(row player, column player)**:

| Row action / column action | Cooperate | Defect |
|---|---|---|
| Cooperate | (3, 3) | (0, 5) |
| Defect | (5, 0) | (1, 1) |

Payoffs are configurable and must satisfy **T > R > P > S** and
**2R > T + S**. The notebook uses a configurable fixed horizon, which satisfies
the assignment's option of either fixed rounds or a continuation probability.

### Strategies

| Strategy | Rule |
|---|---|
| ALL-C | Always cooperate. |
| ALL-D | Always defect. |
| TFT | Cooperate first, then copy the opponent's previous executed action. |
| GRIM | Cooperate until the opponent defects once, then defect forever. |
| GTFT | Cooperate first and after cooperation; after defection, cooperate with the configured forgiveness probability. |

Both players decide before current actions are recorded. Independent action
noise can flip either intended move. Players observe executed actions accurately,
and histories reset between matches.

Round-robin tournaments include all ten distinct unordered pairs of the five
strategies, excluding self-play. Explicit pairwise experiments also support
self-pairs. Tournament payoffs are averaged across each strategy's four opponents.

## Experiments and analysis

The notebook includes:

- Known-answer checks of payoff calculations and strategy behavior.
- Pairwise comparisons of cooperation, exploitation and retaliation.
- Round-robin tournaments with no noise and with action noise.
- Per-round system cooperation, per-round payoffs and cumulative payoffs.
- Pairwise payoff heatmaps and numerical summaries.
- Noisy TFT–TFT versus GTFT–GTFT comparisons.
- Sensitivity analyses for noise, forgiveness and number of rounds.
- Explanations, conclusions, limitations and suggestions for figures to include in the written report.

The default main experiments use **200 rounds**, **30 independent repetitions**,
**seed 42**, **5% action noise** in the noisy scenario and **10% forgiveness**
for GTFT. Change the configuration in Section 2 and rerun all cells to evaluate
other settings. Separate sensitivity experiments use the parameter grids shown
in their own cells.

SD and SE summarize variability across independent repetition-level means.
Displays of mean ±2 SE are approximate uncertainty summaries, not formal tests
of differences between strategy rankings.

## Interactive dashboard

Section 11 implements the optional **interactive UI extension** using
`ipywidgets`. Run the preceding cells and the dashboard cell, then:

1. Select **Round-robin tournament** or **Pairwise game**.
2. Adjust strategies, rounds, repetitions, noise, forgiveness, payoffs and seed.
3. Click **Run simulation** to update the figures and summary table.

Strategy selectors apply only to pairwise mode; round robin uses all five
strategies. Moving a control does not run an experiment until the button is
clicked. The panel records the settings used for each completed run and reports
invalid payoff matrices.

The dashboard requires a live notebook kernel in VS Code or JupyterLab.
**GitHub can display saved figures but cannot run the controls.** If widgets
are missing, confirm that `ipywidgets` is installed in the selected kernel's
environment and restart the kernel.

## Generated outputs

Running the notebook creates `notebook_results/` in the current working
directory. It exports ten main PNG figures and CSV tables, including round-level
cooperation, per-round and cumulative payoffs, and pairwise payoff matrices.
These files can be used in the separately written report.

Dashboard runs export the following under `notebook_results/interactive/`:

- `interactive_overview.png`
- `interactive_per_round.csv`
- `interactive_pairwise.csv`
- `interactive_summary.csv`
- `interactive_config.json`

Each successful dashboard run replaces those interactive output files. Main
analysis outputs are regenerated when their corresponding cells are rerun.
Generated files are not required as inputs to the notebook.

## Interpretation and scope

The model shows when reciprocal rules sustain cooperation and how mistakes and
forgiveness affect outcomes. Strategy performance depends on the payoff matrix,
opponent composition, noise and horizon. No strategy is assumed to win every
experiment.

Strategies and encounter frequencies remain fixed: the model does not simulate
learning, evolutionary selection, spatial interaction or reputation. Cooperation
need not increase over time, and a rising cumulative payoff alone is not evidence
of increasing cooperation.

The implemented optional extensions are **action noise with forgiving strategies**
and the **interactive UI**. Parameter sensitivity analyses provide additional
comparisons. The assignment's separate **1–2 page written report** is prepared
and uploaded by the student; the notebook does not generate a report.



All numerical results and figures are generated by the notebook; no external
dataset is required.
