---
title: "Opening the Lab Notebook"
date: 2026-04-16
categories:
  - research
  - methodology
---

Every experiment begins with a prior. Ours: that rigorous, uncertainty-aware models
outperform ad-hoc heuristics on every time horizon that matters—and that the
gap widens as problem complexity increases.

This site is the public record of that thesis. We document methodology, publish
benchmark results, and release tooling under open-source licenses when the IP
exposure is low and the community benefit is high.

---

### The Problem with Standard ML in Science

Scientific domains impose constraints that generic pipelines ignore:

- **Extrapolation matters.** A model fit to 80% of the operating envelope
  must be reliable at the boundaries, not just the center.
- **Data is expensive.** In formulation work or physical experiments,
  each observation costs real time, real materials, real money.
- **Uncertainty is the product.** Telling a scientist "the answer is 4.7"
  is less useful than "the answer is $4.7 \pm 0.3$ with 95% credible interval,
  and here is what you should measure next to reduce that interval fastest."

---

### Formulation Atlas: The Setup

We model the response surface as a draw from a Gaussian process:

$$
f(\mathbf{x}) \sim \mathcal{GP}\!\left(\mu(\mathbf{x}),\; k(\mathbf{x}, \mathbf{x}')\right)
$$

where $\mathbf{x} \in \mathbb{R}^d$ is the formulation vector (ingredient ratios,
process conditions), $\mu$ is the prior mean (optionally a physics-based model),
and $k$ is a kernel encoding smoothness and length-scale assumptions.

After $n$ observations $\mathcal{D}_n = \{(\mathbf{x}_i, y_i)\}_{i=1}^n$,
the posterior is Gaussian with closed-form mean and covariance:

$$
\mu_n(\mathbf{x}) = \mu(\mathbf{x}) + \mathbf{k}^\top(\mathbf{K} + \sigma^2 \mathbf{I})^{-1}(\mathbf{y} - \boldsymbol{\mu})
$$

$$
\sigma_n^2(\mathbf{x}) = k(\mathbf{x},\mathbf{x}) - \mathbf{k}^\top(\mathbf{K} + \sigma^2 \mathbf{I})^{-1}\mathbf{k}
$$

The next experiment is selected by maximizing an acquisition function $\alpha(\mathbf{x})$
over the posterior. Expected Improvement (EI) is the default:

$$
\alpha_{\text{EI}}(\mathbf{x}) = \mathbb{E}\!\left[\max(f(\mathbf{x}) - f^*, 0)\right]
$$

which has an analytic form under the GP predictive distribution.

---

### What Makes This Hard in Practice

Kernel selection, hyperparameter marginalizing, constraint handling (feasibility regions,
mixture constraints $\sum x_i = 1$), and batch acquisition for parallel experiments—
each is a solved problem in isolation. Integrating them into a system that a non-statistician
can run reliably is the engineering challenge.

That integration is what Formulation Atlas addresses. More detail in subsequent posts.
