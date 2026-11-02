---
layout: page
title: Neural collaborative filtering for anime recommendation
description: Learned user and item embeddings for similar-item, similar-user and personalized recommendations.
importance: 4
category: projects
github: https://github.com/Shahriarmsakib/Recommender-System-for-Anime-Recommendations
---

**Code:** [GitHub](https://github.com/Shahriarmsakib/Recommender-System-for-Anime-Recommendations) · 2023–2024

This is an embedding-based collaborative-filtering recommender trained on tens of millions of user–anime ratings from the Anime Recommendation Database 2020.

- **Model:** user and anime embeddings combined by a normalized dot product, followed by a dense layer, batch normalization and a sigmoid. It is trained on min-max-scaled ratings and reaches a validation MAE of about 0.19 on the [0, 1] scale.
- **Similar titles:** nearest neighbors in the anime-embedding space.
- **Similar users:** nearest neighbors in the user-embedding space.
- **Personalized recommendations:** drawn from what a user's nearest neighbors rated highly, alongside a profile of the user's favorite genres.
