---
title: "Regularization"
date: 2022-10-19
tags:
- seed
---

A method for controlling [[thoughts/complexity|complexity]]. Our main tools:
1. Model averaging (e.g. [[thoughts/Ensemble method|ensemble methods]])
2. Regularization (this)

When we have multiple models with the same training models, we should pick models that are *more conservative* (e.g. in [[thoughts/Linear Regression|linear regression]], pick smaller slope)

We should regularize $w_j$ so that they don't explode.

Makes the tangent to the level curves of the gradient point towards the global minimum

![[thoughts/images/l1-vs-l2-regularization.png]]

### L2-Regularization
$$f(w) = \frac 1 2 \lVert Xw - y \rVert^2 + \frac{\lambda}{2} \lVert w \rVert ^2$$

This *almost always* decreases test error. Bigger $\lambda$ also means gradient descent converges faster.

To help with this, we can standardize continuous feature by replacing it with its z-score.

### L1-Regularization
- Like L2-norm, it’s convex and improves our test error
- Like L0-norm, it encourages elements of $w$ to be exactly zero

#### How is this different from L2?
The penalty stays is proportional to how far away $w_j$ is from zero. There is still something to be gained from making a tiny value exactly equal to 0.

With L2, the penalty gets smaller as you get closer to zero. The penalty asymptotically vanishes as $w_j$ approaches 0 (no incentive for “exact” zeroes).

L1-Regularization sets values to exactly 0, basically removing features from the model