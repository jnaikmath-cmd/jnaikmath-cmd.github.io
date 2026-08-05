---
layout: post
title: "Why We Need EM: The Sum Inside the Log and the Dream of Complete Data"
date: 2026-03-09
description: "When standard MLE fails for Gaussian Mixture Models, Jensen's Inequality and the EM algorithm step in to save the day by 'completing' our data."
tags: [machine-learning, probability, math]
categories: blog
featured: true
---

If you have studied Machine Learning, you are likely intimately familiar with Maximum Likelihood Estimation (MLE). But as you transition from simple models to Gaussian Mixture Models (GMMs), MLE suddenly hits a mathematical brick wall. 

To solve it, we need the Expectation-Maximization (EM) algorithm. But why? Let's look at the math.

### The Baseline: Standard MLE
In standard MLE, we want to find the parameters $\theta$ that maximize the probability of observing our data $X$. We assume our data points are independent and identically distributed (i.i.d.), which allows us to write the likelihood as a product of individual probabilities.

To make the math easier, we take the log of this product, which elegantly turns into a sum:

$$\log P(X|\theta) = \sum_{i=1}^N \log P(x_i|\theta)$$

Taking the derivative of a sum of logs is easy. We set it to zero, and boom—we get a beautiful, closed-form solution for our parameters. 

### The Nightmare: GMMs and Incomplete Data
Now, let's look at a Gaussian Mixture Model. In a GMM, our data comes from $K$ different Gaussian distributions (clusters), but we don't know *which* cluster generated which point. This is a classic example of **incomplete data**.

Because a data point $x_i$ could belong to any of the $K$ clusters, we have to sum up the probabilities across all possible clusters *before* taking the log. The log-likelihood function becomes:

$$\log P(X|\theta) = \sum_{i=1}^N \log \left( \sum_{k=1}^K \pi_k \mathcal{N}(x_i | \mu_k, \Sigma_k) \right)$$

Look closely at that equation. **The sum is inside the log.** Algebraically, there is no way to simplify $\log(a + b)$. Because of this, our parameters ($\mu_k$, $\Sigma_k$, and the mixing weights $\pi_k$) are hopelessly tangled together inside the logarithm. If you take the derivative and set it to zero, you cannot isolate the variables. **There is no closed-form solution.**

### The Dream: Complete Data
To figure out a way around this, let's imagine a dream scenario. What if an oracle came down and told us exactly which cluster every single data point belonged to? 

Let's introduce a latent (hidden) variable, $Z$, where $z_i$ tells us the exact cluster assignment for $x_i$. If we have both $X$ and $Z$, we have **complete data**. 

Under this dream assumption, the likelihood function no longer needs to sum across all $K$ possibilities. It just picks the one correct cluster for each point:

$$\log P(X, Z | \theta) = \sum_{i=1}^N \left( \log \pi_{z_i} + \log \mathcal{N}(x_i | \mu_{z_i}, \Sigma_{z_i}) \right)$$

Notice what happened: the inner summation over $K$ is gone! The log is now applied directly to the individual probabilities. If we take the derivative of this complete data log-likelihood, it behaves exactly like standard MLE. We get perfectly clean, closed-form equations for our parameters.

### The Hero: Jensen’s Inequality
Unfortunately, we don't live in the dream scenario. We don't know $Z$. We only have our incomplete data $X$. 

We are stuck with the sum inside the log: $\log \left( \sum P(X, Z) \right)$. 

This is where our hero arrives: **Jensen's Inequality**. Jensen's Inequality states that for a concave function like a logarithm, the log of a sum is greater than or equal to the sum of the logs. 



By introducing an arbitrary probability distribution $q(Z)$ representing our guess for the cluster assignments, we can apply Jensen's Inequality to mathematically pull the summation *outside* of the logarithm:

$$\log \sum_{Z} q(Z) \frac{P(X, Z | \theta)}{q(Z)} \ge \sum_{Z} q(Z) \log \frac{P(X, Z | \theta)}{q(Z)}$$

This creates a strict lower bound (often called the Evidence Lower Bound, or ELBO). Because the log is now safely inside the summation and directly attached to $P(X, Z)$, the math becomes completely tractable.

### Solving the Problem: The EM Algorithm
Now that Jensen's Inequality has given us a solvable equation, the EM algorithm simply climbs the hill in two alternating steps:

1. **The E-Step (Completing the Data):** This is where the magic happens. Since we don't have the true complete data $Z$, we use our current (initially random) parameters to calculate $q(Z)$—the expected probability that point $x_i$ belongs to cluster $k$. **The E-step is literally the process of making our incomplete data complete.** It fills in the missing $Z$ values with probabilities. 
2. **The M-Step (Maximization):** Now that our data is "completed" by the E-step, we plug these expected values into our Jensen's lower bound. Because the bound mirrors our "Dream Scenario" equation, we can simply apply standard MLE! We take the derivative, set it to zero, and update our parameters using beautiful closed-form solutions.

We take those new parameters, feed them back into the E-step to get better cluster assignments, and repeat. Over iterations, our random guesses improve, the lower bound is pushed up, and we seamlessly bypass the mathematical roadblock of the sum inside the log!