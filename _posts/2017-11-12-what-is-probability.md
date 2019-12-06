---
title: "What is probability?"
categories:
  - Probability
mathjax: true
---

We come across probability not just in statistics classrooms but also in real life. But, have you thought about what probability really means? I would like to introduce you to a formal definition of probability.

## Consider a coin toss...
Let's start with a simple example. Imagine tossing a fair coin where the outcome is either heads or tails. What is the probability of tossing heads?
You're right, $$0.5$$. But why not $$-0.7$$, $$20$$, or $$\frac{1024}{7}$$? Well, the (somewhat boring) answer is because it is defined as such. To give you a more in depth answer, this post will introduce the formal way to define what a probability is.


## The world of $$\Omega$$, $$F$$, $$P$$.
Probability, or more formally, a **probability space** is defined using three letters: $$\Omega$$, $$F$$, $$P$$. What are they? Let's think a fair coin toss as an example.

### $$\Omega$$ is a sample space.
A sample space is a set of all the possible outcomes in a certain process. In a coin toss, a coin can only land heads (H) or tails (T), so there are the two only outcomes. So we have:  

$$\Omega = \{ H,T \}$$

Each element in $$\Omega$$ is an **outcome** and is often referred to as $$\omega$$ ('small' omega).

### $$F$$ is a set of 'events'.
First, let's define an event. An **event** is a set that contains multiple outcomes. For example:  

- $$\{ H \}$$   &nbsp;&nbsp;&nbsp;&nbsp; is "an event that a coin lands heads"  
- $$\{ T \}$$   &nbsp;&nbsp;&nbsp;&nbsp; is "an event that a coin lands heads"  
- $$\{ H,T \}$$ is "an event that a coin lands heads or tails"
- $$\{ \}$$ &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; is "an event that a coin lands neither heads nor tails"

In fact, these are the only events that are defined in a probability space of a coin toss!
A **set of events** is literally a set that contains all the possible events in a certain process. Referring to the four events above, we have:  

$$F = \{\{ \},\{ H \},\{ T \}\{ H,T \}\}$$

Note that $$F$$ contains $$\Omega=\{ H,T \}\}$$. This is always the case in any probability space. To more formally define $$F$$, we need to introduce a more difficult concept called $$\sigma$$-algebra, but I'll leave that to future posts for now.

### $$P$$ is a probability measure.
A **probability measure** is a function that takes in an event as input, and spits out a probability of that event between 0 and 1. Formally, $$P: F \rightarrow [0,1]$$. In our example,

- <span class="tex2jax_ignore">$$P(\{ H \})=0.5$$</span>
- <span class="tex2jax_ignore">$$P(\{ T \})=0.5$$</span>
- <span class="tex2jax_ignore">$$P(\{ H,T \})=1$$</span>
- <span class="tex2jax_ignore">$$P(\{ \})=0$$</span>

For $$P$$ to be a probability measure, we need two more conditions (axioms).

- <span class="tex2jax_ignore">$$\begin{align}&P(\Omega)=1\end{align}$$</span>
- <span class="tex2jax_ignore">$$P(\bigcup_{j=1}^{\infty}A_j) = \sum_{j=1}^{\infty}P(A_j)$$</span> where $$A_j$$ are disjoint.

The first equation seems intuitive. Recall that $$\Omega$$ contains all the outcomes that can possibly happen. This equation is saying that the probability of either one of the all possible outcomes happening is 1. \\
<br/>
The second equation looks mysterious, so let me break it down. First, $$A_j$$ being disjoint means that when one event is happening, another event cannot happen. For example $${H}$$ and $${T}$$ are disjoint, whereas $${H}$$ and $${H,T}$$ are not (because they overlap). Formally, two events $$A,B$$ (or sets) are disjoint when $$A \cap B={}=\phi$$. $$\phi$$ is just a commonly used notation that refers to an empty set.\\
<br/>
The equation is essentially saying that the probability of either one of the multiple disjoint events happening $$P(\bigcup_{j=1}^{\infty}A_j)$$ is the same as the sum of the probability of each event happening ($$\sum_{j=1}^{\infty}P(A_j)$$). \\
For example, in our case, since $${H}$$ and $${T}$$ are disjoint and so it must be the case that

$$P({H}\cup{T})=P({H})+P({T})$$

## That's it!

This is in fact, all we need to define what a probability is. As a side note, physicists did try defining a negative probability. I'm not going into any details about it, but you can [read more about it](https://en.wikipedia.org/wiki/Negative_probability) if interested.
