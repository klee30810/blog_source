---
title: "Chap1. Introduction & Motivation"
description: "Introduction & Motivation"
menuTitle : "Motivation"
weight: 1
date: 2021-03-15T11:15:46+09:00
draft: false
katex: true
markup: mmark
tags: ["Machine Learning"]
hidden: true

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

- We believe that the mathematical foundations of machine learning are important in order to understand fundamental principles upon which more complicated machine learning systems are built.

### 1.1 Finding Words for Intuitions

---

- machine learning algorithm (predictor) : a system that makes predictions based on input data

- training : a system that **adapts** some internal parameters of the predictor so that it performs well on future unseen input data

- data as vectors : an array of numbers, an arrow with a direction and magnitude, an object that obeys addition and scaling

- model : a process for generating data

  → a simplified versions of the real (unknown) data-generating process, **capturing aspects that are relevant for modeling the data and extracting hidden patterns from it**

  → good model can be used to predict what would happen in the real world without performing real-world experiments

- Learning : Training the model means to use the data available to optimize some parameters of the model with respect to a utility function that evaluates how well the model predicts the training data.



### 1.2 Two Ways to Read the Book

---

- Part 1. Mathematics : linear algebra, analytic geometry, matrix decomposition, probability theory, vector calculus, optimization

![image](/images/mldl/mathml/chap1/1-1.png)

- Part 2. Machine Learning : linear regression, dimensionality reduction, density estimation, classification