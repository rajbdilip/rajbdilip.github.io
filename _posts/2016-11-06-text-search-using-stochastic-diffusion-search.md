---
title: "Text search using Stochastic Diffusion Search"
date: 2016-11-06
layout: single
classes: wide
excerpt: "Overview of Stochastic Diffusion Search and a simple algorithm outline for text search."
author_profile: false
read_time: true
categories:
  - algorithms
  - search
  - optimization
tags:
  - stochastic-diffusion-search
  - search
  - optimization
  - java
  - algorithms
header:
  image: /assets/images/posts/2016-11-06-stochastic-diffusion-search.svg
  teaser: /assets/images/posts/2016-11-06-stochastic-diffusion-search.svg
---
Stochastic Diffusion Search (SDS), a multi-agent population-based global search and optimization algorithm, is a distributed mode of computation utilizing interaction between simple agents. SDS shows off a strong mathematical framework. It is robust, has minimal convergence criteria and linear time complexity.

SDS has been applied to diverse problems such as text search, object recognition, feature tracking, mobile robot self-localization and site selection for wireless networks. As a part of my *Optimization Techniques* laboratory project, I implemented a text search using SDS.

## Basic SDS Algorithm

SDS algorithm has many variations. The following is the basic version.

```
1. For all agents do
2. INITIALIZE: Agent picks a random hypothesis 
3. TEST: Agent partially evaluates her hypothesis 
- If test criterion = TRUE, agent = Active (satisfied) 
- Else agent = Inactive (dissatisfied) 
4. DIFFUSE
- Inactive agent meets a randomly chosen agent 
- Inactive agent updates/changes hypothesis 
5. REPEAT until Halting criterion.
```

## SDS Text Search Algorithm

```
INITIALIZATION PHASE
Each agent selects a haystack offset (hypothesis) at random.
WHILE (NOT all agents are active)
TESTING PHASE
Each agent randomly selects an offset less than or equal to the length of needle (needle offset) and matches the character in haystack at (haystack offset + needle offset) with the character in needle at needle offset
IF the letters match
Agent is active.
ELSE
Agent is inactive.
DIFFUSION PHASE
Each inactive agent selects another agent at random.
IF the selected agent is active
The inactive agent adopts the hypothesis (index) of that agent.
ELSE
The inactive agent selects a new index (hypothesis) at random.
END WHILE
Each agent's haystack offset is the starting index of needle.
```

## Implementation

The following link links to the Java implementation of above algorithm on GitHub.

[https://github.com/rajbdilip/stochastic-diffusion-search](https://github.com/rajbdilip/stochastic-diffusion-search)

## Observation

Stochastic Diffusion Search text search is linear and fast. Agents initially search, finish communicating with each other, and hence finish searching in very few iterations, i.e., around 100 iterations with 5 agents in the haystack of length around 500. For smaller increments in the number of agents, the number of iterations required to converge to the solution decreases. But it cannot be ensured that big increments will significantly reduce the number of iterations. The number of iterations can rather increase.

However, if the needle is present in more than one offset, it cannot be guaranteed that this algorithm will find all of the occurrences regardless of the number of agents used. Since SDS is random, a different offset is returned every time.

## References

al-Rifaie, Mohammad Majid, and John Mark Bishop. "Stochastic diffusion search review." *Paladyn, Journal of Behavioral Robotics* 4.3 (2013): 155-173.
