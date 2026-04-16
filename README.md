# 🍽️ Restaurant Recommendation System

This project builds a **hybrid recommendation system** for a restaurant using a combination of:

* Data Mining techniques
* Statistical analysis
* Collaborative filtering
* Deep learning (Transformer model)

The goal is to predict the **Top-10 items** for each user request.

---

# 🧠 Project Idea

Instead of relying on a single model, this system combines multiple approaches:

👉 Traditional methods (rules + similarity + CF)
👉 Statistical insights (context + time)
👉 Deep learning (Transformer)

All signals are combined into a **final scoring system**.

---

# ⚙️ Full Pipeline

```
Raw Data
   ↓
Data Preprocessing
   ↓
Feature Engineering + Recency Weights
   ↓
Time-Based Split (Train / Validation)
   ↓
Signal Generation (Multiple Models)
   ↓
Scoring & Ranking System
   ↓
Top-10 Predictions
   ↓
Evaluation (NDCG@10)
```

---

# 📊 Data Processing

* Convert timestamps to datetime
* Sort data chronologically
* Remove duplicates
* Extract features:

  * hour
  * day_of_week
  * is_weekend
  * meal_period
* Parse baskets into lists
* Apply **recency weighting**:

  * recent orders = higher importance

---

# 🗃️ Data Representation

* Frequency Matrix (user × item)
* Recency-weighted Matrix
* Binary Matrix
* User vectors representing:

  * preferences
  * behavior
  * meal patterns

---

# 📐 Similarity & Relationships

* Item-item similarity (Cosine + Jaccard)
* User-user similarity
* Item relationships based on co-purchases

---

# 🤝 Collaborative Filtering

* Item-based CF → recommends similar items
* User-based CF → recommends from similar users

---

# 📦 Pattern Mining

* FP-Growth to find frequent itemsets
* Association rules:

  * Example: `{wrap, fries} → drink`
* Closed & maximal itemsets to reduce noise

---

# 📈 Statistical Insights

* Chi-square test → detects relationships
* Correlation analysis
* Context-based popularity:

  * meal period
  * weekend vs weekday

---

# 🤖 Deep Learning Model

Masked Basket Transformer (BERT-style):

* Learns relationships inside baskets
* Predicts missing items
* Uses:

  * embeddings
  * self-attention
  * contextual tokens

---

# 🎯 Scoring System

All models are combined into a final score:

* Transformer
* Item similarity
* Collaborative filtering
* Association rules
* Context popularity
* Recency

### Process:

1. Generate candidate items
2. Score them using multiple signals
3. Re-rank using Transformer

---

# 📏 Evaluation

Metric used:

👉 **NDCG@10**

Measures ranking quality — higher is better.

---

# 📊 Results

* Baseline: **0.6441**
* Best validation: **0.7429**
* Best leaderboard: **0.7449**
* Latest submission: **0.7302**

---

# 💡 Key Insight

The performance improvement comes from:

👉 Combining multiple models
👉 Not relying on a single method

---

# 🚀 How to Run

1. Open the notebook in Jupyter
2. Place all CSV files in the same folder
3. Run cells in order
4. Generate submission file

---

# 👩‍💻 Notes

* The system uses **implicit feedback** (no ratings)
* Recency plays a major role
* Ensemble weighting is critical for performance

---
