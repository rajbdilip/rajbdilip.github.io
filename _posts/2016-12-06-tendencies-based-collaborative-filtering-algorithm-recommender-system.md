---
title: "Tendencies-based collaborative filtering algorithm"
date: 2016-12-06
layout: single
classes: wide
author_profile: false
read_time: true
categories:
  - machine-learning
  - recommender-systems
  - algorithms
tags:
  - collaborative-filtering
  - recommender
  - machine-learning
  - algorithm
mathjax: true
---
As part of my academic research project titled *Impact of Recommender System*, I got to study various collaborative filtering algorithms. I was supposed to study, implement, and compare them. Tendencies-based was the best among them in terms of accuracy and computational efficiency. It was proposed by Fidel Cacheda and his team of researchers from University of A Coruna in their paper titled ***Comparison of Collaborative Filtering Algorithms: Limitations of Current Techniques and Proposals for Scalable, High-Performance Recommender Systems.*** It was as accurate as other collaborative filtering algorithms like item-based, similarity fusion, and others, if not more accurate than them. It was the most computationally efficient.

## Algorithm
Tendencies-based algorithm, instead of looking for relations between users or items, looks at the differences between them.

Often, users with similar opinions rate items in a different way: some users mostly give positive ratings and rate really bad items negative while others usually rate negative and give positive ratings to the best items only. This algorithm deals with these variations using the concept of user tendency and item tendency.

### Notation
$r_{ui}$ denotes the rating given by user *u* to item *i*. $\hat{r}_{ui}$ denotes the prediction made by the algorithm for the rating of item *i* by user *u*. $\mu_u$ denotes user mean rating and $\mu_i$ denotes item mean rating. $I_u$ is the set of items rated by user *u*, and $U_i$ is the set of users who rated item *i*.

### Tendency Calculation
**Tendency of a user ($\tau_u$)** tells if a user tends to rate items positively. It is defined as the average difference between their ratings and the item mean.

$$
\tau_u = \frac{1}{|I_u|} \sum_{i \in I_u} (r_{ui} - \mu_i)
$$

**Tendency of an item ($\tau_i$)** refers to whether users consider it an especially good or especially bad item.

$$
\tau_i = \frac{1}{|U_i|} \sum_{u \in U_i} (r_{ui} - \mu_u)
$$

### Prediction Calculation
The algorithm defines four cases based on the signs of the user and item tendencies.

If both the user and the item have a positive tendency:

$$
\hat{r}_{ui} = \max(\mu_u + \tau_i, \mu_i + \tau_u)
$$

If both the user and the item have a negative tendency:

$$
\hat{r}_{ui} = \min(\mu_u + \tau_i, \mu_i + \tau_u)
$$

If the user has a negative tendency but the item has a positive tendency:

$$
\hat{r}_{ui} = \mu_u + \mu_i + \beta(\tau_u + \tau_i)
$$

If the user has a positive tendency but the item has a negative tendency:

$$
\hat{r}_{ui} = \mu_u + \mu_i + \beta(\tau_u + \tau_i)
$$

Here, $\beta$ is a parameter that controls the contribution of the item and user mean.

As observed, a simple formula is used in the four cases and the calculation is highly efficient: training time complexity is ***O(mn)*** and rating can be predicted in ***O(1)*** time.

[Implementation code can be downloaded from my GitHub repository.](https://github.com/rajbdilip/tendencies-based-recommender-system)
