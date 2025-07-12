# Unmasking Popularity Bias: A Study on Fairness Evaluation and Mitigation in Recommendation Systems

This repository contains the code and resources for the research project "Unmasking Popularity Bias: A Study on Fairness Evaluation and Mitigation in Recommendation Systems". The study investigates how popularity bias affects recommendation systems, evaluates its impact on different user groups, and assesses a mitigation strategy.

## 📜 Abstract

Recommendation systems are integral to navigating vast online content but often suffer from **popularity bias**: the tendency to over-recommend already popular items. This can harm user experience by limiting content discovery and creating fairness issues, especially for users with niche tastes. This study investigates popularity bias propagation and its fairness implications by:

1.  Conducting a focused reproducibility study in the **Music (Last.fm)** and **Movie (MovieLens 1M)** domains.
2.  Analyzing the impact of two key evaluation strategies (`UserTest`, `TrainItems`).
3.  Comparing three user grouping methods, including a novel **Niche Consumption Rate** approach.
4.  Evaluating bias using **Average Popularity of Recommended Items (%AGAP)** and accuracy using **NDCG@10**.
5.  Implementing and assessing a post-processing mitigation technique (**multiplicative damping, α=0.5**) to measure its effectiveness and its trade-off with accuracy.

Our findings confirm that the evaluation strategy profoundly influences measured bias. The mitigation strategy successfully reduces bias but at the cost of recommendation accuracy (NDCG@10).

## 🎯 Project Goals

This project aims to contribute to the understanding and mitigation of popularity bias by answering the following research questions:

1.  **Impact of Evaluation Strategy**: How significantly do different evaluation strategies (`UserTest` vs. `TrainItems`) affect the measurement of popularity bias and accuracy?
2.  **Effectiveness of User Grouping**: How do fairness patterns compare when users are grouped by popular item consumption versus our novel `NicheConsumptionRate`?
3.  **Mitigation Efficacy**: How effective is multiplicative damping at reducing measured popularity bias and narrowing fairness gaps between user groups?
4.  **Mitigation & Accuracy Trade-off**: What is the impact of this mitigation strategy on recommendation accuracy (NDCG@10)?

## ✨ Key Features & Contributions

* **Targeted Reproducibility Study**: Reproduces and extends the framework from Daniil et al. (2024) to analyze the interplay of evaluation strategy and user grouping.
* **Novel User Grouping Method**: Introduces and evaluates the `NicheConsumptionRate` method to directly identify users based on their affinity for niche items.
* **Integrated Mitigation Assessment**: Implements a post-processing mitigation technique and evaluates its effectiveness and trade-offs using a rigorous framework.
* **Cross-Domain Analysis**: Compares popularity bias effects in two distinct domains: Music and Movies.

## 🛠️ Methodology

The research was conducted in two main phases:

### Study 1: Baseline Reproducibility Study

1.  **Datasets**:
    * **Music**: [Last.fm-1b](https://www.dtic.upf.edu/~ocelma/MusicRecommendationDataset/lastfm-1K.html) subset (3,000 users, 12,690 artists).
    * **Movies**: [MovieLens 1M](https://grouplens.org/datasets/movielens/1m/) (6,040 users, 3,900 movies).
2.  **Algorithms**: A selection of 6 algorithms from the [Cornac](https://cornac.preferred.ai/) library were used:
    * `MostPop`
    * `UserKNN`
    * `ItemKNN`
    * `PMF`
    * `NMF`
    * `HPF`
3.  **Evaluation Strategies**:
    * `UserTest`: Ranks only items from the user's test set.
    * `TrainItems`: Ranks all items not seen by the user in the training set.
4.  **User Grouping Methods**: Users were segmented into Low, Medium, and High groups based on:
    * `PopularPercentage`: Percentage of popular items in their profile.
    * `AveragePopularity`: Average popularity of items in their profile.
    * `NicheConsumptionRate` (Novel): Percentage of niche items in their profile.

### Study 2: Bias Mitigation Study

This phase builds on Study 1 by applying a **multiplicative damping** post-processing technique to the recommendation scores before ranking. The goal is to reduce the influence of an item's popularity.

The formula for the new score is:
$$ \text{new\_score} = \frac{\text{original\_score}}{\text{item\_popularity}^{\alpha}} $$
For this study, we used a fixed **α = 0.5**.

## 📊 Key Results

1.  **Evaluation Strategy is Crucial**: The `TrainItems` strategy consistently revealed larger and more significant popularity bias compared to the `UserTest` strategy, confirming the findings of prior work.
2.  **Fairness Disparities**: Niche users consistently experience the most extreme shifts in the popularity of their recommendations and receive lower-quality recommendations (lower NDCG@10) when novel items are suggested.
3.  **Mitigation is a Trade-Off**: The multiplicative damping technique was highly effective at reducing the magnitude and disparity of popularity bias (%AGAP). However, this came at a distinct cost to ranking accuracy (NDCG@10), which generally decreased across all conditions.

| Domain | Algorithm | Baseline %AGAP (High Niche) | Mitigated %AGAP (High Niche) | Baseline NDCG@10 (High Niche) | Mitigated NDCG@10 (High Niche) |
| :--- | :--- | :---: | :---: | :---: | :---: |
| Music | MostPop | 641.1 | -36.5 | 0.159 | 0.025 |
| Music | HPF | -71.8 | 104.2 | 0.353 | 0.033 |
| Movies | MostPop | 240.7 | -37.5 | 0.083 | 0.333 |
| Movies | HPF | 81.7 | -91.3 | 0.512 | 0.039 |

*A summary of results for the High Niche group under the `TrainItems` evaluation strategy.*

## 퓨 Future Work

* Systematically tune the mitigation damping parameter `α`.
* Explore and compare other pre-processing and in-processing mitigation techniques.
* Expand the analysis to other domains (e.g., books, e-commerce) and newer deep learning-based algorithms.

## 🙏 Acknowledgements

This work builds upon the foundational research by Abdollahpouri et al. (2019) and the methodological investigations by Daniil et al. (2024). We thank the creators of the Cornac library and the maintainers of the MovieLens and Last.fm datasets.
