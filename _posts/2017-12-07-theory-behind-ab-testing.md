---
title: "The Theory behind AB Testing: Introduction to Causal Inference"
categories:
  - Causal Inference
mathjax: true
---

If you've been working in the tech industry, or have thought about doing so, you've probably heard of AB testing. Some of you have even conducted one. The idea is as follows: say you own a website with a red login button. You're thinking that you should change it to blue, since it looks to ugly. To determine which color you should use, you ask your users. You randomly sample 100 users and show 50 users the red button and 50 users the blue button. You measure the ratio of people who login to your website for each group, and see if there's a big difference.

This is an example of a *completely randomized experiment* (CRE). In CRE, you first sample *units* (users), randomly assign a fraction of people to *treatment* (the red button) vs *control* (the blue button) and measure the difference in the *outcome* (the login rate).

Here, we can intuitively see that the difference in the login rate is caused by the different colors of the login button. In other words, the difference in the login rate has a causal interpretation.

On the other hand, imagine a situation where you started a campaign on your website, say a 10\% off sale for all ice creams sold on your website. To see the effect of your campaign, you compare the total number of ice creams sold before and after the campaign. You saw a 20\% increase in the sales volume. "The campaign was a success!", you conclude. But can you really call the campaign a success?

Not really. For a simple example, imagine that the campaign was running for two months in June and July. In May, you had $1 million in monthly revenue, and after the campaign, in August, now you have $1.2 million. Now consider this question: when are users more likely to buy ice creams, in May or in August? If you live in a certain part of the northern hemisphere like me (and so were the users), they would be more likely to buy ice creams in August. So, it is hard to tell if that $0.2 million increase in sales was coming from the campaign or from the fact that it was summer after the campaign was over. In such a scenario, the difference in the revenue does NOT have a causal interpretation: we don't know if it was due to the campaign.


### Causal inference is the theory behind AB testing.

When can we say that the increase in the login rate was *caused* by the change in the login button color? When can we say that the increase in revenue was *caused* by the campaign conducted? In general, when can we learn that something *caused* something else?

This is what causal inference is all about. Causal inference is a field for understanding the causal relationships between different events.

Let me formalize the notations used in causal inference. Let $$i$$ be units. Units are users in the above example.

- $$X_i$$: Pretreatment covariates. These can be age, gender, registration rate, past purchase history of the users. It is often a $$d$$ dimensional vector where $$d$$ is the number of features about each user.
- $$D_i$$: Treatment. For the above example, $$D_i=1$$ if the button that a user $$i$$ shown is red and $$D_i=0$$ if blue. $$D_i$$ is often binary, but can also take multiple, or even continuous values.
- $$Y_i$$: Observed outcome. In the above examples, this can be an indicator of whether user $$i$$ logged in or the total amount of ice cream user $$i$$ bought.

### Potential Outcomes define what are fundamentally unknown.

In real world, each unit can only have either one of the two treatments. If the user sees a blue button, they cannot see the red one, and vice versa. When $$Y_i$$ can take on two values, one under treatment $$Y_i(1)$$ and one under control $$Y_i(0)$$, we can observe either one of the two. Hence, $$Y_i(1)$$ and $$Y_i(0)$$ are called the potential outcomes. The fact that we can only observe one of the two (or many, in the case with more than two treatments) is so fundamental that it is called the "Fundamental Problem of Causal Inference".

### Before analysis, we need SUTVA.

Before moving on to understanding the causal effects in depth, I will introduce two assumptions that are often employed to make the modeling simple. SUTVA stands for Stable Unit Treatment Value Assumption, and it consists of two assumptions about the data:

- $$Y_i(d_1,...,d_N) = Y_i(d_i)$$. No Interference. "My outcome is only dependent on my treatment assignment and not others." E.g. the amount of ice cream I bought will depend on whether I was targeted by the campaign but not on whether my friends were targeted by the campaign.
- $$Y_i=Y_i(d)$$ if $$D_i=d$$. No Hidden Variations in Treatment. E.g. If I'm administered a medicine, I can only take or not take it; I cannot take half the portion of others, take an older version of it that is less effective, etc.

SUTVA assumptions makes sense in some cases, but not in others. For example, we should be skeptical about the no interference case if the treatment on friends, neighbors, families, etc of the units can affect their potential outcomes. The ice cream example can potentially violate no interference (and thus, have a *spill over effect*).

The second assumption can break down if the treatments were not correctly standardized and prepared, as in the case of medicine doses.

However, to understand most important concepts in causal inference that can be useful for business and social sciences, it is ok to SUTVA (for now).

### Finally, we can talk about causal effects!
Under the SUTVA assumption, we can finally define what we ultimately care about, the causal effects. First, what is the increase in my ice cream purchases if I'm exposed to the campaign? That is defined as *Individual Causal Effect* (ITE):

$$\tau_i = Y_i(1)-Y_i(0)$$

This should be easy to understand. The difference in the outcome when I'm assigned treatment vs control is my causal effect of the treatment. Next, let's think about this in the population level. Say there are N users on the website. The *Average Treatment Effect* (ATE) is ITE averaged over the population:

$$E[\tau_i] = E[Y(1)]-E[Y(0)] = \frac{1}{N} \sum_{i=1}^N [Y_i(1)-Y_i(0)]$$

These two are probably the two most important metrics in causal inference. In addition, we can think about the treatment effect only for those who were treated as Average Treatment effect over the Treated (ATT) $$E[\tau_i\|D_i=1]$$ and ATE for users with a specific set of covariates $$ATE(x) = E[\tau_i\|X_i=x]$$.

### Population vs Sample

The treatment effects I just defined above are over the population (e.g. all users on your website). But usually, it is hard to run experiments over the entire population. We need to sample a fraction of the population, $$S$$ (a set of units of size $$n_S$$). In such cases, we define the *Sample Average Treatment Effect* (SATE):

$$\tau_S = \frac{1}{n}\sum_{i\in S}Y_i(1)-Y_i(0)$$

In contrast to SATE, ATE defined above is also referred to as the Population Average Treatment Effect (PATE). In later posts, we'll discuss if SATE is a valid estimator of PATE.

### Are these notations even helpful?

You might wonder, especially if you know AB tests well, if these mathematical formations of the problem is of any help. If so, consider a simple case where there's a spill over effect. If users' friends being in a campaign affects users' outcome for their ice cream purchases, the above treatment effects will not be able to be estimated from a simple comparison in the outcome between the two sample groups! In such a case, we need to assess the existence of spill over effects and incorporate that to our causal effect estimation. Otherwise, we can make a wrong conclusion of campaign being successful or unsuccessful, or the degrees of its success. Hence, being able to talk about experiments using the potential outcome model described above is crucial for being certain that the effect you measured has a causal interpretation. If not convinced yet, I hope to write some more posts in the future about the cool techniques developed in causal inference useful everywhere in business and academics.
