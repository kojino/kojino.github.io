---
title: "Inference Cheat Sheet"
mathjax: true
---

## Sufficiency and Ancillarity

### Def. Sufficiency
A statistic $$T(Y)$$ is sufficient for $$\theta$$ if $$Y|T$$ is $$\theta$$ free, i.e. $$P_\theta ( Y \in A | T = T (Y))$$ is $$\theta$$ free.

### Thrm. Factorization Theorem
$$T(Y)$$ is sufficient for $$\theta$$ iff $$f_\theta(y)=g_\theta(T(y))h(y)$$.

### Def. Minimal Sufficiency
A sufficient statistic $$T(Y)$$ is minimal if for any sufficent statistic $$S$$, there exists a function $$g$$ such that $$T(Y)=g(S(Y))$$ for all.

### Thrm.
A sufficient statistic $$T(Y)$$ is minimal if "$$\frac{f_\theta(x)}{f_\theta(y)}$$ is free of $$\theta$$ for $$x\neq y \Rightarrow T(x)=T(y)$$".

### Def. Complete
A statistic $$T(Y)$$ is complete for $$\theta$$ if "$$E_\theta [h (T (Y ))] = 0 \Rightarrow h(T)=0$$"

### Prop.
A CSS is also minimal, if MSS exists.

### Def. Ancillarity
A statistic $$A(Y)$$ is ancillary for $$\theta$$ if its distribution does not depend
on $$\theta$$.

### Thrm. Basu's Theorem
If $$T$$ is a CSS for $$\theta$$ and $$A$$ is ancillary for $$\theta$$, then $$A$$ and $$T$$ are independent.

## Unbiased Point Estimation

### Def. Unbiasedness
A statistic $$T (Y )$$ is an unbiased estimator of $$g(\theta)$$ if $$E_\theta [T (Y )] = g(\theta)$$.

### Thrm. Rao-Blackwell
Let $$W (Y )$$ be an unbiased estimator of $$g(\theta)$$ and $$T$$ be any sufficient statistic. Then, $$E_\theta [W | T]$$ is unbiased and $$Var(E_\theta [W | T]) \leq Var(E_\theta[W])$$. 

### Thrm. Lehmann–Scheffe
Let $$W (Y )$$ be an unbiased estimator of $$g(\theta)$$ and $$T$$ be CSS. Then, $$E_\theta[W|T]$$ is uniformly minimum variance unbiased estimator (UMVUE) of $$\theta$$.

## The Cramer-Rao Lower Bound

### Def. The Score Function
$$S(Y,\theta)=\frac{\partial \log f_\theta(Y)}{\partial \theta}$$

### Def. Exchange Derivatives and Integrals
$$\frac{\partial^m}{\partial\theta^m} \int g_\theta(y)d\mu(y) = \int \frac{\partial^m}{\partial\theta^m} 