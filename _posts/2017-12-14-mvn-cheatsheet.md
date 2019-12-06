---
title: "Multivariate Normal Cheatsheet"
categories:
  - Machine Learning
mathjax: true
---

Multivariate normal (MVN) is used everywhere in machine learning, from simple regressions, linear discriminant analysis, Kalman filters to gaussian processes. But very few textbooks summarize the important characteristics in a concise manner. So here they are.

## Definition
$$\mathbf{Y} \sim N_k(\mathbf{\mu},\mathbf{V})$$ if $$Y=A\mathbf{Z}+\mathbf{\mu}$$.
- $$\mathbf{Y}$$: $$k$$ dimensional vector
- $$\mathbf{\mu}$$: $$k$$ dimensional mean vector
- $$\mathbf{V}$$: $$m \times m$$ dimensional covariance matrix. It is positive semi-definite: $$\mathbf{x'Vx}>0$$ for all $$\mathbf{x}\neq \mathbf{0}$$.
- $$A$$: $$k \times m$$ dimensional matrix. $$\mathbf{V} = AA'$$
- $$\mathbf{Z}=(Z_1,...,Z_m)$$ where $$Z_i \sim_{iid} N(0,1)$$

## Equivalent Definition
$$\mathbf{Y} \sim N_k(\mathbf{\mu},\mathbf{V})$$ if all linear combinations of $$\mathbf{t'Y}$$ are univariate normal, i.e.

$$\mathbf{t'Y} \sim N_k(\mathbf{t'\mu},\mathbf{t'Vt})$$ for any $$\mathbf{t}$$.

## PDF and MGF
- PDF: $$f(\mathbf{Y}) = \frac{1}{(2\pi)^\frac{k}{2}\lvert \mathbf{V}\big \rvert^\frac{1}{2}}exp(-\frac{1}{2}((\mathbf{V-\mu})'V^{-1}(\mathbf{V-\mu}))$$
- MGF: $$M_{\mathbf{Y}}(\mathbf{t}) = E(e^{\mathbf{t'Y}}) = exp(\mathbf{t'\mu}+\frac{1}{2}\mathbf{t'Vt})$$

## Linear Transformations of MVN
If $$\mathbf{Y} \sim N_k(\mathbf{\mu},\mathbf{V})$$,
- $$\mathbf{X} = B\mathbf{Y} + \mathbf{b} \sim N(B\mathbf{\mu} + \mathbf{b},B\mathbf{V}B')$$
- $$\mathbf{a'Y} \sim N(\mathbf{a'\mu},\mathbf{a'Va})$$

## Within MVN...
Let $$Y=\begin{pmatrix}
\mathbf{Y_1} \\
\mathbf{Y_2}
\end{pmatrix} \sim N_{k_1+k_2}(\mathbf{\mu},\mathbf{V})$$, where \\
$$\mathbf{\mu}=\begin{pmatrix}
\mu_1 \\
\mu_2
\end{pmatrix}$$ and $$\mathbf{V}=\begin{pmatrix}
\mathbf{V_{11}},\mathbf{V_{12}} \\
\mathbf{V_{21}},\mathbf{V_{22}}
\end{pmatrix}$$

Then,
- Uncorrelation implies independence: $$\mathbf{V_{12}}=0 \Leftrightarrow \mathbf{Y_1} \perp \mathbf{Y_2}$$
- Marginal: $$\mathbf{Y_i} \sim N(\mu_1,\mathbf{V_{11}})$$
- Conditional: $$\mathbf{Y_2}\lvert \mathbf{Y_1} \sim N(\mu_{2\cdot 1},\mathbf{V_{22\cdot 1}})$$, where $$\mu_{2\cdot 1}=\mu_2+\mathbf{V_{21}}\mathbf{V_{11}}^{-1}(\mathbf{Y_1}-\mu_1)$$, $$\mathbf{V_{22\cdot 1}}=\mathbf{V_{22}}+\mathbf{V_{21}}\mathbf{V_{11}}^{-1}\mathbf{V_{12}}$$.

## Joint Distribution
$$Y\lvert \theta \sim N_k(\theta,A_1),\theta\sim N_k(\mu,A_2)$$ then $$(Y,\theta)\sim N_{2k}(\begin{pmatrix}
\mu \\
\mu
\end{pmatrix},\begin{pmatrix}
A_1+A_2,A_2 \\
A_2,A_2
\end{pmatrix})$$
