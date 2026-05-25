# STELLAR PAPER — DEADLINE-EXTENSION RUNBOOK FOR JASUR
## One file: what the paper is, what to run this week, what to skip, what the new code does, and all new code inline

This is the file to show Jasur on Zoom.

It is written in easy language on purpose.

This file tells Jasur:

- what the current paper is really about;
- which code already exists in the repo;
- which new code has already been prepared;
- which tasks matter **this week**;
- which tasks should **not** eat the week;
- what the time-window issue really means;
- how to run the stronger analysis step by step;
- and it includes **all newly added code inline**.

**Important boundary:**  
The repo already contains the current submitted-paper helper chain.  
This file inlines **all newly added code** that sits on top of that existing repo code.

---

# THIS WEEK ONLY — JASUR EXECUTION BLOCK

This is the only part Jasur needs for the sprint.

## Do NOT run this week
- old internal embedding diagnostics
- old masking pipeline
- old masked-only evaluation
- `16_run_window_sensitivity.py`
- any new-window experiments
- any major baseline retuning

## What to run this week

### 1. Baseline rerun
Run from:

`Community Detection/SSLPA/manual/normalized/ncsf/helpers/`

Use the wrapper first:

```bash
python run_submitted_holdout_pipeline.py
```

If that fails, run the explicit chain:

```bash
python 3_prepare_fixed_test_split.py
python 4_prepare_dev_repeats.py
python 5_run_sslpa_dev_repeats.py
python 6_evaluate_dev_repeats_binary.py
python 7_evaluate_dev_repeats_multiclass.py
python 8_choose_final_thresholds.py
python 9_run_final_dev_sslpa_and_test.py
python 10_report_flagged_risky_addresses.py
```

Optional debug only:

```bash
python 11_compute_risky_overlap.py
```

### 2. Fast high-value outputs

```bash
python 12_plot_binary_tradeoff.py
python 13_summarize_threshold_stability.py
python 17_run_filter_ablation.py
python 19_build_review_artifact_pack.py
```

### 3. Binary outer robustness

Start with 3:

```bash
python 14_run_outer_split_sensitivity.py --n-outer-splits 3
python 15_aggregate_outer_split_results.py
```

If runtime is okay, go to 5:

```bash
python 14_run_outer_split_sensitivity.py --n-outer-splits 5
python 15_aggregate_outer_split_results.py
```

### 4. Optional small risk-score extension
Only if everything above is clean and there is still time:

```bash
python 20_compute_continuous_risk_scores.py
python 21_evaluate_continuous_risk_ranking.py
python 22_plot_pr_curve_risk_score.py
```

## Very short meaning of each new thing
- Binary tradeoff figure = shows why lower coverage is a deliberate screening tradeoff
- Threshold stability = shows thresholds are not random nonsense
- Ablation = shows NCRF is more than one trivial threshold
- Outer robustness = strongest answer to “one lucky split?”
- Risk score = optional ranking add-on, not a new paper identity

## Time-window work this week
Do NOT run new windows.
Only strengthen the writing and, if easy, compute cheap descriptive checks from the existing 48-day data.


# 0. Deadline reality

BCCA 2026 currently shows:

- paper deadline: **May 31, 2026 (extended)**
- acceptance notification: **June 30, 2026**
- camera-ready: **July 30, 2026**

This means there is a short but real chance to strengthen the current submission.

The correct mindset is:

- do not rewrite the whole paper,
- do not invent a new model,
- do not waste time on secondary things,
- add the strongest realistic upgrades to the current paper identity.

---

# 1. What the current paper is about

## Final title
**A Neighborhood-Constrained Risk Filter for Address Screening on Stellar**

## Final method name
**NCRF = Neighborhood-Constrained Risk Filter**

## Plain-English paper identity

The paper says:

1. **Raw SSLPA** spreads labels widely through the transaction graph.
2. That gives broad coverage.
3. But some of those propagated labels are weak or noisy.
4. **NCRF** is a conservative filter on top of SSLPA.
5. NCRF keeps only those propagated labels that have enough local graph support.
6. So the result is a **smaller but cleaner shortlist**.

## What the paper is NOT saying

This paper is **not** saying:
- “we labelled the full network perfectly”
- “we built a calibrated risk-probability model”
- “we invented a completely new propagation architecture”

It is saying:
- **raw SSLPA gives breadth**
- **NCRF gives a smaller, higher-confidence risky subset**

## One sentence that must stay alive

**SSLPA proposes. NCRF decides which proposals are trustworthy enough to keep.**

Do not lose this sentence.

---

# 2. The non-negotiable rule

Whenever NCRF results are discussed:

- the gains are gains on the **covered subset**
- **coverage must always be reported separately**

Plain English:

If the filter keeps only a small fraction of nodes, and those kept nodes look clean, that is fine — but we must also say clearly how many nodes were kept.

We are not allowed to hide that.

---

# 3. Which pipeline matters now?

There are really two pipelines in the repo.

## Old pipeline: the older 7-step NCSF pipeline
This is the older structure with:
1. label stats / filtered labels
2. entity evaluation
3. binary scam evaluation
4. internal embedding metrics
5. plotting
6. masking / semi-supervised splits
7. masked-only evaluation

This pipeline is useful as project history and for some older artifacts.

But it is **not** the main pipeline for the current paper anymore.

## Current pipeline: the holdout helper pipeline
This is the pipeline in:

`Community Detection/SSLPA/manual/normalized/ncsf/helpers/`

This is the one that matches the current paper:
- fixed blind test
- repeated development splits
- raw SSLPA vs SSLPA+NCRF
- final blind test
- risky overlap summary

## Correct rule
For this week:

- **use the current holdout helper pipeline as the main pipeline**
- only use older pieces if they give something cheap and still useful

---

# 4. What can be skipped this week

To save time, Jasur should **deprioritize or skip**:

- old internal embedding diagnostics
- old masking pipeline
- old masked-only evaluation

Why?

Because in the current paper these are already secondary, not the main proof.

If space is needed in the paper, these are the first places to cut or compress.

---

# 5. What code already exists in the repo

The current submitted-paper helper chain already exists in the repo.

Important existing files:
- `common_holdout.py`
- `adapters.py`
- `3_prepare_fixed_test_split.py`
- `4_prepare_dev_repeats.py`
- `5_run_sslpa_dev_repeats.py`
- `6_evaluate_dev_repeats_binary.py`
- `7_evaluate_dev_repeats_multiclass.py`
- `8_choose_final_thresholds.py`
- `9_run_final_dev_sslpa_and_test.py`
- `10_report_flagged_risky_addresses.py`
- `11_compute_risky_overlap.py`

This means:

**the submitted-paper logic is already implemented.**

That is good news.

---

# 6. What new code is already prepared

These new files have already been written and are included inline below:

- `review_config.py`
- `holdout_experiment_lib.py`
- `run_submitted_holdout_pipeline.py`
- `12_plot_binary_tradeoff.py`
- `13_summarize_threshold_stability.py`
- `14_run_outer_split_sensitivity.py`
- `15_aggregate_outer_split_results.py`
- `window_sensitivity_manifest.example.json`
- `16_run_window_sensitivity.py`
- `17_run_filter_ablation.py`
- `18_build_multiclass_uncertainty_appendix.py`
- `19_build_review_artifact_pack.py`
- `20_compute_continuous_risk_scores.py`
- `21_evaluate_continuous_risk_ranking.py`
- `22_plot_pr_curve_risk_score.py`

## Important truth
All of the new files above are already written.

The only major thing not solved by Python alone is:
- the real project-specific graph/label wiring for **window sensitivity**

That is an input-mapping issue, not a missing-code issue.

---

# 7. The five things that matter this week

## 7.1 Binary operating-tradeoff figure
This is a picture that shows:
- raw SSLPA keeps lots of nodes
- NCRF keeps fewer nodes
- but the kept nodes are cleaner

Why it helps:
- it makes the main paper story visual
- it directly answers “coverage is low”
- it explains why NCRF is useful even though raw SSLPA already looks strong

## 7.2 Threshold-stability summary
This checks whether threshold selection is reasonably stable across repeated development splits.

Why it helps:
- it shows the filter is not just a random lucky setting
- it supports the claim that binary is cleaner and more stable than multiclass

## 7.3 Cheap ablation
This compares:
- degree-only filter
- ratio-only filter
- full NCRF

Why it helps:
- it answers “isn’t this just a trivial threshold?”

## 7.4 Binary repeated outer held-out robustness
This repeats the whole main binary evaluation several times with different blind-test splits.

Plain English:
instead of trusting one exam, the method takes several exams.

Why it helps:
- it is the strongest answer to “one lucky split?”
- it is the strongest deeper-analysis upgrade you can likely add this week

## 7.5 Small continuous risk-score extension
This adds a ranking score on top of the same local-support idea.

Instead of only saying:
- keep
- drop

it also says:
- how strong is the local support for risk?

Why it helps:
- top-k shortlists
- precision-recall curves
- stronger prioritization language

Important:
this is a **small extension**, not a replacement for the main NCRF identity.

---

# 8. What to run this week, in the correct order

## Phase 1 — make sure the current paper still reruns

### Run from:
`Community Detection/SSLPA/manual/normalized/ncsf/helpers/`

### Commands
```bash
python 3_prepare_fixed_test_split.py
python 4_prepare_dev_repeats.py
python 5_run_sslpa_dev_repeats.py
python 6_evaluate_dev_repeats_binary.py
python 7_evaluate_dev_repeats_multiclass.py
python 8_choose_final_thresholds.py
python 9_run_final_dev_sslpa_and_test.py
python 10_report_flagged_risky_addresses.py
```

Optional duplicate/debug:
```bash
python 11_compute_risky_overlap.py
```

### Or use the wrapper if baseline paths already work
```bash
python run_submitted_holdout_pipeline.py
```

### Deliverable
Create:
- `docs/submitted_pipeline_reproduction_note.md`

This should say:
- what ran cleanly
- what broke
- what had to be fixed
- whether the baseline still matches the submitted paper logic

---

## Phase 2 — fast high-value outputs

### A. Binary tradeoff
```bash
python 12_plot_binary_tradeoff.py
```

### B. Threshold stability
```bash
python 13_summarize_threshold_stability.py
```

### C. Cheap ablation
```bash
python 17_run_filter_ablation.py
```

### D. Review asset pack
```bash
python 19_build_review_artifact_pack.py
```

These are the fastest useful upgrades.

---

## Phase 3 — deeper robustness

### Start with a pilot
```bash
python 14_run_outer_split_sensitivity.py --n-outer-splits 3
python 15_aggregate_outer_split_results.py
```

### If runtime is okay, go to 5
```bash
python 14_run_outer_split_sensitivity.py --n-outer-splits 5
python 15_aggregate_outer_split_results.py
```

Correct rule:
- do **binary first**
- do **not** let multiclass robustness eat the week before binary is done

---

## Phase 4 — small ranking extension

```bash
python 20_compute_continuous_risk_scores.py
python 21_evaluate_continuous_risk_ranking.py
python 22_plot_pr_curve_risk_score.py
```

Only keep this in the paper if:
- the outputs are clean,
- the story is easy to explain,
- and it does not bloat the paper.

---

# 9. Why outer-split robustness has a runtime warning

This is simple.

The expensive part is not the threshold filtering.
The expensive part is running **SSLPA on the big graph** many times.

One binary outer split roughly means:
- 5 inner repeated SSLPA runs
- plus 1 final outer test run

So one outer split is about 6 full-graph SSLPA runs.

That is why we start with 3.
If runtime looks okay, then push to 5.

Three clean outer blind-test repeats are already far better than zero.

---

# 10. Time-window concern: what can still be improved this week without new data

Claudio’s concern was basically:
“tell me why this time window makes sense.”

The good news is:

**we are not stuck.**

We do **not** need a new dataset or full window-sensitivity experiments to make the time-window justification much stronger.

The key idea is:

**stop treating the time-window justification as only one sentence about protocol dates.**
Instead, turn it into a small, grounded argument with several pieces of evidence.

That is realistic this week.

## Strongest framing

The current window, **2024-12-30 to 2025-02-15**, already has one major strength:

- it lies fully inside a **single protocol regime**

That matters because:
- Protocol 22 went live on Mainnet on **2024-12-05**
- Protocol 23 / Whisk arrived only on **2025-09-03**
- Protocol 24 arrived on **2025-10-22**
- Protocol 25 arrived on **2026-01-22**

So the submitted slice does **not** mix regimes.

This is a methodological strength, not an apology.

Better still:
- Protocol 23 introduced meaningful changes such as unified events and state archival
- Protocol 24 was a stability-oriented upgrade after Whisk
- Protocol 25 introduced further network changes

That helps justify why the first paper deliberately stayed inside one regime.

## The basic story we want

> We chose a single-regime post-Protocol-22 slice on purpose, to avoid mixing materially different protocol environments.

That is stronger than just saying “it was after Protocol 22.”

## What can be strengthened this week

### A. Make the protocol argument sharper
Add all of the following ideas:

- protocol upgrades can change network behavior and data-generation conditions
- later protocols introduced new network capabilities and stability changes
- mixing pre- and post-upgrade behavior would have muddied interpretation
- therefore, staying inside one regime was the cleaner first design choice

### B. Point out the post-upgrade buffer
This is a strong point and should be said clearly.

The submitted window starts **25 days after** Protocol 22 went live.

That means:
- the window does **not** start on upgrade day
- it starts after a short settling period
- this reduces the chance that we are measuring immediate upgrade turbulence instead of normal post-upgrade behavior

### C. Add a label-availability justification
If true in the data pipeline, say clearly:

- usable graph-matched trusted labels begin on **2024-12-30**

That means the window was constrained by the intersection of:

1. **label availability**
2. **single-regime coherence**
3. **tractable graph scale**

This is a much stronger justification than sounding arbitrary.

### D. Add a “why 48 days?” sentence
The clean reasons are:

- long enough to produce a large connected graph
- short enough to remain computationally tractable
- short enough to stay inside one stable protocol regime

### E. Add a tiny timeline figure or one-row table
Very simple:

- Protocol 22 date
- chosen window start
- chosen window end
- Protocol 23 date

This visually answers a big part of the concern.

## Stronger optional grounding using the same data

These do **not** require new windows.
These use the existing 48-day data only.

### F. Add an in-window stability check
Using the existing 48-day data, compute simple daily descriptive series such as:

- daily transaction count
- daily active account count
- daily unique edges or payment flows
- daily scam-labeled activity count, if easy
- daily proportion of activity touching labeled nodes, if easy

What this helps show:

- the sample is not a one-day spike pretending to be a study
- the network is active throughout the interval
- the window looks like a meaningful observational slice

### G. Add a concentration check
Compute something simple like:

- share of total transactions contributed by the busiest day
- share of total active nodes contributed by the busiest day
- maybe share contributed by the busiest week

Why this helps:

- if one day dominates the whole sample, the window looks fragile
- if no single day dominates, the window looks more representative

This is a very reviewer-friendly sanity check.

### H. Add a first-half vs second-half descriptive comparison
Split the 48-day window into two 24-day halves and compare **descriptive stats only**, such as:

- transactions
- active nodes
- maybe labeled-node support
- maybe SCAM/NON-SCAM support counts, if easy

The goal is **not** to rerun the model twice.
The goal is to show:

- the window does not look like two totally different worlds glued together

If the halves are broadly comparable, that strengthens the justification.
If they differ somewhat, that is still okay. You can say the window shows normal within-window variation, not a regime break.

### I. Add a clearer scope statement
Add a sentence like:

> This paper does not claim temporal generalization across all Stellar regimes. It studies a single, label-available, post-Protocol-22, pre-Protocol-23 regime and treats broader temporal robustness as future work.

This helps because it makes the claim smaller, cleaner, and more honest.

## What should NOT be claimed this week

Do **not** claim:
- window sensitivity
- robustness across multiple windows
- cross-protocol generalization

unless those experiments were actually run.

This week the correct move is:

- strengthen justification
- add lightweight descriptive grounding
- do not fake new evidence

## What I recommend doing this week

If the goal is to strengthen the time-window justification quickly, do these four things:

### 1. Rewrite the protocol-justification paragraph
Add:
- single-regime rationale
- 25-day post-upgrade buffer
- label-availability rationale
- tractability rationale

### 2. Add one simple figure
Best options:
- daily transaction count
- daily active account count
- maybe both if the figure stays clean

This is probably the best single new grounding figure.

### 3. Add one tiny summary table
Example rows:
- total days
- median daily transactions
- max-day transaction share
- median daily active nodes
- max-day active-node share
- maybe first-half vs second-half totals

This makes the window look characterized, not arbitrary.

### 4. Add one sentence in limitations / future work
Say clearly:
- broader cross-window and cross-regime sensitivity remains future work

That keeps the paper honest.

## The exact logic chain to use in the paper

1. We needed a window where trusted labels were available in usable graph-aligned form.
2. We wanted to avoid mixing protocol regimes.
3. We therefore selected a post-Protocol-22, pre-Protocol-23 interval.
4. We did not start on upgrade day itself, but after a short settling period.
5. The chosen 48-day slice is large enough to produce a meaningful graph, but still computationally tractable.
6. Simple descriptive checks show that the window is not dominated by one isolated burst.
7. Broader temporal sensitivity remains future work.

That is grounded.
That is honest.
That is much stronger than a one-line protocol-date excuse.

## Final message for Jasur

Jasur does **not** need to run new windows this week.

He only needs to help, if possible, with lightweight descriptive summaries from the **existing 48-day data**, such as:
- daily counts
- concentration checks
- first-half vs second-half descriptive comparisons

The main improvement is still in the **writing and framing**, not in pretending we ran a full temporal-robustness study.



# 11. What to cut or compress in the paper to make room

## First to cut or compress
- internal embedding diagnostics
- Figure 2
- most of Table IV discussion

## Second to cut or compress
- masking section
- most masking prose

## Why this is safe
Because these are already secondary in the current draft.

The paper’s real spine is:
- fixed blind test
- raw SSLPA vs SSLPA+NCRF
- conservative screening tradeoff

---

# 12. What this week can realistically achieve

If the week goes well, the paper can become:

- the same honest NCRF screening paper
- but with:
  - a visible tradeoff figure
  - empirical threshold stability
  - a clean ablation
  - repeated outer blind-test robustness on the primary binary task
  - maybe a compact ranking extension

That is a serious upgrade.
It does not guarantee acceptance.
But it makes the paper much harder to dismiss as “too preliminary.”

---

# 13. What remains for later, not this week

Do **not** let these eat the week:

- full observation-window sensitivity
- rebuilding multiple new windows
- major retuning of all baselines
- large new masking work
- giant refactoring for code beauty

The only big thing still missing after this week will likely be:
- true window sensitivity across multiple windows

That is fine.
That is an honest limitation.

---

# 14. What Yasir should do in parallel

## Y1 — review-response bank
Create:
- `docs/review_response_bank.md`

Include short prewritten answers for:
- low coverage
- why not raw SSLPA only
- one lucky split concern
- why this 48-day window
- modest contribution
- class-disjoint split concern
- trivial-threshold objection

## Y2 — paper slot map
Create:
- `docs/paper_slot_map.md`

Say where each new thing would go:
- main paper
- appendix
- rebuttal only
- stronger revision only

## Y3 — two paper tracks
Keep:
- `camera_ready_clean`
- `stronger_revision`

---

# 15. Final instruction to Jasur

Do **not** panic and rewrite the whole project.

Do **not** invent a new architecture.

Do **not** let the continuous risk-score branch replace the current paper identity.

Instead:

1. rerun the current holdout baseline;
2. generate tradeoff, threshold stability, ablation;
3. run binary outer-split robustness;
4. if time and results are clean, add the small risk-score extension;
5. compress or remove masking and internal diagnostics if space is needed.

That is the correct way to go deeper **without breaking the paper**.

---

# 16. All new code, inline

The sections below are the full new code layer.
The old repo helper chain is still assumed to be present in the repo.

## `review_config.py`

```python

from __future__ import annotations

import os
from pathlib import Path

# ---- base paths ---------------------------------------------------------------
BASE_DIR = Path(os.environ.get("STELLAR_PUBLICATION_BASE", "/home/user/jfayzullaev/stellar-clustering/publication"))

GRAPH_PATH = Path(os.environ.get(
    "STELLAR_GRAPH_PATH",
    str(BASE_DIR / "data" / "LCC" / "LCC_G_tx_undirected_weighted.pkl")
))
LABELS_CSV = Path(os.environ.get(
    "STELLAR_LABELS_CSV",
    str(BASE_DIR / "new-labled-data" / "normalization" / "labeled_nodes_in_graph.csv")
))

HELPERS_DIR = Path(os.environ.get("STELLAR_HELPERS_DIR", str(Path(__file__).resolve().parent)))
OUT_ROOT = Path(os.environ.get("STELLAR_OUT_ROOT", str(HELPERS_DIR)))

# ---- split policy -------------------------------------------------------------
TEST_RATIO = float(os.environ.get("STELLAR_TEST_RATIO", "0.20"))
RANDOM_SEED = int(os.environ.get("STELLAR_RANDOM_SEED", "42"))
MIN_CLASS_SIZE = int(os.environ.get("STELLAR_MIN_CLASS_SIZE", "10"))

N_REPEATS = int(os.environ.get("STELLAR_N_REPEATS", "5"))
TRAIN_RATIO_WITHIN_DEV = float(os.environ.get("STELLAR_TRAIN_RATIO_WITHIN_DEV", "0.875"))
VAL_RATIO_WITHIN_DEV = float(os.environ.get("STELLAR_VAL_RATIO_WITHIN_DEV", "0.125"))
RANDOM_SEED_BASE = int(os.environ.get("STELLAR_RANDOM_SEED_BASE", "100"))

OUTER_N_SPLITS = int(os.environ.get("STELLAR_OUTER_N_SPLITS", "20"))
OUTER_RANDOM_SEED_BASE = int(os.environ.get("STELLAR_OUTER_RANDOM_SEED_BASE", "1000"))

# ---- threshold grids ----------------------------------------------------------
D_GRID = [int(x) for x in os.environ.get("STELLAR_D_GRID", "1,2,3,5,10").split(",")]
R_GRID = [float(x) for x in os.environ.get("STELLAR_R_GRID", "0.1,0.2,0.3,0.4,0.5,0.6,0.7,0.8,0.9").split(",")]

# ---- SSLPA settings -----------------------------------------------------------
SSLPA_MAX_ITER = int(os.environ.get("STELLAR_SSLPA_MAX_ITER", "100"))
SSLPA_CONVERGENCE_THRESHOLD = float(os.environ.get("STELLAR_SSLPA_CONVERGENCE_THRESHOLD", "0.001"))

# ---- risk score ---------------------------------------------------------------
RISK_SCORE_BETA_LOWER_Q = float(os.environ.get("STELLAR_RISK_SCORE_BETA_LOWER_Q", "0.10"))
TOP_KS = [int(x) for x in os.environ.get("STELLAR_TOP_KS", "10,25,50,100,193,200,500").split(",")]

# ---- output roots -------------------------------------------------------------
FIXED_TEST_DIR = OUT_ROOT / "step6_holdout" / "fixed_test"
DEV_REPEATS_DIR = OUT_ROOT / "step6_holdout" / "dev_repeats"
PREDICTIONS_DIR = DEV_REPEATS_DIR / "predictions"
DEV_BINARY_DIR = OUT_ROOT / "step7_holdout" / "dev_binary"
DEV_MULTICLASS_DIR = OUT_ROOT / "step7_holdout" / "dev_multiclass"
FINAL_TEST_DIR = OUT_ROOT / "step7_holdout" / "final_test"
REVIEW_ASSETS_DIR = OUT_ROOT / "step8_review_assets"

OUTER_SENSITIVITY_DIR = OUT_ROOT / "outer_sensitivity"
WINDOW_SENSITIVITY_DIR = OUT_ROOT / "window_sensitivity"
RISK_SCORE_DIR = OUT_ROOT / "risk_score"
REVIEW_PACK_DIR = OUT_ROOT / "review_artifact_pack"

UNKNOWN_LABEL = "UNKNOWN"
SCAM_LABEL = "SCAM"

def ensure_dirs() -> None:
    for p in [
        FIXED_TEST_DIR,
        DEV_REPEATS_DIR,
        PREDICTIONS_DIR,
        DEV_BINARY_DIR,
        DEV_MULTICLASS_DIR,
        FINAL_TEST_DIR,
        REVIEW_ASSETS_DIR,
        OUTER_SENSITIVITY_DIR,
        WINDOW_SENSITIVITY_DIR,
        RISK_SCORE_DIR,
        REVIEW_PACK_DIR,
    ]:
        p.mkdir(parents=True, exist_ok=True)

```

## `holdout_experiment_lib.py`

```python

from __future__ import annotations

import json
import math
import shutil
from pathlib import Path
from typing import Any

import pandas as pd
from sklearn.model_selection import StratifiedShuffleSplit

import adapters as adapter_mod
from common_holdout import (
    load_labels,
    make_binary_labels,
    apply_multiclass_policy,
    evaluate_binary,
    evaluate_multiclass,
)
from review_config import (
    UNKNOWN_LABEL,
    SCAM_LABEL,
    TEST_RATIO,
    RANDOM_SEED,
    MIN_CLASS_SIZE,
    N_REPEATS,
    VAL_RATIO_WITHIN_DEV,
    RANDOM_SEED_BASE,
    D_GRID,
    R_GRID,
)

try:
    from scipy.stats import beta as beta_dist
except Exception:  # pragma: no cover
    beta_dist = None


def save_json(path: Path, obj: dict[str, Any]) -> None:
    path.parent.mkdir(parents=True, exist_ok=True)
    with open(path, "w", encoding="utf-8") as f:
        json.dump(obj, f, indent=2)


def write_df(df: pd.DataFrame, path: Path) -> None:
    path.parent.mkdir(parents=True, exist_ok=True)
    df.to_csv(path, index=False)


def set_graph_path(graph_path: str | Path) -> None:
    graph_path = Path(graph_path)
    if getattr(adapter_mod, "GRAPH_PATH", None) != graph_path:
        adapter_mod.GRAPH_PATH = graph_path
        if hasattr(adapter_mod, "_G_CACHE"):
            adapter_mod._G_CACHE = None


def load_branch_frames(labels_csv: str | Path, min_class_size: int = MIN_CLASS_SIZE) -> tuple[pd.DataFrame, pd.DataFrame, list[str], list[str]]:
    df = load_labels(str(labels_csv))
    df_bin = make_binary_labels(df)[["node_id", "binary_label"]].rename(columns={"binary_label": "label"})
    df_mul, keep_classes, merged_classes = apply_multiclass_policy(df, min_class_size=min_class_size)
    df_mul = df_mul[["node_id", "multiclass_label"]].rename(columns={"multiclass_label": "label"})
    return df_bin, df_mul, keep_classes, merged_classes


def stratified_split(df: pd.DataFrame, test_ratio: float, seed: int) -> tuple[pd.DataFrame, pd.DataFrame]:
    sss = StratifiedShuffleSplit(n_splits=1, test_size=test_ratio, random_state=seed)
    train_idx, test_idx = next(sss.split(df[["node_id"]], df["label"]))
    return df.iloc[train_idx].copy(), df.iloc[test_idx].copy()


def make_dev_repeats(dev_df: pd.DataFrame, n_repeats: int = N_REPEATS, val_ratio: float = VAL_RATIO_WITHIN_DEV,
                     seed_base: int = RANDOM_SEED_BASE) -> list[tuple[str, pd.DataFrame, pd.DataFrame, int]]:
    repeats: list[tuple[str, pd.DataFrame, pd.DataFrame, int]] = []
    for i in range(1, n_repeats + 1):
        repeat_id = f"repeat_{i:02d}"
        seed = seed_base + i
        sss = StratifiedShuffleSplit(n_splits=1, test_size=val_ratio, random_state=seed)
        train_idx, val_idx = next(sss.split(dev_df[["node_id"]], dev_df["label"]))
        train_df = dev_df.iloc[train_idx].copy()
        val_df = dev_df.iloc[val_idx].copy()
        repeats.append((repeat_id, train_df, val_df, seed))
    return repeats


def run_sslpa_from_train_df(train_df: pd.DataFrame, mode: str, out_csv: Path, graph_path: str | Path) -> pd.DataFrame:
    set_graph_path(graph_path)
    write_df(train_df, out_csv)
    pred_df = adapter_mod.run_existing_sslpa(out_csv, mode=mode)
    return pred_df


def tune_threshold_grid(raw_pred: pd.DataFrame, val_df: pd.DataFrame, mode: str,
                        d_grid: list[int] | None = None, r_grid: list[float] | None = None) -> tuple[dict[str, Any], pd.DataFrame, dict[str, Any]]:
    d_grid = d_grid or D_GRID
    r_grid = r_grid or R_GRID

    if mode == "binary":
        raw_metrics = evaluate_binary(raw_pred, val_df)
    else:
        raw_metrics = evaluate_multiclass(raw_pred, val_df)

    grid_rows = []
    for d_min in d_grid:
        for r_min in r_grid:
            pred_ncrf = adapter_mod.apply_existing_ncsf(raw_pred.copy(), d_min, r_min, mode=mode)
            if mode == "binary":
                metrics = evaluate_binary(pred_ncrf, val_df)
            else:
                metrics = evaluate_multiclass(pred_ncrf, val_df)
            row = {"d_min": d_min, "r_min": r_min, **metrics}
            grid_rows.append(row)

    grid_df = pd.DataFrame(grid_rows)

    if mode == "binary":
        best = grid_df.sort_values(["f1", "precision", "coverage"], ascending=False).iloc[0]
        selected = {
            "d_min": int(best["d_min"]),
            "r_min": float(best["r_min"]),
            "selection_metric": "f1",
            "selection_value": float(best["f1"]),
        }
    else:
        best = grid_df.sort_values(["macro_f1", "coverage"], ascending=False).iloc[0]
        selected = {
            "d_min": int(best["d_min"]),
            "r_min": float(best["r_min"]),
            "selection_metric": "macro_f1",
            "selection_value": float(best["macro_f1"]),
        }

    return raw_metrics, grid_df, selected


def choose_final_threshold_from_selected(selected_rows: list[dict[str, Any]], mode: str) -> dict[str, Any]:
    df = pd.DataFrame(selected_rows)
    grouped = (
        df.groupby(["d_min", "r_min"], as_index=False)["selection_value"]
        .mean()
        .sort_values("selection_value", ascending=False)
    )
    best = grouped.iloc[0]
    if mode == "binary":
        metric = "mean_validation_f1"
    else:
        metric = "mean_validation_macro_f1"
    return {
        "branch": mode,
        "d_min": int(best["d_min"]),
        "r_min": float(best["r_min"]),
        "selection_metric": metric,
        "selection_value": float(best["selection_value"]),
    }


def final_eval(dev_df: pd.DataFrame, test_df: pd.DataFrame, mode: str, final_threshold: dict[str, Any],
               out_dir: Path, graph_path: str | Path) -> dict[str, Any]:
    out_dir.mkdir(parents=True, exist_ok=True)
    dev_csv = out_dir / f"full_dev_labels_{mode}.csv"
    raw_pred = run_sslpa_from_train_df(dev_df, mode=mode, out_csv=dev_csv, graph_path=graph_path)
    write_df(raw_pred, out_dir / f"final_sslpa_predictions_raw_{mode}.csv")

    if mode == "binary":
        raw_metrics = evaluate_binary(raw_pred, test_df)
    else:
        raw_metrics = evaluate_multiclass(raw_pred, test_df)
    write_df(pd.DataFrame([raw_metrics]), out_dir / f"final_test_metrics_raw_sslpa_{mode}.csv")

    ncrf_pred = adapter_mod.apply_existing_ncsf(
        raw_pred.copy(),
        d_min=int(final_threshold["d_min"]),
        r_min=float(final_threshold["r_min"]),
        mode=mode,
    )
    write_df(ncrf_pred, out_dir / f"final_sslpa_predictions_ncrf_{mode}.csv")

    if mode == "binary":
        ncrf_metrics = evaluate_binary(ncrf_pred, test_df)
    else:
        ncrf_metrics = evaluate_multiclass(ncrf_pred, test_df)
    write_df(pd.DataFrame([ncrf_metrics]), out_dir / f"final_test_metrics_ncrf_{mode}.csv")

    return {
        "raw_pred": raw_pred,
        "ncrf_pred": ncrf_pred,
        "raw_metrics": raw_metrics,
        "ncrf_metrics": ncrf_metrics,
    }


def compute_risky_overlap(test_df: pd.DataFrame, filtered_pred: pd.DataFrame) -> dict[str, Any]:
    merged = test_df.merge(filtered_pred, on="node_id", how="left")
    merged["predicted_label"] = merged["predicted_label"].fillna(UNKNOWN_LABEL)
    flagged = merged[merged["predicted_label"] == SCAM_LABEL].copy()

    x = int(len(flagged))
    y = int((flagged["label"] == SCAM_LABEL).sum())
    z = int((merged["label"] == SCAM_LABEL).sum())

    return {
        "flagged_risky_addresses_X": x,
        "true_scam_among_flagged_Y": y,
        "total_true_scam_in_blind_test_Z": z,
        "overlap_fraction_Y_over_X": (y / x if x > 0 else 0.0),
        "capture_fraction_Y_over_Z": (y / z if z > 0 else 0.0),
    }


def apply_degree_only_filter(pred_df: pd.DataFrame, d_min: int, graph_path: str | Path) -> pd.DataFrame:
    set_graph_path(graph_path)
    g = adapter_mod._get_graph()
    label_map = dict(zip(pred_df["node_id"].astype(str), pred_df["predicted_label"].astype(str)))
    out_map = label_map.copy()

    for node_str, lbl in label_map.items():
        if lbl == UNKNOWN_LABEL:
            continue
        try:
            node_int = int(node_str)
        except ValueError:
            out_map[node_str] = UNKNOWN_LABEL
            continue
        if node_int not in g:
            out_map[node_str] = UNKNOWN_LABEL
            continue
        if len(list(g.neighbors(node_int))) < d_min:
            out_map[node_str] = UNKNOWN_LABEL

    out = pred_df.copy()
    out["predicted_label"] = out["node_id"].astype(str).map(out_map)
    return out


def apply_ratio_only_filter(pred_df: pd.DataFrame, r_min: float, graph_path: str | Path) -> pd.DataFrame:
    set_graph_path(graph_path)
    g = adapter_mod._get_graph()
    label_map = dict(zip(pred_df["node_id"].astype(str), pred_df["predicted_label"].astype(str)))
    out_map = label_map.copy()

    for node_str, lbl in label_map.items():
        if lbl == UNKNOWN_LABEL:
            continue
        try:
            node_int = int(node_str)
        except ValueError:
            out_map[node_str] = UNKNOWN_LABEL
            continue
        if node_int not in g:
            out_map[node_str] = UNKNOWN_LABEL
            continue
        neighbors = list(g.neighbors(node_int))
        deg = len(neighbors)
        if deg == 0:
            out_map[node_str] = UNKNOWN_LABEL
            continue
        same = sum(1 for nb in neighbors if label_map.get(str(nb), UNKNOWN_LABEL) == lbl)
        if same / deg < r_min:
            out_map[node_str] = UNKNOWN_LABEL

    out = pred_df.copy()
    out["predicted_label"] = out["node_id"].astype(str).map(out_map)
    return out


def _beta_lower_bound(successes: int, failures: int, q: float) -> float:
    a = successes + 1
    b = failures + 1
    if beta_dist is not None:
        return float(beta_dist.ppf(q, a, b))
    mean = a / (a + b)
    var = (a * b) / (((a + b) ** 2) * (a + b + 1))
    z = 1.2815515655446004  # approx 10th percentile lower z for one-sided normal
    return max(0.0, min(1.0, mean - z * math.sqrt(var)))


def compute_binary_risk_scores(raw_pred_df: pd.DataFrame, graph_path: str | Path, lower_q: float = 0.10) -> pd.DataFrame:
    set_graph_path(graph_path)
    g = adapter_mod._get_graph()
    label_map = dict(zip(raw_pred_df["node_id"].astype(str), raw_pred_df["predicted_label"].astype(str)))

    rows = []
    for node_str, pred_lbl in label_map.items():
        try:
            node_int = int(node_str)
        except ValueError:
            rows.append({
                "node_id": node_str,
                "predicted_label": pred_lbl,
                "degree": 0,
                "same_label_neighbors": 0,
                "same_label_ratio": 0.0,
                "risk_score": 0.0,
            })
            continue

        if pred_lbl != SCAM_LABEL or node_int not in g:
            rows.append({
                "node_id": node_str,
                "predicted_label": pred_lbl,
                "degree": int(len(list(g.neighbors(node_int)))) if node_int in g else 0,
                "same_label_neighbors": 0,
                "same_label_ratio": 0.0,
                "risk_score": 0.0,
            })
            continue

        neighbors = list(g.neighbors(node_int))
        deg = len(neighbors)
        same = sum(1 for nb in neighbors if label_map.get(str(nb), UNKNOWN_LABEL) == SCAM_LABEL)
        ratio = same / deg if deg else 0.0
        score = _beta_lower_bound(same, deg - same, lower_q) if deg else 0.0

        rows.append({
            "node_id": node_str,
            "predicted_label": pred_lbl,
            "degree": int(deg),
            "same_label_neighbors": int(same),
            "same_label_ratio": float(ratio),
            "risk_score": float(score),
        })

    out = pd.DataFrame(rows).sort_values(["risk_score", "same_label_ratio", "degree"], ascending=False)
    return out


def build_topk_summary(scored_eval_df: pd.DataFrame, top_ks: list[int]) -> pd.DataFrame:
    rows = []
    sorted_df = scored_eval_df.sort_values("risk_score", ascending=False).reset_index(drop=True)
    total_positive = int((sorted_df["label"] == SCAM_LABEL).sum())

    for k in top_ks:
        top = sorted_df.head(k).copy()
        tp = int((top["label"] == SCAM_LABEL).sum())
        precision = tp / len(top) if len(top) else 0.0
        recall = tp / total_positive if total_positive else 0.0
        rows.append({
            "k": int(k),
            "n_rows": int(len(top)),
            "tp_scam": tp,
            "precision_at_k": precision,
            "recall_at_k": recall,
        })
    return pd.DataFrame(rows)


def maybe_copy(src: Path, dst: Path) -> bool:
    if src.exists():
        dst.parent.mkdir(parents=True, exist_ok=True)
        shutil.copy2(src, dst)
        return True
    return False

```

## `run_submitted_holdout_pipeline.py`

```python

from __future__ import annotations

import subprocess
import sys
from pathlib import Path

HERE = Path(__file__).resolve().parent

STEPS = [
    "3_prepare_fixed_test_split.py",
    "4_prepare_dev_repeats.py",
    "5_run_sslpa_dev_repeats.py",
    "6_evaluate_dev_repeats_binary.py",
    "7_evaluate_dev_repeats_multiclass.py",
    "8_choose_final_thresholds.py",
    "9_run_final_dev_sslpa_and_test.py",
    "10_report_flagged_risky_addresses.py",
]


def run_step(script_name: str) -> None:
    script_path = HERE / script_name
    print(f"\n=== Running {script_name} ===")
    subprocess.run([sys.executable, str(script_path)], check=True)


def main() -> None:
    for step in STEPS:
        run_step(step)
    print("\nDone: submitted holdout pipeline finished cleanly.")


if __name__ == "__main__":
    main()

```

## `12_plot_binary_tradeoff.py`

```python

from __future__ import annotations

import json

import matplotlib.pyplot as plt
import pandas as pd

from review_config import DEV_BINARY_DIR, FINAL_TEST_DIR, REVIEW_ASSETS_DIR


def load_threshold_grids() -> pd.DataFrame:
    frames = []
    for csv_path in sorted(DEV_BINARY_DIR.glob("repeat_*_threshold_grid_binary.csv")):
        df = pd.read_csv(csv_path)
        df["source_file"] = csv_path.name
        frames.append(df)
    if not frames:
        raise FileNotFoundError("No binary threshold-grid CSVs found.")
    return pd.concat(frames, ignore_index=True)


def aggregate_grid(df: pd.DataFrame) -> pd.DataFrame:
    grouped = (
        df.groupby(["d_min", "r_min"], as_index=False)
        .agg(
            precision_mean=("precision", "mean"),
            precision_std=("precision", "std"),
            recall_mean=("recall", "mean"),
            f1_mean=("f1", "mean"),
            coverage_mean=("coverage", "mean"),
            n=("repeat_id", "count"),
        )
        .sort_values(["f1_mean", "precision_mean", "coverage_mean"], ascending=False)
    )
    return grouped


def frontier(df: pd.DataFrame, xcol: str, ycol: str) -> pd.DataFrame:
    ordered = df.sort_values(xcol, ascending=True).copy()
    best = -1.0
    keep = []
    for _, row in ordered.iterrows():
        if row[ycol] >= best:
            keep.append(True)
            best = row[ycol]
        else:
            keep.append(False)
    return ordered[pd.Series(keep, index=ordered.index)]


def main() -> None:
    REVIEW_ASSETS_DIR.mkdir(parents=True, exist_ok=True)

    grids = load_threshold_grids()
    table = aggregate_grid(grids)

    with open(FINAL_TEST_DIR / "final_threshold_binary.json", "r", encoding="utf-8") as f:
        final_cfg = json.load(f)

    raw_metrics = pd.read_csv(FINAL_TEST_DIR / "final_test_metrics_raw_sslpa_binary.csv").iloc[0].to_dict()
    filt_metrics = pd.read_csv(FINAL_TEST_DIR / "final_test_metrics_ncsf_binary.csv").iloc[0].to_dict()

    chosen = table[(table["d_min"] == final_cfg["d_min"]) & (table["r_min"] == final_cfg["r_min"])]
    chosen.to_csv(REVIEW_ASSETS_DIR / "binary_tradeoff_table.csv", index=False)

    front_p = frontier(table, "coverage_mean", "precision_mean")
    front_f1 = frontier(table, "coverage_mean", "f1_mean")

    plt.figure(figsize=(8, 6))
    plt.scatter(table["coverage_mean"], table["precision_mean"], alpha=0.45, label="Threshold grid")
    plt.plot(front_p["coverage_mean"], front_p["precision_mean"], linewidth=1.5, label="Pareto-like frontier")
    plt.scatter([raw_metrics["coverage"]], [raw_metrics["precision"]], marker="x", s=120, label="Raw SSLPA")
    plt.scatter([filt_metrics["coverage"]], [filt_metrics["precision"]], marker="D", s=100, label="Chosen NCRF")
    plt.xlabel("Coverage")
    plt.ylabel("Precision")
    plt.title("Binary tradeoff: coverage vs precision")
    plt.legend()
    plt.tight_layout()
    plt.savefig(REVIEW_ASSETS_DIR / "binary_tradeoff_coverage_precision.png", dpi=220)
    plt.close()

    plt.figure(figsize=(8, 6))
    plt.scatter(table["coverage_mean"], table["f1_mean"], alpha=0.45, label="Threshold grid")
    plt.plot(front_f1["coverage_mean"], front_f1["f1_mean"], linewidth=1.5, label="Pareto-like frontier")
    plt.scatter([raw_metrics["coverage"]], [raw_metrics["f1"]], marker="x", s=120, label="Raw SSLPA")
    plt.scatter([filt_metrics["coverage"]], [filt_metrics["f1"]], marker="D", s=100, label="Chosen NCRF")
    plt.xlabel("Coverage")
    plt.ylabel("F1")
    plt.title("Binary tradeoff: coverage vs F1")
    plt.legend()
    plt.tight_layout()
    plt.savefig(REVIEW_ASSETS_DIR / "binary_tradeoff_coverage_f1.png", dpi=220)
    plt.close()

    table.to_csv(REVIEW_ASSETS_DIR / "binary_tradeoff_full_grid_aggregated.csv", index=False)
    print("Done: binary tradeoff artifacts saved.")


if __name__ == "__main__":
    main()

```

## `13_summarize_threshold_stability.py`

```python

from __future__ import annotations

import matplotlib.pyplot as plt
import pandas as pd

from review_config import DEV_BINARY_DIR, DEV_MULTICLASS_DIR, REVIEW_ASSETS_DIR


def summarize(summary_csv, branch_name: str) -> pd.DataFrame:
    df = pd.read_csv(summary_csv)
    out = (
        df.groupby(["d_min", "r_min"], as_index=False)
        .agg(
            chosen_count=("repeat_id", "count"),
            mean_selection_value=("selection_value", "mean"),
            std_selection_value=("selection_value", "std"),
        )
        .sort_values(["chosen_count", "mean_selection_value"], ascending=False)
    )
    out["branch"] = branch_name
    return out


def plot_heat_like(table: pd.DataFrame, branch_name: str, outfile) -> None:
    pivot = table.pivot(index="d_min", columns="r_min", values="chosen_count").fillna(0)
    plt.figure(figsize=(8, 5))
    plt.imshow(pivot.values, aspect="auto")
    plt.xticks(range(len(pivot.columns)), [str(c) for c in pivot.columns])
    plt.yticks(range(len(pivot.index)), [str(i) for i in pivot.index])
    plt.xlabel("r_min")
    plt.ylabel("d_min")
    plt.title(f"Threshold stability: {branch_name}")
    for i in range(pivot.shape[0]):
        for j in range(pivot.shape[1]):
            plt.text(j, i, int(pivot.values[i, j]), ha="center", va="center")
    plt.tight_layout()
    plt.savefig(outfile, dpi=220)
    plt.close()


def main() -> None:
    REVIEW_ASSETS_DIR.mkdir(parents=True, exist_ok=True)

    bin_table = summarize(DEV_BINARY_DIR / "threshold_selection_summary_binary.csv", "binary")
    mul_table = summarize(DEV_MULTICLASS_DIR / "threshold_selection_summary_multiclass.csv", "multiclass")

    bin_table.to_csv(REVIEW_ASSETS_DIR / "binary_threshold_stability.csv", index=False)
    mul_table.to_csv(REVIEW_ASSETS_DIR / "multiclass_threshold_stability.csv", index=False)

    plot_heat_like(bin_table, "binary", REVIEW_ASSETS_DIR / "binary_threshold_stability.png")
    plot_heat_like(mul_table, "multiclass", REVIEW_ASSETS_DIR / "multiclass_threshold_stability.png")

    with open(REVIEW_ASSETS_DIR / "threshold_stability_note.md", "w", encoding="utf-8") as f:
        f.write("# Threshold stability note\n\n")
        f.write("Binary threshold selections should be interpreted as the main stability signal.\n")
        f.write("Multiclass threshold selections should be interpreted more cautiously if they are more diffuse.\n")

    print("Done: threshold stability summaries saved.")


if __name__ == "__main__":
    main()

```

## `14_run_outer_split_sensitivity.py`

```python

from __future__ import annotations

import argparse
import json
from pathlib import Path

import pandas as pd

from holdout_experiment_lib import (
    load_branch_frames,
    stratified_split,
    make_dev_repeats,
    run_sslpa_from_train_df,
    tune_threshold_grid,
    choose_final_threshold_from_selected,
    final_eval,
    compute_risky_overlap,
    save_json,
    write_df,
)
from review_config import (
    LABELS_CSV,
    GRAPH_PATH,
    MIN_CLASS_SIZE,
    TEST_RATIO,
    OUTER_N_SPLITS,
    OUTER_RANDOM_SEED_BASE,
    N_REPEATS,
    RANDOM_SEED_BASE,
    VAL_RATIO_WITHIN_DEV,
    OUTER_SENSITIVITY_DIR,
)


def run_branch(branch: str, dev_df: pd.DataFrame, test_df: pd.DataFrame, out_dir: Path, graph_path: Path) -> dict:
    branch_dir = out_dir / branch
    branch_dir.mkdir(parents=True, exist_ok=True)

    selected_rows = []
    raw_repeat_metrics = []

    for repeat_id, train_df, val_df, seed in make_dev_repeats(
        dev_df, n_repeats=N_REPEATS, val_ratio=VAL_RATIO_WITHIN_DEV, seed_base=RANDOM_SEED_BASE
    ):
        repeat_dir = branch_dir / repeat_id
        repeat_dir.mkdir(parents=True, exist_ok=True)

        pred_df = run_sslpa_from_train_df(
            train_df,
            mode=branch,
            out_csv=repeat_dir / f"{repeat_id}_train_labels_{branch}.csv",
            graph_path=graph_path,
        )
        write_df(pred_df, repeat_dir / f"{repeat_id}_sslpa_predictions_raw_{branch}.csv")
        write_df(val_df, repeat_dir / f"{repeat_id}_val_labels_{branch}.csv")

        raw_metrics, grid_df, selected = tune_threshold_grid(pred_df, val_df, mode=branch)
        selected["repeat_id"] = repeat_id
        selected["seed"] = seed
        selected_rows.append(selected)
        raw_repeat_metrics.append({"repeat_id": repeat_id, **raw_metrics})
        write_df(pd.DataFrame([raw_metrics]), repeat_dir / f"{repeat_id}_val_metrics_raw_sslpa_{branch}.csv")
        write_df(grid_df.assign(repeat_id=repeat_id), repeat_dir / f"{repeat_id}_threshold_grid_{branch}.csv")
        save_json(repeat_dir / f"{repeat_id}_selected_threshold_{branch}.json", selected)

    selected_df = pd.DataFrame(selected_rows)
    raw_repeat_df = pd.DataFrame(raw_repeat_metrics)
    write_df(selected_df, branch_dir / f"threshold_selection_summary_{branch}.csv")
    write_df(raw_repeat_df, branch_dir / f"raw_repeat_validation_metrics_{branch}.csv")

    final_threshold = choose_final_threshold_from_selected(selected_rows, mode=branch)
    save_json(branch_dir / f"final_threshold_{branch}.json", final_threshold)

    final_results = final_eval(
        dev_df=dev_df,
        test_df=test_df,
        mode=branch,
        final_threshold=final_threshold,
        out_dir=branch_dir,
        graph_path=graph_path,
    )

    overlap = None
    if branch == "binary":
        overlap = compute_risky_overlap(test_df, final_results["ncrf_pred"])
        write_df(pd.DataFrame([overlap]), branch_dir / "final_test_risky_overlap_summary.csv")

    return {
        "branch": branch,
        "final_threshold": final_threshold,
        "raw_metrics": final_results["raw_metrics"],
        "ncrf_metrics": final_results["ncrf_metrics"],
        "overlap": overlap,
    }


def main() -> None:
    parser = argparse.ArgumentParser()
    parser.add_argument("--labels-csv", default=str(LABELS_CSV))
    parser.add_argument("--graph-path", default=str(GRAPH_PATH))
    parser.add_argument("--out-dir", default=str(OUTER_SENSITIVITY_DIR))
    parser.add_argument("--n-outer-splits", type=int, default=OUTER_N_SPLITS)
    parser.add_argument("--outer-seed-base", type=int, default=OUTER_RANDOM_SEED_BASE)
    args = parser.parse_args()

    labels_csv = Path(args.labels_csv)
    graph_path = Path(args.graph_path)
    out_dir = Path(args.out_dir)
    out_dir.mkdir(parents=True, exist_ok=True)

    df_bin, df_mul, keep_classes, merged_classes = load_branch_frames(labels_csv, min_class_size=MIN_CLASS_SIZE)
    manifest_rows = []

    for i in range(1, args.n_outer_splits + 1):
        outer_seed = args.outer_seed_base + i
        outer_run_id = f"outer_run_{i:03d}"
        run_dir = out_dir / outer_run_id
        run_dir.mkdir(parents=True, exist_ok=True)

        dev_bin, test_bin = stratified_split(df_bin, test_ratio=TEST_RATIO, seed=outer_seed)
        dev_mul, test_mul = stratified_split(df_mul, test_ratio=TEST_RATIO, seed=outer_seed)

        save_json(run_dir / "outer_split_config.json", {
            "outer_run_id": outer_run_id,
            "outer_seed": outer_seed,
            "test_ratio": TEST_RATIO,
            "n_repeats": N_REPEATS,
            "val_ratio_within_dev": VAL_RATIO_WITHIN_DEV,
            "labels_csv": str(labels_csv),
            "graph_path": str(graph_path),
            "multiclass_keep_classes": keep_classes,
            "multiclass_merged_classes": merged_classes,
        })

        write_df(dev_bin, run_dir / "binary" / "outer_dev_labels_binary.csv")
        write_df(test_bin, run_dir / "binary" / "outer_test_labels_binary.csv")
        write_df(dev_mul, run_dir / "multiclass" / "outer_dev_labels_multiclass.csv")
        write_df(test_mul, run_dir / "multiclass" / "outer_test_labels_multiclass.csv")

        bin_res = run_branch("binary", dev_bin, test_bin, run_dir, graph_path)
        mul_res = run_branch("multiclass", dev_mul, test_mul, run_dir, graph_path)

        manifest_rows.append({
            "outer_run_id": outer_run_id,
            "outer_seed": outer_seed,
            "binary_d_min": bin_res["final_threshold"]["d_min"],
            "binary_r_min": bin_res["final_threshold"]["r_min"],
            "multiclass_d_min": mul_res["final_threshold"]["d_min"],
            "multiclass_r_min": mul_res["final_threshold"]["r_min"],
        })

    write_df(pd.DataFrame(manifest_rows), out_dir / "outer_run_manifest.csv")
    print(f"Done: outer split sensitivity artifacts saved under {out_dir}")


if __name__ == "__main__":
    main()

```

## `15_aggregate_outer_split_results.py`

```python

from __future__ import annotations

import argparse
from pathlib import Path

import matplotlib.pyplot as plt
import pandas as pd

from review_config import OUTER_SENSITIVITY_DIR


def load_metric_rows(out_dir: Path, branch: str) -> tuple[pd.DataFrame, pd.DataFrame, pd.DataFrame | None]:
    raw_rows = []
    ncrf_rows = []
    overlap_rows = []

    for run_dir in sorted(out_dir.glob("outer_run_*")):
        raw_csv = run_dir / branch / f"final_test_metrics_raw_sslpa_{branch}.csv"
        ncrf_csv = run_dir / branch / f"final_test_metrics_ncrf_{branch}.csv"
        if raw_csv.exists() and ncrf_csv.exists():
            raw = pd.read_csv(raw_csv).iloc[0].to_dict()
            ncrf = pd.read_csv(ncrf_csv).iloc[0].to_dict()
            raw["outer_run_id"] = run_dir.name
            ncrf["outer_run_id"] = run_dir.name
            raw_rows.append(raw)
            ncrf_rows.append(ncrf)

        overlap_csv = run_dir / "binary" / "final_test_risky_overlap_summary.csv"
        if branch == "binary" and overlap_csv.exists():
            row = pd.read_csv(overlap_csv).iloc[0].to_dict()
            row["outer_run_id"] = run_dir.name
            overlap_rows.append(row)

    raw_df = pd.DataFrame(raw_rows)
    ncrf_df = pd.DataFrame(ncrf_rows)
    overlap_df = pd.DataFrame(overlap_rows) if overlap_rows else None
    return raw_df, ncrf_df, overlap_df


def describe_df(df: pd.DataFrame, metrics: list[str]) -> pd.DataFrame:
    rows = []
    for metric in metrics:
        series = df[metric].dropna()
        rows.append({
            "metric": metric,
            "mean": series.mean(),
            "median": series.median(),
            "std": series.std(),
            "p02_5": series.quantile(0.025),
            "p97_5": series.quantile(0.975),
            "n": len(series),
        })
    return pd.DataFrame(rows)


def paired_deltas(raw_df: pd.DataFrame, ncrf_df: pd.DataFrame, metrics: list[str]) -> pd.DataFrame:
    merged = raw_df.merge(ncrf_df, on="outer_run_id", suffixes=("_raw", "_ncrf"))
    rows = []
    for _, row in merged.iterrows():
        out = {"outer_run_id": row["outer_run_id"]}
        for metric in metrics:
            out[f"delta_{metric}"] = row[f"{metric}_ncrf"] - row[f"{metric}_raw"]
        rows.append(out)
    return pd.DataFrame(rows)


def save_boxplot(df: pd.DataFrame, metrics: list[str], title: str, out_png: Path) -> None:
    plt.figure(figsize=(10, 6))
    plt.boxplot([df[m].dropna() for m in metrics], labels=metrics)
    plt.title(title)
    plt.tight_layout()
    plt.savefig(out_png, dpi=220)
    plt.close()


def main() -> None:
    parser = argparse.ArgumentParser()
    parser.add_argument("--out-dir", default=str(OUTER_SENSITIVITY_DIR))
    args = parser.parse_args()

    out_dir = Path(args.out_dir)
    plots_dir = out_dir / "plots"
    plots_dir.mkdir(parents=True, exist_ok=True)

    # binary
    raw_bin, ncrf_bin, overlap_df = load_metric_rows(out_dir, "binary")
    if not raw_bin.empty and not ncrf_bin.empty:
        raw_bin.to_csv(out_dir / "per_split_metrics_binary_raw.csv", index=False)
        ncrf_bin.to_csv(out_dir / "per_split_metrics_binary_ncrf.csv", index=False)
        deltas_bin = paired_deltas(raw_bin, ncrf_bin, ["precision", "recall", "f1", "coverage"])
        deltas_bin.to_csv(out_dir / "paired_deltas_binary.csv", index=False)

        describe_df(raw_bin, ["precision", "recall", "f1", "coverage"]).to_csv(
            out_dir / "summary_binary_raw.csv", index=False
        )
        describe_df(ncrf_bin, ["precision", "recall", "f1", "coverage"]).to_csv(
            out_dir / "summary_binary_ncrf.csv", index=False
        )
        describe_df(deltas_bin, [c for c in deltas_bin.columns if c.startswith("delta_")]).to_csv(
            out_dir / "summary_binary_deltas.csv", index=False
        )

        save_boxplot(raw_bin, ["precision", "recall", "f1", "coverage"], "Outer-split binary raw SSLPA", plots_dir / "binary_raw_boxplot.png")
        save_boxplot(ncrf_bin, ["precision", "recall", "f1", "coverage"], "Outer-split binary NCRF", plots_dir / "binary_ncrf_boxplot.png")
        save_boxplot(deltas_bin, [c for c in deltas_bin.columns if c.startswith("delta_")], "Outer-split binary paired deltas", plots_dir / "binary_delta_boxplot.png")

    if overlap_df is not None and not overlap_df.empty:
        overlap_df.to_csv(out_dir / "per_split_risky_overlap_binary.csv", index=False)
        describe_df(
            overlap_df,
            ["flagged_risky_addresses_X", "true_scam_among_flagged_Y", "total_true_scam_in_blind_test_Z",
             "overlap_fraction_Y_over_X", "capture_fraction_Y_over_Z"]
        ).to_csv(out_dir / "summary_risky_overlap_binary.csv", index=False)

    # multiclass
    raw_mul, ncrf_mul, _ = load_metric_rows(out_dir, "multiclass")
    if not raw_mul.empty and not ncrf_mul.empty:
        metrics = ["macro_f1", "micro_f1", "weighted_f1", "nmi", "ari", "ami", "fmi", "coverage"]
        raw_mul.to_csv(out_dir / "per_split_metrics_multiclass_raw.csv", index=False)
        ncrf_mul.to_csv(out_dir / "per_split_metrics_multiclass_ncrf.csv", index=False)
        deltas_mul = paired_deltas(raw_mul, ncrf_mul, metrics)
        deltas_mul.to_csv(out_dir / "paired_deltas_multiclass.csv", index=False)

        describe_df(raw_mul, metrics).to_csv(out_dir / "summary_multiclass_raw.csv", index=False)
        describe_df(ncrf_mul, metrics).to_csv(out_dir / "summary_multiclass_ncrf.csv", index=False)
        describe_df(deltas_mul, [c for c in deltas_mul.columns if c.startswith("delta_")]).to_csv(
            out_dir / "summary_multiclass_deltas.csv", index=False
        )

        save_boxplot(raw_mul, metrics, "Outer-split multiclass raw SSLPA", plots_dir / "multiclass_raw_boxplot.png")
        save_boxplot(ncrf_mul, metrics, "Outer-split multiclass NCRF", plots_dir / "multiclass_ncrf_boxplot.png")
        save_boxplot(deltas_mul, [c for c in deltas_mul.columns if c.startswith("delta_")], "Outer-split multiclass paired deltas", plots_dir / "multiclass_delta_boxplot.png")

    print(f"Done: aggregated outer sensitivity artifacts saved under {out_dir}")


if __name__ == "__main__":
    main()

```


## `17_run_filter_ablation.py`

```python

from __future__ import annotations

import argparse
import json
from pathlib import Path

import pandas as pd

from holdout_experiment_lib import (
    apply_degree_only_filter,
    apply_ratio_only_filter,
    write_df,
)
from common_holdout import evaluate_binary, evaluate_multiclass
from review_config import FINAL_TEST_DIR, GRAPH_PATH, REVIEW_ASSETS_DIR


def main() -> None:
    parser = argparse.ArgumentParser()
    parser.add_argument("--final-dir", default=str(FINAL_TEST_DIR))
    parser.add_argument("--graph-path", default=str(GRAPH_PATH))
    parser.add_argument("--out-dir", default=str(REVIEW_ASSETS_DIR))
    args = parser.parse_args()

    final_dir = Path(args.final_dir)
    out_dir = Path(args.out_dir)
    out_dir.mkdir(parents=True, exist_ok=True)

    # binary
    raw_bin = pd.read_csv(final_dir / "final_sslpa_predictions_raw_binary.csv")
    test_bin = pd.read_csv(final_dir / "fixed_test_labels_binary.csv") if (final_dir / "fixed_test_labels_binary.csv").exists() else None
    if test_bin is None:
        alt = final_dir.parent.parent / "step6_holdout" / "fixed_test" / "fixed_test_labels_binary.csv"
        test_bin = pd.read_csv(alt)

    with open(final_dir / "final_threshold_binary.json", "r", encoding="utf-8") as f:
        bin_cfg = json.load(f)

    degree_only_bin = apply_degree_only_filter(raw_bin, int(bin_cfg["d_min"]), args.graph_path)
    ratio_only_bin = apply_ratio_only_filter(raw_bin, float(bin_cfg["r_min"]), args.graph_path)

    rows_bin = [
        {"variant": "raw_sslpa", **evaluate_binary(raw_bin, test_bin)},
        {"variant": "degree_only", **evaluate_binary(degree_only_bin, test_bin)},
        {"variant": "ratio_only", **evaluate_binary(ratio_only_bin, test_bin)},
    ]

    # multiclass
    raw_mul = pd.read_csv(final_dir / "final_sslpa_predictions_raw_multiclass.csv")
    test_mul = pd.read_csv(final_dir / "fixed_test_labels_multiclass.csv") if (final_dir / "fixed_test_labels_multiclass.csv").exists() else None
    if test_mul is None:
        alt = final_dir.parent.parent / "step6_holdout" / "fixed_test" / "fixed_test_labels_multiclass.csv"
        test_mul = pd.read_csv(alt)

    with open(final_dir / "final_threshold_multiclass.json", "r", encoding="utf-8") as f:
        mul_cfg = json.load(f)

    degree_only_mul = apply_degree_only_filter(raw_mul, int(mul_cfg["d_min"]), args.graph_path)
    ratio_only_mul = apply_ratio_only_filter(raw_mul, float(mul_cfg["r_min"]), args.graph_path)

    rows_mul = [
        {"variant": "raw_sslpa", **evaluate_multiclass(raw_mul, test_mul)},
        {"variant": "degree_only", **evaluate_multiclass(degree_only_mul, test_mul)},
        {"variant": "ratio_only", **evaluate_multiclass(ratio_only_mul, test_mul)},
    ]

    write_df(pd.DataFrame(rows_bin), out_dir / "ablation_binary.csv")
    write_df(pd.DataFrame(rows_mul), out_dir / "ablation_multiclass.csv")

    with open(out_dir / "ablation_note.md", "w", encoding="utf-8") as f:
        f.write("# Ablation note\n\n")
        f.write("This compares raw SSLPA against degree-only and ratio-only filtering.\n")
        f.write("Full NCRF/NCSF should be compared against these using the final test outputs already saved by the main pipeline.\n")

    print(f"Done: ablation artifacts saved under {out_dir}")


if __name__ == "__main__":
    main()

```

## `18_build_multiclass_uncertainty_appendix.py`

```python

from __future__ import annotations

import argparse
from pathlib import Path

import pandas as pd

from review_config import DEV_MULTICLASS_DIR, FINAL_TEST_DIR, REVIEW_ASSETS_DIR, OUTER_SENSITIVITY_DIR


def maybe_load(path: Path) -> pd.DataFrame | None:
    return pd.read_csv(path) if path.exists() else None


def main() -> None:
    parser = argparse.ArgumentParser()
    parser.add_argument("--dev-multiclass-dir", default=str(DEV_MULTICLASS_DIR))
    parser.add_argument("--final-dir", default=str(FINAL_TEST_DIR))
    parser.add_argument("--outer-dir", default=str(OUTER_SENSITIVITY_DIR))
    parser.add_argument("--out-dir", default=str(REVIEW_ASSETS_DIR))
    args = parser.parse_args()

    dev_dir = Path(args.dev_multiclass_dir)
    final_dir = Path(args.final_dir)
    outer_dir = Path(args.outer_dir)
    out_dir = Path(args.out_dir)
    out_dir.mkdir(parents=True, exist_ok=True)

    selected = maybe_load(dev_dir / "threshold_selection_summary_multiclass.csv")
    if selected is not None:
        summary = (
            selected.groupby(["d_min", "r_min"], as_index=False)
            .agg(
                chosen_count=("repeat_id", "count"),
                mean_selection_value=("selection_value", "mean"),
                std_selection_value=("selection_value", "std"),
            )
            .sort_values(["chosen_count", "mean_selection_value"], ascending=False)
        )
        summary.to_csv(out_dir / "multiclass_threshold_appendix_summary.csv", index=False)

    # support counts
    fixed_test = final_dir / "fixed_test_labels_multiclass.csv"
    if not fixed_test.exists():
        fixed_test = final_dir.parent.parent / "step6_holdout" / "fixed_test" / "fixed_test_labels_multiclass.csv"
    test_df = pd.read_csv(fixed_test)
    test_df.groupby("label").size().reset_index(name="count").sort_values("count", ascending=False).to_csv(
        out_dir / "multiclass_fixed_test_support_counts.csv", index=False
    )

    # outer sensitivity if available
    outer_summary = maybe_load(outer_dir / "summary_multiclass_ncrf.csv")
    if outer_summary is not None:
        outer_summary.to_csv(out_dir / "multiclass_outer_sensitivity_summary.csv", index=False)

    with open(out_dir / "multiclass_uncertainty_appendix.md", "w", encoding="utf-8") as f:
        f.write("# Multiclass uncertainty appendix\n\n")
        f.write("- Threshold stability is summarized in `multiclass_threshold_appendix_summary.csv` when available.\n")
        f.write("- Fixed blind-test support counts are listed in `multiclass_fixed_test_support_counts.csv`.\n")
        if outer_summary is not None:
            f.write("- Outer-split multiclass robustness summary is included when available.\n")
        f.write("\nInterpretation rule: multiclass remains supporting evidence, not the main practical identity of the paper.\n")

    print(f"Done: multiclass appendix artifacts saved under {out_dir}")


if __name__ == "__main__":
    main()

```

## `19_build_review_artifact_pack.py`

```python

from __future__ import annotations

import argparse
from pathlib import Path

import pandas as pd

from holdout_experiment_lib import maybe_copy, save_json
from review_config import FINAL_TEST_DIR, REVIEW_ASSETS_DIR, OUTER_SENSITIVITY_DIR, WINDOW_SENSITIVITY_DIR, REVIEW_PACK_DIR


def main() -> None:
    parser = argparse.ArgumentParser()
    parser.add_argument("--final-dir", default=str(FINAL_TEST_DIR))
    parser.add_argument("--review-assets-dir", default=str(REVIEW_ASSETS_DIR))
    parser.add_argument("--outer-dir", default=str(OUTER_SENSITIVITY_DIR))
    parser.add_argument("--window-dir", default=str(WINDOW_SENSITIVITY_DIR))
    parser.add_argument("--out-dir", default=str(REVIEW_PACK_DIR))
    args = parser.parse_args()

    final_dir = Path(args.final_dir)
    review_assets_dir = Path(args.review_assets_dir)
    outer_dir = Path(args.outer_dir)
    window_dir = Path(args.window_dir)
    out_dir = Path(args.out_dir)
    out_dir.mkdir(parents=True, exist_ok=True)

    copied = []

    core_files = [
        final_dir / "final_test_metrics_raw_sslpa_binary.csv",
        final_dir / "final_test_metrics_ncsf_binary.csv",
        final_dir / "final_test_metrics_raw_sslpa_multiclass.csv",
        final_dir / "final_test_metrics_ncsf_multiclass.csv",
        final_dir / "final_threshold_binary.json",
        final_dir / "final_threshold_multiclass.json",
        final_dir / "final_test_risky_overlap_summary.csv",
    ]
    for src in core_files:
        dst = out_dir / "core" / src.name
        if maybe_copy(src, dst):
            copied.append(str(dst.relative_to(out_dir)))

    for src in review_assets_dir.glob("*"):
        dst = out_dir / "review_assets" / src.name
        if maybe_copy(src, dst):
            copied.append(str(dst.relative_to(out_dir)))

    optional_files = [
        outer_dir / "summary_binary_ncrf.csv",
        outer_dir / "summary_multiclass_ncrf.csv",
        outer_dir / "summary_binary_deltas.csv",
        window_dir / "window_manifest.csv",
        window_dir / "per_window_metrics.csv",
        review_assets_dir / "ablation_binary.csv",
        review_assets_dir / "ablation_multiclass.csv",
        review_assets_dir / "multiclass_uncertainty_appendix.md",
    ]
    for src in optional_files:
        dst = out_dir / "optional" / src.name
        if maybe_copy(src, dst):
            copied.append(str(dst.relative_to(out_dir)))

    save_json(out_dir / "review_pack_manifest.json", {"copied_files": copied})

    with open(out_dir / "README.md", "w", encoding="utf-8") as f:
        f.write("# Review artifact pack\n\n")
        f.write("This pack bundles core final-test outputs and available review-defense artifacts.\n")
        f.write("\n## Included files\n")
        for rel in copied:
            f.write(f"- `{rel}`\n")

    print(f"Done: review artifact pack saved under {out_dir}")


if __name__ == "__main__":
    main()

```

## `20_compute_continuous_risk_scores.py`

```python

from __future__ import annotations

import argparse
from pathlib import Path

import pandas as pd

from holdout_experiment_lib import compute_binary_risk_scores, write_df
from review_config import FINAL_TEST_DIR, GRAPH_PATH, RISK_SCORE_DIR, RISK_SCORE_BETA_LOWER_Q


def main() -> None:
    parser = argparse.ArgumentParser()
    parser.add_argument("--final-dir", default=str(FINAL_TEST_DIR))
    parser.add_argument("--graph-path", default=str(GRAPH_PATH))
    parser.add_argument("--out-dir", default=str(RISK_SCORE_DIR))
    parser.add_argument("--lower-q", type=float, default=RISK_SCORE_BETA_LOWER_Q)
    args = parser.parse_args()

    final_dir = Path(args.final_dir)
    out_dir = Path(args.out_dir)
    out_dir.mkdir(parents=True, exist_ok=True)

    raw_pred = pd.read_csv(final_dir / "final_sslpa_predictions_raw_binary.csv")
    scored = compute_binary_risk_scores(raw_pred, graph_path=args.graph_path, lower_q=args.lower_q)
    write_df(scored, out_dir / "final_test_scored_addresses.csv")

    with open(out_dir / "risk_score_note.md", "w", encoding="utf-8") as f:
        f.write("# Continuous risk score note\n\n")
        f.write("Score is a conservative lower-confidence bound based on same-label local support in the raw SSLPA neighborhood.\n")
        f.write("This is a ranking branch, not a replacement for thresholded NCRF screening.\n")

    print(f"Done: risk scores saved under {out_dir}")


if __name__ == "__main__":
    main()

```

## `21_evaluate_continuous_risk_ranking.py`

```python

from __future__ import annotations

import argparse
from pathlib import Path

import pandas as pd
from sklearn.metrics import average_precision_score, precision_recall_curve

from holdout_experiment_lib import build_topk_summary, write_df
from review_config import FINAL_TEST_DIR, RISK_SCORE_DIR, TOP_KS


def main() -> None:
    parser = argparse.ArgumentParser()
    parser.add_argument("--final-dir", default=str(FINAL_TEST_DIR))
    parser.add_argument("--risk-score-dir", default=str(RISK_SCORE_DIR))
    parser.add_argument("--top-ks", default=",".join(str(k) for k in TOP_KS))
    args = parser.parse_args()

    final_dir = Path(args.final_dir)
    risk_dir = Path(args.risk_score_dir)
    top_ks = [int(x) for x in args.top_ks.split(",") if x]

    scored = pd.read_csv(risk_dir / "final_test_scored_addresses.csv")

    test_bin = final_dir / "fixed_test_labels_binary.csv"
    if not test_bin.exists():
        test_bin = final_dir.parent.parent / "step6_holdout" / "fixed_test" / "fixed_test_labels_binary.csv"
    truth = pd.read_csv(test_bin)

    eval_df = truth.merge(scored, on="node_id", how="left")
    eval_df["risk_score"] = eval_df["risk_score"].fillna(0.0)
    eval_df["y_true"] = (eval_df["label"] == "SCAM").astype(int)

    ap = average_precision_score(eval_df["y_true"], eval_df["risk_score"])
    precision, recall, thresholds = precision_recall_curve(eval_df["y_true"], eval_df["risk_score"])
    pr_df = pd.DataFrame({
        "precision": precision,
        "recall": recall,
    })
    if len(thresholds) == len(pr_df) - 1:
        pr_df["threshold"] = list(thresholds) + [None]
    else:
        pr_df["threshold"] = None

    topk_df = build_topk_summary(eval_df[["node_id", "label", "risk_score"]], top_ks)

    write_df(pr_df, risk_dir / "final_test_pr_table.csv")
    write_df(topk_df, risk_dir / "final_test_topk_summary.csv")
    write_df(pd.DataFrame([{"average_precision": ap, "n_eval_rows": len(eval_df)}]), risk_dir / "final_test_ranking_summary.csv")
    write_df(eval_df.sort_values("risk_score", ascending=False), risk_dir / "final_test_scored_eval_join.csv")

    print(f"Done: ranking evaluation artifacts saved under {risk_dir}")


if __name__ == "__main__":
    main()

```

## `22_plot_pr_curve_risk_score.py`

```python

from __future__ import annotations

import argparse
from pathlib import Path

import matplotlib.pyplot as plt
import pandas as pd

from review_config import RISK_SCORE_DIR


def main() -> None:
    parser = argparse.ArgumentParser()
    parser.add_argument("--risk-score-dir", default=str(RISK_SCORE_DIR))
    args = parser.parse_args()

    risk_dir = Path(args.risk_score_dir)
    pr_df = pd.read_csv(risk_dir / "final_test_pr_table.csv")

    plt.figure(figsize=(8, 6))
    plt.plot(pr_df["recall"], pr_df["precision"])
    plt.xlabel("Recall")
    plt.ylabel("Precision")
    plt.title("Precision-recall curve: continuous risk score")
    plt.tight_layout()
    plt.savefig(risk_dir / "final_test_pr_curve.png", dpi=220)
    plt.close()

    print(f"Done: PR curve saved under {risk_dir}")


if __name__ == "__main__":
    main()

```

# 17. Minimal execution checklist

1. Put the new files into the existing `helpers/` folder.
2. Keep the old repo helper chain in place.
3. Rerun the current holdout baseline.
4. Run:
   - `python 12_plot_binary_tradeoff.py`
   - `python 13_summarize_threshold_stability.py`
   - `python 17_run_filter_ablation.py`
   - `python 19_build_review_artifact_pack.py`
5. Run outer-split pilot:
   - `python 14_run_outer_split_sensitivity.py --n-outer-splits 3`
   - `python 15_aggregate_outer_split_results.py`
6. If runtime is okay, push outer splits to 5.
7. If time and outputs are clean, run:
   - `python 20_compute_continuous_risk_scores.py`
   - `python 21_evaluate_continuous_risk_ranking.py`
   - `python 22_plot_pr_curve_risk_score.py`
8. Do **not** spend the week on full window sensitivity.
9. Strengthen the time-window **justification** in the writing this week.
10. Feed the new outputs back into the paper.

---

# 18. Final note

This file is intentionally long.

The point is simple:
Jasur should be able to stay inside one markdown file and understand:
- what the paper is,
- what matters this week,
- what to skip,
- what to run,
- why each step matters,
- and all the new code.

That is the whole point of this runbook.
