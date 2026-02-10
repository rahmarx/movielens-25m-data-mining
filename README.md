🎬 MovieLens-25M-Data-Mining: Sequential Behavioral Analysis

📌 Project Overview

Traditional recommendation engines often treat user preferences as static "snapshots" (e.g., "People who liked X also liked Y"). This project seeks to move beyond basic association by investigating the temporal evolution of user behavior. Using the MovieLens 25M dataset, I analyze not just what users watch, but the order and frequency with which they interact with different genres and franchises.

🎯 Core Objectives

Scalable Data Ingestion: Implementing robust, automated pipelines to handle 25 million records efficiently within a Jupyter environment.

Pattern Discovery: Utilizing Frequent Itemset Mining (FP-Growth) to establish baseline co-occurrence rules.

Temporal Dynamics: Applying Sequential Pattern Mining (PrefixSpan) to identify "watch paths" and the decay of interest in film sequels.

📊 Dataset: MovieLens 25M

The dataset, provided by GroupLens Research, represents one of the most comprehensive open-source movie rating archives available.

Size: ~250MB (Compressed), 650MB+ (Uncompressed CSVs).

Volume: 25,000,000 ratings across 62,000 movies.

Sparsity: High (The "Long Tail" effect where 10% of movies account for 90% of ratings).

Key Features: userId, movieId, rating, timestamp (Unix format).

🛠 Methodology & Roadmap

Phase 1: Exploratory Data Analysis (EDA)
Current focus is on data integrity and statistical distributions:

Rating Skew: Analyzing the concentration of 3.5–5.0 star ratings.

Engagement Tiers: Segmenting "Power Users" (1000+ ratings) vs. "Casual Viewers" (<20 ratings).

Temporal Gaps: Measuring the average time between a user's first and last rating.

Phase 2: Frequent Itemset Mining (Course Focus)
Implementing Association Rule Mining to identify high-confidence movie pairings.

Justifying support/confidence thresholds to filter out statistical noise in the "Long Tail".

Phase 3: Sequential Pattern Mining (Beyond-Course Focus)
Transitioning to the PrefixSpan algorithm to treat user watch-histories as ordered sequences.

Goal: Determine if watching a specific genre (e.g., Horror) predicts a "palate cleanser" watch (e.g., Comedy) in the subsequent sequence.
