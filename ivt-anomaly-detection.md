# IVT Statistical Analysis: Anomaly Detection for Traffic Quality

## Problem
Large-scale web traffic naturally exhibits volatility, making it difficult to distinguish legitimate fluctuations from potentially invalid or non-organic behavior. Mean-based monitoring approaches were overly sensitive to outliers, generating noisy alerts and making it hard to prioritize investigation effort.

The goal of this analysis was to develop a statistically robust, interpretable method for identifying abnormal traffic spikes that warrant review, while minimizing false positives from normal variability.

## Context & Data
- ~51M total ad requests analyzed globally over a two-week window (Jan)
- Analysis limited to geographies contributing at least 0.05% of total volume to ensure statistical relevance
- Daily aggregated request counts by country and U.S. state
- A secondary metric (fill rate) evaluated as a corroborating signal

A parallel analysis was conducted at both the **country** and **U.S. state** level to assess geographic concentration of risk :contentReference[oaicite:1]{index=1}.

## Approach
To account for skewed, heavy-tailed traffic distributions, the analysis relied on **robust, median-based statistics** rather than mean-driven thresholds.

Key steps included:
- Defining “normal” behavior using the **median daily request count** for each geography
- Measuring typical variability with the **Median Absolute Deviation (MAD)**, scaled to be comparable to standard deviation
- Computing a **MAD-scaled spike score** to quantify how extreme each geography’s largest single-day spike was relative to its baseline
- Classifying spikes using interpretable thresholds:
  - ~1 → normal variation  
  - ~3 → unusual spike  
  - ≥5 → extreme, likely non-organic behavior

This framework enabled consistent comparison across geographies with very different traffic volumes :contentReference[oaicite:2]{index=2}.

## Results / Impact
The analysis identified a clear clustering of abnormal traffic spikes within a narrow time window (January 12–14), indicating short-lived bursts rather than random fluctuation.

**Global findings:**
- Several countries exhibited extreme spike scores (≈8–18) and were flagged as **High Risk**
- Elevated risk was concentrated in a limited subset of geographies rather than broadly distributed
- Additional countries showed moderate anomalies and were classified in a **Review** range rather than escalated

**U.S. state findings:**
- State-level anomalies were limited in scope
- Only a small number of states exhibited short-term spikes well outside normal variability
- No evidence of widespread or systemic geographic issues within U.S. traffic

Overall, the framework enabled targeted investigation by narrowing attention to a small number of high-risk segments instead of treating volatility as uniformly suspicious :contentReference[oaicite:3]{index=3}.

## Validation & Tradeoffs
- Median and MAD were chosen for robustness under skewed, real-world distributions
- Spike clustering across adjacent days supported the interpretation of genuine bursts rather than noise
- Fill rate was evaluated as a corroborating metric but was not consistently depressed during spike periods, reinforcing the need for multi-signal interpretation
- Sensitivity for very low-volume geographies was intentionally reduced to avoid unstable signals

The approach prioritized **interpretability and operational usefulness** over more complex modeling techniques.

## What I’d Do Next
- Extend the framework to incorporate additional behavioral signals (e.g., CTR, viewability, user-agent diversity)
- Implement automated daily monitoring and alerting based on spike score thresholds
- Explore seasonality-aware baselines for longer time horizons
