---
title: "AI in Society: 3. Adversarial Attacks"
mathjax: true
---

In classic machine learning, we assume that the test distribution is the same as the train distribution. This is unlikely in real world, where new inputs can have noise, be manipulated by agents to maximize their own interests, or even distorted for the malicious purpose of hurting your ML system. In this post, I mostly focus on the final set of inputs, often referred to as "adversarial" inputs. If we can prepare for the worst case, by developing models that are robust against these adversarial inputs, then we can also cope with the other two types of distortions.

# Types of Advesarial Inputs

There are many ways to classify adversarial inputs. Here are two based on the setup of what's available and what you want to do.

## Blackbox vs Whitebox

Adversarial inputs can be generated with or without (or partial) the knowledge of the classifier. In classic literatures, we assume that an adversary knows the classifier (whitebox). More recent papers developed ways to attack a classifier when the adversary doesn't know the model. In this case, people usually assume that an adversary can "query" the model, meaning that they can input a limited number of carefully chosen data points, investigate their outputs to imagine what the model might actually be. Think of it as you using IBM Watson API within the budget of \$500.

## Targeted vs Untargeted

When an adversary wants a data point to be misclassified, he/she wants it to either be misclassified to a specific class, or just any class. For example, if you are a criminal escaping from [Chinese government's facial recognition system](http://www.scmp.com/news/china/society/article/2115094/china-build-giant-facial-recognition-database-identify-any), you probably want yourself to be misclassified as anyone other than you (untargeted). On the other hand, if you're tyring to break into someone else's iPhone X using its facial recognition system, you probably want to be misclassified as that specific person (targeted).

## Missing Features

Adversaries can choose to drop a subset of features that most harms a classifier learned. More formally, we will assume for now that the adversary knows the learner's strategy (i.e. classifier). Among $$d$$ features, the adversaries choose $$J \in [d]$$ with $$\vert J\vert \leq V$$, for some norm budget $$V$$. For the subsets of features chosen, the values are all set to $$0$$ (or missing). The goal of the learner than is the following:

$$\min E_{(x,y)\sim D} \max_{J:|J|\leq V}\sum_{j\not{\in}J}l(f(x_j),y_j)$$

In other words, the learner should find a classifier $$f$$ that minimizes the loss, assuming that the adversary chooses a subset of features so as to maximize that loss. Note that these features can be same or different for each data point.

## Corrupted Features

This one is similar to missing features, but instead of dropping $$V$$ features for each data point, we can inject some random noise to $$V$$ features. The objective function looks quite similar, with an additional consideration of a "noise budget" (i.e. how much noise you can inject for each feature).

## Adversarial Examples

Research in missing features and corrupted features typically considers finding an attack/defense strategy for classifiers averaged over the distribution of inputs. If you look at the objective above, you can see the expectation symbol there. In contrast, adversarial examples consider a situation where you create one very small arbitrary perturbation that, when injected to a particular image, gets that image to be misclassified. Formally, we want to find

$$ \Delta(x;f) = \min_r \vert \vert r \vert \vert_2 \ s.t. \ f(x+r) \neq f(x)$$

As you can see, the objective now is not the overall performance. Of course, you can apply these adversarial methods to each data point. But former methods have been proposed with indepth analysis of computational complexity with respect to the feature size and the sample size. In contrast, adversarial examples have tackled the question: "how well can you fool with as less noise as possible?"

Adversarial examples have gained a lot of attention in the past few years. In addition to targeted vs untargeted and whitebox vs blackbox, there have been other attempts to generate different types of adversarial examples.

### Universal Adversarial Examples



### Physical Adversarial Examples

## Data Poisoning


## Robust Optimization

## Robustness in Unsupervised Machine Learning
