# JoinQuant Multi-Model Research & Ensemble Strategy

This project is designed for the **JoinQuant (聚宽) Research & Backtesting environment** and relies on JoinQuant-specific APIs.

---

## Project Overview

This codebase contains two main components:

### 1. Research / Training Module
- Builds a **multi-model machine learning framework**
- Pulls factor data using `jqfactor.get_factor_values`
- Trains multiple models:
  - LightGBM
  - XGBoost
  - Random Forest
  - SVR
  - Linear Regression
- Produces an **ensemble model**
- Saves trained artifacts locally:
  - `multi_model_selected_features.pkl`
  - `multi_model_ensemble.pkl`
  - `multi_model_ensemble.txt` (Base64 backup)

### 2. Trading Strategy / Backtest
- Standard JoinQuant strategy structure using:
  - `initialize(context)`
  - global parameters (`g.stock_num`, `g.model_type`, etc.)
- Uses trained models to select stocks periodically
- Designed to be run directly in JoinQuant's **Strategy Backtest** interface

---

## Environment Requirements

This project **must be run inside JoinQuant**.

Required JoinQuant permissions:
- Research Notebook access
- Factor data access (`jqfactor`)
- Strategy backtesting access

Required Python packages (typically preinstalled in JoinQuant):
- `pandas`
- `numpy`
- `tqdm`
- `scikit-learn`
- `lightgbm`
- `xgboost`

> ⚠️ This project will **not run in a standard local Python environment** because `jqdata` and `jqfactor` are JoinQuant-only modules.

---

## How to Run — Research / Training

### Step 1: Open JoinQuant Research
1. Log in to JoinQuant
2. Go to **研究 (Research)** → create or open a Notebook
3. Upload or paste the research code (the notebook containing `MultiModelResearch`)

### Step 2: Run Training
At the bottom of the notebook, execute:

```python
test_multi_model()
```

This will:
- Fetch factor data
- Train all models
- Build the ensemble
- Save model artifacts

### Key Parameters You May Adjust
Inside the training code:
- `start_date` / `end_date`
- `period = 'M'` (monthly)
- `stock_pool` (e.g. `ZXBZ`)
- Factor list

---

## Output Files

After successful training, the following files will be generated:

```
multi_model_selected_features.pkl
multi_model_ensemble.pkl
multi_model_ensemble.txt
```

These files are used by the strategy module.

---

## How to Run — Strategy Backtest

1. Go to **策略 (Strategy)** in JoinQuant
2. Create a new strategy
3. Paste the strategy code (starting with `initialize(context)`)
4. Click **回测 (Backtest)**

### Strategy Parameters
Inside `initialize(context)`:
- `g.stock_num = 10` — number of holdings
- `g.model_type = 'ensemble'`
- `g.factor_list` — factors used for scoring

---

## Running Locally (Not Supported)

Local execution is **not supported** because JoinQuant APIs are unavailable outside the platform.

Installing packages locally will not resolve this:
```bash
pip install pandas numpy scikit-learn lightgbm xgboost
```

---

## Common Issues

- **ImportError: jqdata / jqfactor**
  - You are not running inside JoinQuant

- **Permission denied for factor data**
  - Your JoinQuant account lacks data permissions

- **Model package missing**
  - Enable LightGBM / XGBoost in JoinQuant if available

---

## Recommended Workflow

1. Run model training in JoinQuant Research
2. Verify model artifacts are saved
3. Run strategy backtest using the trained ensemble
4. Tune factors, dates, and portfolio size

---

## Author Notes

This framework is suitable for:
- Factor research

- Model comparison
- Ensemble stock selection
- Academic or production-style quant research
