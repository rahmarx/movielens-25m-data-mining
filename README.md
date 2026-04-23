# 🎬 MovieLens-25M-Data-Mining: Does Order Matter?

Every modern recommender system — Netflix, Spotify, YouTube — is built on the assumption that the ORDER you consume things in contains useful signal. This project tests that assumption directly. Using the MovieLens 25M dataset, I ran two data mining methods head-to-head: **Apriori**, which treats each user's history as an unordered basket, and **PrefixSpan**, which treats it as a time-ordered sequence. Same data, same support threshold, one concrete downstream task. The result was a clean negative finding with a grounded explanation — and a useful lesson about what sequence mining actually requires from your data.

---

## 🎥 Project Video

👉 [Watch the 2-minute project pitch here]((https://www.youtube.com/watch?v=lSR_DlJppf0))

---

## 📓 Main Deliverable

👉 The main deliverable is **`notebooks/main_notebook.ipynb`**

This notebook tells the full story: motivation, methods, asymmetry analysis, case studies, prediction evaluation, limitations, and conclusion. Read this one.

---

## ❓ Research Question

> **Does the temporal ordering of movie ratings contain pattern-mining signal that an unordered basket representation throws away?**

Frequent-itemset mining treats each user's history as an unordered basket — it knows *what* you watched but not *when*. Sequential pattern mining treats the same history as an ordered sequence and uses the full temporal information. The research question is whether that extra information actually helps, tested on a real downstream task: predicting a user's next watch.

---

## 🔑 Results Summary

On a 10,000-user sample of MovieLens 25M, **the unordered model wins**: Apriori achieves Hit@10 = 0.110 vs PrefixSpan's 0.090, and MRR@20 of 0.047 vs 0.037. The ordering signal in the data is real, but it reflects release-date artifacts rather than genuine viewing behavior — because MovieLens records when users *rate* movies, not when they *watch* them. The full analysis, case studies, and honest discussion of why this happens live in the notebook.

---

## 📊 Dataset

[MovieLens 25M (GroupLens Research)](https://grouplens.org/datasets/movielens/25m/)

- **Volume:** 25,000,095 ratings across 62,423 movies from 162,541 users
- **Time range:** 1995-2019
- **Files used:** `ratings.csv` (userId, movieId, rating, timestamp) and `movies.csv` (movieId, title, genres)
- **License:** Non-commercial research/education

The notebook downloads and extracts the dataset automatically on first run via the `hydrate_movielens()` function. No manual download needed.

**Preprocessing steps (all inside the notebook):**
- Sample 10,000 users at random (seed 42)
- Keep the top 500 most-rated movies in the sample
- Cap each user's sequence at their 40 earliest ratings
- Drop users with fewer than 5 ratings after filtering
- Minimum support threshold: 5% of users

---

## 🗂 Repo Structure

```
.
├── README.md
├── LICENSE
├── .gitignore
├── requirements.txt
└── notebooks/
    ├── checkpoint_1.ipynb     <- Checkpoint 1: dataset selection + EDA
    ├── checkpoint_2.ipynb     <- Checkpoint 2: research questions + method plan
    └── main_notebook.ipynb    <- Final deliverable (start here)
```

---

## ▶️ How to Reproduce

This project was built and tested in **Google Colab**. To run it locally, clone the repo and install the requirements:

```bash
git clone https://github.com/rahmarx/movielens-25m-data-mining.git
cd movielens-25m-data-mining
pip install -r requirements.txt
jupyter notebook notebooks/main_notebook.ipynb
```

Then select **Run All**. The notebook will download MovieLens 25M on the first run (~250 MB, one-time) and complete end-to-end in roughly 5 minutes on a laptop.

To run in **Google Colab**, open the notebook directly from GitHub via File > Open notebook > GitHub tab, or upload it manually and run all cells.

**Run order:** the checkpoints are standalone. For the full project, only `main_notebook.ipynb` needs to be run.

---

## 📦 Key Dependencies

| Package | Version |
|---|---|
| Python | 3.11 |
| pandas | 2.2.0 |
| numpy | 1.26.4 |
| matplotlib | 3.8.3 |
| seaborn | 0.13.2 |
| efficient-apriori | 2.0.6 |
| prefixspan | 0.5.2 |

Full list of dependencies is in `requirements.txt`.
