---
layout: page
title: Room occupancy estimation from sensor data
description: Counting occupants from non-intrusive environmental sensors on a heavily imbalanced dataset.
importance: 3
category: projects
github: https://github.com/Shahriarmsakib/Room-Occupancy-Estimation
---

**Code:** [GitHub](https://github.com/Shahriarmsakib/Room-Occupancy-Estimation) · Graduate team research project, UC Riverside, 2023

The task is to estimate how many people (0–3) are in a room from non-intrusive environmental sensors: temperature, light, sound, CO₂ and passive-infrared motion, recorded by several sensor nodes ([UCI Room Occupancy Estimation dataset](https://archive.ics.uci.edu/dataset/864/room+occupancy+estimation)). The empty-room class dominates, so the class imbalance has to be handled explicitly and per-class F1 matters more than accuracy.

#### Approach

- **Exploratory analysis:** feature correlations and class balance.
- **Imbalance handling:** majority-class undersampling.
- **Representations:** raw features compared with PCA, LDA, t-SNE, factor analysis, truncated SVD and ICA projections.
- **Models:** logistic regression, Naive Bayes, decision trees, SVMs and a feed-forward neural network, each compared with off-the-shelf implementations.

#### Results

LDA was the most effective representation: logistic regression on LDA features reached 0.99 accuracy and F1. A decision tree reached 0.996 test accuracy.
