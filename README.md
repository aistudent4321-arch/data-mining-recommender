# 🍽️ Restaurant Recommendation System – Project Summary

This project builds a complete recommendation system for a restaurant using multiple data mining and machine learning techniques.

---

## 📊 Data Processing

In this stage, the raw data is cleaned and transformed into a usable format:

* **Timestamp parsing**: Convert string dates into datetime objects
* **Sorting**: Order all data chronologically
* **Duplicate removal**: Remove repeated rows
* **Feature engineering**:

  * hour
  * day_of_week
  * is_weekend
  * meal_period
* **Basket parsing**: Convert strings like `WRAP001|SAUCE001` into lists
* **Recency weighting**:

  * Formula: exponential decay
  * Recent interactions are more important
* **Time-based split**:

  * Last 10% of data used for validation

---

## 🗃️ Data Representation

Different matrices were built to represent user behavior:

* **Frequency matrix**: How many times a user bought each item
* **Recency-weighted matrix**: Gives higher importance to recent purchases
* **Binary matrix**: 1 if user bought item, 0 otherwise
* **User vectors**:

  * Average price
  * Food preferences
  * Meal behavior
  * Category ratios

---

## 📐 Similarity Methods

Used to find relationships between items and users:

* **Cosine similarity (item-item)**: Based on co-purchase behavior
* **Jaccard similarity (item-item)**: Based on binary overlap
* **Cosine similarity (user-user)**: Based on behavioral features

---

## 🤝 Collaborative Filtering

Two types of recommendation methods:

* **Item-based CF**:

  * Recommends items similar to what the user already bought
* **User-based CF**:

  * Finds similar users and recommends what they liked

---

## 📦 Frequent Pattern Mining

Used to discover patterns in baskets:

* **FP-Growth algorithm**:

  * Finds frequently purchased item combinations
* **Association rules**:

  * Example: `{burger, fries} → drink`
  * Uses:

    * support
    * confidence
    * lift
* **Closed itemsets**:

  * Removes redundant patterns
* **Maximal itemsets**:

  * Keeps only strongest patterns

---

## 📈 Statistical Analysis

Used to understand relationships in the data:

* **Chi-square test**:

  * Tests relationships between:

    * meal_period
    * hour
    * weekend
* **Correlation analysis**:

  * Measures relationships (e.g., price vs time)
* **Context popularity**:

  * Popular items per meal period
* **Weekend popularity**:

  * Different behavior on weekends

---

## 🧠 Deep Learning Model (Transformer)

A BERT-style model was used to predict missing items in a basket:

* Embedding layer (128 dimensions)
* Positional encoding
* Transformer Encoder (2 layers)
* Multi-head self-attention
* Context tokens (e.g., `<LUNCH>`, `<WEEKEND>`)
* Multi-mask training
* Label smoothing
* Gradient clipping
* Learning rate scheduler
* Early stopping

---

## 🎯 Scoring & Ranking System

All models are combined into one final recommendation system:

* **Normalization**:

  * Softmax (for dense signals)
  * Min-max (for sparse signals)
* **Weighted ensemble**:

  * Combines multiple signals:

    * transformer
    * similarity
    * collaborative filtering
    * association rules
    * popularity
    * recency
* **Basket bonus**:

  * Adds score if rule matches basket
* **Two-stage pipeline**:

  1. Generate top 20 candidates
  2. Re-rank using transformer

---

## 📏 Evaluation

* Metric used: **NDCG@10**
* Measures ranking quality (higher = better)

---

## 🚀 Final Idea

This system does NOT rely on a single model.

Instead, it combines:

* Traditional methods (rules, similarity, CF)
* Statistical analysis
* Deep learning (Transformer)

👉 The final performance depends on how well these components are combined.

---
