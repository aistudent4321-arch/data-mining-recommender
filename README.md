# Restaurant Recommendation System

## Overview

This project was developed for the AI306 Data Mining Restaurant Recommendation Challenge.

The objective is to predict the next item a customer is most likely to add to their current basket and generate a ranked Top-10 recommendation list.

The final solution uses a LightGBM LambdaRank learning-to-rank model combined with extensive feature engineering, dynamic user-history modeling, contextual popularity statistics, and a 5-seed ensemble.

---

#  Competition Results

| Metric                 | Score      |
| ---------------------- | ---------- |
| Best Private Score     | **0.7866** |
| Best Public Score      | **0.7780** |
| Final Leaderboard Rank | **#2**     |

---

# Problem Statement

Given:

* User information
* Current basket items
* Temporal context (meal period, hour, weekend)

The system predicts which menu item the customer is most likely to add next.

This is formulated as a **Learning-to-Rank (LTR)** problem rather than a classification problem.

For every basket, the model ranks all candidate menu items and returns the Top-10 recommendations.

The competition evaluation metric is **NDCG@10**.

---

# Dataset

The project uses five datasets:

| File                   | Description                        |
| ---------------------- | ---------------------------------- |
| train_transactions.csv | Transaction-level purchase history |
| train_baskets.csv      | Basket-level order data            |
| train_users.csv        | User metadata                      |
| menu_items.csv         | Menu catalog containing 50 items   |
| test_instances.csv     | Competition prediction instances   |

Dataset period:

* Training: January 2024 – July 2025
* Testing: July 2025 – September 2025

The menu contains:

* 50 items
* 9 categories

Including:

* Wraps
* Plates
* Highlanders
* Appetizers
* Salads
* Sides
* Beverages
* Sauces
* Desserts

---

# Model

## LightGBM LambdaRank

The final solution uses LightGBM with the LambdaRank objective.

LambdaRank is specifically designed for ranking tasks and directly optimizes NDCG during training.

Why LambdaRank?

* Competition metric is NDCG@10
* Output is a ranked list, not a class label
* Ranking quality matters more than classification accuracy

Why LightGBM?

* Fast training
* Handles large feature sets efficiently
* Built-in LambdaRank support
* Strong performance on tabular data

---

# Hyperparameters

```python
LGBM_PARAMS = {
    "objective": "lambdarank",
    "metric": "ndcg",
    "ndcg_eval_at": [10],
    "boosting_type": "gbdt",
    "learning_rate": 0.05,
    "num_leaves": 63,
    "min_data_in_leaf": 200,
    "feature_fraction": 0.85,
    "bagging_fraction": 0.85,
    "bagging_freq": 5,
    "lambda_l2": 1.0,
    "label_gain": [0, 1]
}
```

Additional settings:

```python
ENSEMBLE_SEEDS = [1337, 2024, 314159, 7, 42]
NUM_BOOST_ROUND = 3000
EARLY_STOPPING = 150
RECENCY_HALFLIFE_DAYS = 60
```

---

# Feature Engineering

The system uses approximately 90 engineered features.

Feature groups include:

### Item Features

* Price
* Calories
* Popularity weight
* Category
* Protein type
* Vegetarian / Vegan / Spicy flags

### Context Popularity Features

* Global popularity
* Meal-period popularity
* Day-of-week popularity
* Hour-bucket popularity
* Weekend popularity

### User History Features

* Purchase counts
* Recency-weighted counts
* User preferences
* Favorite items
* Purchase frequency

### Category Preference Features

* Category affinity
* Top category
* Category purchase counts

### Last Basket Features

* Previously ordered items
* Basket recurrence
* Days since last basket

### Co-occurrence Features

* Conditional probabilities
* Pair counts
* Log-lift
* PPMI
* Item similarity

### Collaborative Signals

* Item-based similarity scores
* User-item interaction signals

### Position Features

* Average item position
* Relative basket position
* Basket size

### Basket Composition Features

* Main course indicators
* Beverage indicators
* Sauce indicators
* Add-on indicators

---

# User Modeling

User history is built dynamically for every training and validation sample.

Only baskets that occurred before the sample timestamp are used.

This prevents temporal leakage and ensures realistic recommendation behavior.

The system models:

* User preferences
* Purchase frequency
* Recency effects
* Category preferences
* Repeat-order behavior

---

# Training Pipeline

```text
Raw Data
    ↓
Data Loading
    ↓
Statistics Construction
    ↓
User History Building
    ↓
Feature Engineering
    ↓
Dataset Generation
    ↓
LightGBM LambdaRank Training
    ↓
Validation
    ↓
Ensemble Prediction
    ↓
Submission File
```

Training process:

1. Generate candidate items
2. Build feature vectors
3. Create X_train
4. Create y_train
5. Create groups_train
6. Train LightGBM rankers
7. Ensemble predictions from 5 seeds
8. Generate Top-10 recommendations

---

# Evaluation

Metric used:

## NDCG@10

Normalized Discounted Cumulative Gain at rank 10.

NDCG rewards placing relevant items higher in the recommendation list.

Higher values indicate better ranking quality.

Additional metrics:

* Hit@10
* Hit@1

---

# Key Technical Contributions

The largest performance gains came from:

* Dynamic per-sample user history
* Context-aware popularity features
* Triplet conditional probabilities
* Similar past-basket completion
* Position-aware features
* Basket composition features
* Rank-averaged 5-seed ensemble

---

# Repository Structure

```text
reco-challenge/
├── scripts/
│   ├── run_pipeline.py
│   └── hp_sweep.py
├── src/
│   ├── config.py
│   ├── data_io.py
│   ├── stats.py
│   ├── user_history.py
│   ├── features.py
│   ├── dataset_builder.py
│   ├── ranker.py
│   └── evaluate.py
├── outputs/
├── requirements.txt
└── README.md
```

---

# Installation

```bash
pip install -r requirements.txt
```

---

# Running the Project

```bash
python scripts/run_pipeline.py
```

Force rebuild:

```bash
python scripts/run_pipeline.py --force
```

---

# Team TSJ

* Tala Mamdouh Khushaym
* Judy Jamal Farghal
* Sama Ashraf Osailan

---

AI306 Data Mining Project — Spring 2026
