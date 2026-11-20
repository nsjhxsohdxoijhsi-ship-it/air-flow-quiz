# Airflow Quiz: Weekly Model Comparison DAG

An Astro project (Astro Runtime 12.1.1) with one Airflow DAG, written with the TaskFlow API, that runs a small ML workflow every week.

- **DAG ID:** `weekly_model_comparison`
- **DAG file:** `dags/weekly_model_comparison.py`
- **Schedule:** `@weekly`, start date 1 January 2026, `catchup=False`

## How to run

```bash
astro dev start
```

Open http://localhost:8080 (login `admin` / `admin`), unpause `weekly_model_comparison` and press **Trigger**. Stop with `astro dev stop`.

## Pipeline
preprocess_data ──► train_baseline ──┐
└─► train_candidate ──┴─► evaluate_models


- `preprocess_data` runs first and feeds both training tasks.
- `train_baseline` and `train_candidate` don't depend on each other, so they run in parallel.
- `evaluate_models` runs only after both training tasks succeed.
- Dependencies come only from passing task outputs as arguments (no `>>`, `set_upstream` or `set_downstream`).
