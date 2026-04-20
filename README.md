# 🎬 MovieLens-25M-Data-Mining: Does Order Matter?

A head-to-head comparison of **FP-Growth / Apriori** (unordered frequent-itemset mining) and **PrefixSpan** (sequential pattern mining) on the MovieLens 25M dataset. The project was built for the final deliverable of a Data Mining & Analysis course and consolidates work from two earlier checkpoints into a single focused analysis.

---

## 📌 The question

Frequent-itemset mining treats each user's history as an **unordered basket**. It knows *what* you watched but not *when*. Sequential pattern mining treats the same history as an **ordered sequence** and uses the full temporal information. The textbook pitch for PrefixSpan is essentially *"it's like Apriori, but with ordering"*.

So:

> **Does the temporal ordering of movie ratings actually contain pattern-mining signal that an unordered basket representation throws away?**

This project puts both methods on the same data, at the same support threshold, and then uses a concrete downstream task (next-watch prediction) to decide which one does better.

---

## 🔑 TL;DR of the findings

The result surprised me. After running Apriori and PrefixSpan head-to-head on a 10,000-user sample of MovieLens 25M:

- **Roughly half of the frequent length-2 co-watchings are strongly asymmetric** in PrefixSpan's ordered view, but those asymmetries turn out to be **release-date artifacts** of the MovieLens dataset, not genuine narrative-sequence behavior. The top asymmetric pairs are 1993-1995 films, not franchise sequels.
- **On next-watch prediction, the unordered model wins.** Apriori hits Hit@10 = 0.110 vs PrefixSpan's 0.090. MRR@20 is 0.047 for Apriori vs 0.037 for PrefixSpan.
- **The honest conclusion is that on this data, with this task, the ordering information doesn't help.** MovieLens records rating timestamps, not viewing timestamps, and the "sequence" PrefixSpan picks up turns out to be mostly an artifact of when each movie became ratable.

The full narrative, with case studies and honest limitations, lives in the notebook.

---

## 📊 Dataset

[MovieLens 25M (GroupLens Research)](https://grouplens.org/datasets/movielens/25m/)

- **Size:** ~250 MB compressed, ~650 MB uncompressed
- **Volume:** 25,000,095 ratings across 62,423 movies from 162,541 users
- **Time range:** 1995-2019
- **Schema used:** `ratings.csv` (userId, movieId, rating, timestamp) and `movies.csv` (movieId, title, genres)
- **License:** Non-commercial research/education

The notebook downloads the dataset automatically on first run.

---

## 🛠 Methods

| Stage | Algorithm | Role |
|---|---|---|
| Frequent Itemset Mining | **Apriori** (via `efficient-apriori`) | Unordered baseline. Extracts frequent baskets and association rules. **Course method.** |
| Sequential Pattern Mining | **PrefixSpan** (via `prefixspan`) | Ordered view. Extracts frequent subsequences from time-ordered rating histories. **External / beyond-course method.** |
| Comparison | Custom asymmetry metric | For each length-2 itemset, quantifies how much probability mass sits in the dominant ordering vs the reverse. |
| Evaluation | Next-watch hold-out | Hides each user's last rated movie, refits both methods on training portion, compares Hit@10 / Hit@20 / MRR@20 against a popularity baseline. |

**Why `efficient-apriori` instead of `mlxtend.fpgrowth`:** the standard course library was ~100x slower than `efficient-apriori` at this data shape, to the point of being unusable for length-3 mining. The two libraries produce identical itemsets at the same support threshold. Runtime details are in Section 4 of the notebook.

---

## 🗂 Repository contents

```
.
├── README.md                            <- this file
├── notebooks/
│   ├── checkpoint_1.ipynb     <- Checkpoint 1: dataset selection + initial EDA
│   ├── checkpoint_2.ipynb     <- Checkpoint 2: research questions + method plan
│   └── final_project.ipynb    <- Final deliverable (this is the one to read)
```

---

## ▶️ Reproducibility

### Requirements

```
python >= 3.9
pandas
numpy
matplotlib
seaborn
efficient-apriori
prefixspan
requests
```

### Install and run

```bash
# Clone
git clone https://github.com/rahmarx/movielens-25m-data-mining.git
cd movielens-25m-data-mining

# Install (recommended: a fresh virtualenv)
pip install pandas numpy matplotlib seaborn efficient-apriori prefixspan requests jupyter

# Launch
jupyter notebook notebooks/637006715_final_project.ipynb
```

Then "Run All". The notebook will download MovieLens 25M on the first run (~250 MB, one-time) and run end-to-end in roughly 5 minutes on a laptop.

### Key parameters

Set at the top of the sampling section of the final notebook. Tweak these to reproduce larger or smaller runs.

| Parameter | Value | Note |
|---|---|---|
| `SAMPLE_USERS` | 10,000 | ~6% of the user base |
| `TOP_K_MOVIES` | 500 | Top-500 most-rated movies in the sample |
| `MAX_SEQ_LEN` | 40 | Cap each user's sequence at 40 earliest ratings |
| `MIN_USER_MOVIES` | 5 | Drop very-light users |
| `MIN_SUPPORT` | 0.05 | 5% of users |
| Random seed | 42 | Set for user sampling |

---

## 📝 Project checkpoints

This repo captures the full arc of the project:

- **Checkpoint 1** (dataset selection + EDA): compared three candidate datasets (MeTooMA tweets, MovieLens 25M, CorDGT dynamic graphs), selected MovieLens 25M, and ran initial EDA covering rating distribution, movie popularity long-tail, and user engagement tiers.
- **Checkpoint 2** (research questions): proposed three research questions. For the final project these were collapsed into one focused question per the professor's "don't do a hodge-podge" guidance.
- **Final project**: this notebook. One question, one clear answer, deep analysis, honest conclusion.
