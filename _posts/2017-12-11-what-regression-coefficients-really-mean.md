---
title: "What regression coefficients really mean"
categories:
  - Causal Inference
mathjax: true
---

There is nothing in statistics that is as easy as regressions to use but as hard to make a correct interpretation. Especially in causal inference, even the coefficients of a linear regression is quite hard to interpret. If you ask a random statistics student on campus and ask them to interpret a fitted linear regression model, you might find them having trouble giving a crisp explanation of what it captures. This is because, often times, the coefficients are not a causal effect. It also consists of what is known as a selection bias, which I'll explain in this post.

### The goal of regression = learn CEF.

CEF stands for conditional expectation function. CEF explains how much $$Y$$ changes as $$X$$ change. More formally, CEF is defined as

$$\mu(x) = E[Y\lvert X=x]$$

In short, the goal of any regression is to estimate $$\hat{\mu}(x)$$.

### regressions can be parametric or non parametric.

There are two ways to estimate CEF one way is parametric. We approximate CEF with a function with some parameters, and learn the best parameters that best fits CEF.

$$\hat{\mu}(x) = g_{\theta}(x)$$

Here, $$\theta$$ is the parameter we would like to estimate. For example, in linear regressions, $$\theta$$ will be the coefficients for the features.

On the other hand, there's a non parametric approach to the problem, where we directly estimate $$\mu(x)$$. This includes methods like random forest and other decision tree algorithms.


### Regression does not imply causality.

As we shall see, CEF is not causal. To see this, let's think about CEF in the case of an experiment with binary treatment. In this case, CEF must be linear, since the model is saturated. Namely, treatment $$D$$ can only take on two possible values, and hence a simple linear model can capture everything about the changes in $$D$$. Hence, we have:

$$\mu(d) = E[Y\lvert D=d] = \beta_0 + \beta D$$

This encode the information of how much $$Y$$ changes as $$D$$ changes.

Since

$$E[Y\lvert D=0] = \beta_0$$
and
$$E[Y\lvert D=1] = \beta_0 + \beta$$

we can write these two in a more concise notation:

$$E[Y\lvert D] = E[Y\lvert D=0] + D (E[Y\lvert D=1] - E[Y\lvert D=0])$$

This is our CEF. The reason I used $$\beta=E[Y\lvert D=1] - E[Y\lvert D=0]$$ in my previous post about the experiment is because it the ATE in the randomized experiment case is exactly this CEF. In general, however, $$\beta$$ will not be a causal effect. Let's see what $$\beta$$ consists of:

$$\begin{align}
\beta &= E[Y\lvert D=1] - E[Y\lvert D=0] \\
&= E[Y(1)\lvert D=1]-E[Y(0)\lvert D=0] (\because \text{SUTVA})\\
&= E[Y(1)\lvert D=1]-E[Y(0)\lvert D=1]+E[Y(0)\lvert D=1]-E[Y(0)\lvert D=0] \\
&= ATT + E[Y(0)\lvert D=1]-E[Y(0)\lvert D=0] \\
&= ATE + (ATT-ATE) + (E[Y(0)\lvert D=1]-E[Y(0)\lvert D=0])
\end{align}$$

Before explaining what this decomposition means, note that if the unconfoundedness assumption holds, $$\beta$$ will reduce to ATE, as shown in the previous post.

$$\begin{align}
\beta &= E[Y\lvert D=1] - E[Y\lvert D=0] \\
&= E[Y(1)\lvert D=1]-E[Y(0)\lvert D=0] (\because \text{SUTVA})\\
&= E[Y(1)]-E[Y(0)] (\because \text{unconfoundedness}) \\
&= ATE
\end{align}$$

This is why running a linear regression on a binary treatment experiment is enough to capture the causal effect.

Now let's get back to the more general case. We have

$$\beta = ATE + (ATT-ATE) + (E[Y(0)\lvert D=1]-E[Y(0)\lvert D=0])$$

What does this mean? It clearly means the coefficient of the regression is not just ATE. It consists of two more terms:

- $$E[Y(0)\lvert D=1]-E[Y(0)\lvert D=0]$$: Type I Selection Bias. This arises when $$Y(0) \not{\perp} D$$, meaning that the default outcome in the control case is different for those actually treated vs those who are not.
- $$ATT-ATE$$: Type II Selection Bias. This arises when $$Y(1)-Y(0)$$, meaning that the benefit of the treatment is different for those actually treated vs those who are not.

### Example: Regression for measuring the effect going to college

Selection biases might be hard to understand. So let me illustrate with an example. Again, think of an experiment for measuring the effect of going to college on future income. This time, we cannot randomly assign students to go to college as is usually the case. So this is an example of what is known as an observational study. What do the selection biases say?

If type I bias is positive, it means that even before entering college, those students who are to enter college have a higher potential to earn more. This intuitively makes sense, as those who enroll in colleges can be from a better socio economic status, are more motivated to advance their careers, etc.

If type II bias is positive, it means that those who go to college can gain more from college than those who don't. In other words, even if those who didn't college went college, they couldn't have increased the income as much as those who actually went.

Hence, these two biases can hinders $$\beta$$ from capturing the true "going to college" effect averaged over both those who did and didn't go to college.

### Summary: how to interpret regression coefficients

For the case when you run a linear regression on a binary treatment experiment, don't jump to the conclusion that the regression coefficients capture the true causal effect. If you witness that the change in going to college will raise the future income by $1000 a year, look for the two selection biases. It might be the case that those who went to college could earn $200 more than those who didn't and also increase the future earnings by $100 more than those who didn't. In this case, the the true "going to college" effect is $700.
