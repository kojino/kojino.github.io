---
title: "Formal definition of an experiment"
categories:
  - Causal Inference
mathjax: true
---

Science experiments, social experiments, thought experiments, ... We use the word "experiment" somewhat often in real life. But have you ever wondered what experiments really are? Probably not... But don't close the page yet! It is actually quite interesting to learn about how statisticians formally define experiments. This post is about that. If you read this post, you will be able to tell your friends having a thought experiment that what they are doing is, statistically speaking, a not valid experiment. What a great knowledge to have!

### Jumping right in, the definition of an experiment.

Recall the previous post about causal inference. In any scientific studies, we care about the following three types of variables:

- $$X_i$$: Pretreatment covariates.
- $$D_i$$: Treatment.
- $$Y_i$$: Observed outcome.

Given these variables, let $$p_i$$ be the probability of unit i receiving a treatment given its covariates and potential outcomes. Formally,

$$p_i = P(D_i=1\|X,Y(0),Y(1))$$

Then an experiment can be defined quite simply; it is a study where $$p_i$$ is controlled by and known to the researcher. In other words, if the experimenter can decide whether to assign treatment or control to each unit, it is an experiment. For example, AB testing is an experiment since we are controlling how to allocate users to different groups.

### Randomized Experiments

The most important type of experiment is the randomized experiment. The intuitive way to understand it is, it is any experiment where units are randomly assigned to treatment or control, according to $$p_i$$. More rigorously, a randomized experiment should satisfy the following three conditions:

- $$0 < p_i < 1$$, non deterministic. Each unit has some chance (even very small) of being assigned to treatment/control.
- $$p_i \perp Y_{-i}, X_{-i} \forall i$$, individualistic. The treatment assignment probability doesn't depend covariates and potential outcomes of other units.
- $$p_i \perp Y_{i} \forall i$$, unconfounded. The treatment assignment probability is independent of the potential outcomes.

The first two assumptions should make an intuitive sense. The last one needs a bit of thought and in fact, it is the most crucial for characterizing a randomized experiment. Why are experiments not randomized if the treatment assignment is confounded? To see this, think of an experiment for measuring the effect of going to college on future income. Suppose you can randomly assign high school grads to go / not go to college (this is unethical in practice). If the experiment is counfounded, it means that the probability of a student assigned to go to college depends on their future income (supposing we are the God and we know their future). In the likely scenario, those who will earn more will have a higher probability ($$p_i$$) of being assigned to go to school. Consider the difference between the treat and the control in this scenario. Will it have a causal interpretation, in the sense that it captures the causal effect of going to college on the income? The answer is no. This is because we can't know if the difference in the future income is due to students who went to college already being smart, or the college education making them more capable of earning. Hence, unconfoundedness is crucial for deriving causal effects from randomized experiments.

Let me rephrase this argument in a more rigorous way.

Recall from the previous post that one of the metrics that we care about is the Average Treatment Effect:

$$E[\tau_i] = E[Y(1)]-E[Y(0)] = \frac{1}{N} \sum_{i=1}^N [Y_i(1)-Y_i(0)]$$

We cannot directly obtain $$E[Y(1)]-E[Y(0)]$$ since we can only observe one of the potential outcomes for each unit. On the other hand, what is available is the following:

$$\beta = E[Y|D=1]-E[Y|D=0]$$

I used the character $$\beta$$ for a reason I'll explain in the next post about causal inference. This is the average outcome for the treated minus the average outcome for the control. My claim here is that only under the unconfoundedness assumption, $$\beta$$ has a causal interpretation. It's actually a simple math:

$$ \begin{align}
\beta &= E[Y|D=1]-E[Y|D=0] \\
&= E[Y(1)|D=1]-E[Y(0)|D=0] (\because \text{SUTVA})\\
&= E[Y(1)]-E[Y(0)] (\because \text{unconfoundedness}) \\
&= ATE
\end{align} $$

I'm using the second assumption of SUTVA ($$Y_i=Y_i(d)$$) from the first to the second line. For the second to the third line, I'm using the fact that if $$p_i \perp Y_{i}$$ (unconfounded) then $$E[Y(1)\|D=1] = E[Y(1)]$$ and $$E[Y(0)\|D=0] = E[Y(0)]$$.

Hence, we have that *the difference in $$Y$$ between treatment and control is ATE only if the unconfoundedness assumption holds*. This is a very important result in causal inference, and this is why randomized experiment is such a convenient type of experiment.

### Examples of Randomized Experiments

Last but not least, let me introduce four types of randomized experiments. Some are not as widely used as the others, but they all have the above property of $$\beta=ATE$$ and hence you should consider these when running your AB tests:
- Bernoulli Trials: Each unit is assigned to treatment with the same probability $$p$$.
- Completely Randomized Experiments: Randomly sample $$n_t$$ units and assign them to treatment. AB tests are typically completely randomized experiments.
- Stratified Randomized Experiments: Subgroup units based on the covariates $$X$$. Within the subgroup, run a completely randomized experiments.
- Paired Randomized Experiments: Special case of stratified randomized experiments with two samples in each group.

That's it! Now you should be confident with what experiments are, and why AB tests are so powerful in finding causal effects. On the other side of the spectrum, there exists observational studies, where $$p_i$$ is unknown. The current studies in causal inference is very much focused on observational studies, as people have done enough research on experiments. I'll write about observational studies in the coming posts.
