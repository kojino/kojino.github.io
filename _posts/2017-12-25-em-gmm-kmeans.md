---
title: "EM, Mixture of Gaussians and K-means"
categories:
  - Machine Learning
mathjax: true
---

This post ties together EM (Expectation Maximization), GMM (Gaussian Mixture Models), K means and variational inference. If you have taken an introductory machine learning course and have learned these algorithms, but the connections between them are not yet clear, this is the post for you. If you have not heard about two of the above terms, I suggest you read the wikipedia page for each before reading this post.

## TL;DR:
- EM is a variational inference algorithm to optimize the lower bound of the log likelihood.
- GMM is a specific example of EM where the base distribution is MVN (multivariate normal).
- K means is GMM where variance and cluster assignment probabilities are fixed and cluster assignments are hard.

This post is rather theoretic compared to the other ones but I assure you that you'll have a very deep understanding of the above algorithms by the end of this post.

## Theory of Variational Inference

Let me first introduce the EM algorithm in a rather theoretical way. I found this theoretical introduction more "intuitive" than other attempts. I hope you will, too. In short, the goal of EM is to increase the likelihood, and it does so by giving a lower bound of the likelihood. Let $$X=\{X_1,X_2,...,X_N\}$$ denote the data we have at hand. Then, the likelihood of a model parameterized by $$\theta$$ is

$$l(\theta)=p(X\lvert \theta)$$

Now let me introduce variables for cluster membership $$Z=\{Z_1,Z_2,...,Z_N\}$$. Here, each $$Z_i$$ is a $$K$$ dimensional vector where each vector denotes which cluster each data point belongs to. Then, using the law of total probability,

$$l(\theta)=p(X\lvert \theta)=\sum_Z p(X,Z\lvert \theta)$$

The learning objective, as usual, is to maximize the log likelihood:

$$argmax_{\theta} \ log \: l(\theta)=argmax_{\theta} \ log \: \sum_Z p(X,Z\lvert \theta)$$

This would have been easier if the log likelihood was of form $$\sum_Z log(p(X,Z\lvert \theta))$$ as the sum is outside the log and hence we can take the derivative easily. Life is not so easy here since the sum is inside the log. **How can we take the log outside?** This question motivates us to introduce Jensen's Inequality:


> For any concave function $$\phi$$, $$\phi(EX) \geq E\phi(X)$$
  <cite><a href="https://en.wikipedia.org/wiki/Jensen%27s_inequality">Jensen's Inequality</a></cite>

Using Jensen's Inequality, we can take the sum outside:

$$\begin{align}
& log \ \sum_Z p(X,Z\lvert \theta) \\
& =log \ \sum_Z \frac{q(Z)}{q(Z)} p(X,Z\lvert \theta) \\
& =log \ \sum_Z q(Z) \frac{p(X,Z\lvert \theta)}{q(Z)} \\
& =log \ E_{Z\sim q} \frac{p(X,Z\lvert \theta)}{q(Z)} \\
& \geq E_{Z\sim q} \ log \ \frac{p(X,Z\lvert \theta)}{q(Z)} \\
& = \sum_Z q(Z) log \ \frac{p(X,Z\lvert \theta)}{q(Z)} \\
& \equiv L(q,\theta)
\end{align}$$

Jensen's Inequality is used from line 4 to 5 to provide $$L(q,\theta)$$ as the lower bound for $$log \ \sum_Z p(X,Z\lvert \theta)$$. You should be able to see that $$L(q,\theta)$$ only has the sum outside the log.

I abruptly introduced $$q(Z)$$ without really explaining it. What is $$q(Z)$$ here? We can think of $$q(Z)$$ as a probability distribution we model that we wish to be as close to the true data distribution $$p(Z\lvert X,\theta)$$ as possible. To see why this is the case, consider the difference between the true model log likelihood and our lower bound approximation $$L(q,\theta)$$:

$$\begin{align}
& log \: l(\theta)-L(q,\theta) \\
&= log \: l(\theta)-\sum_Z q(Z) log \ \frac{p(X,Z\lvert \theta)}{q(Z)} \\
&= \sum_Z q(Z) log \: l(\theta)-\sum_Z q(Z) log \ \frac{p(X,Z\lvert \theta)}{q(Z)} \\
&= \sum_Z q(Z) log \: l(\theta)-\sum_Z q(Z) log \ \frac{p(Z\lvert X,\theta)p(X\lvert \theta)}{q(Z)} \\
&= \sum_Z q(Z) (log \: p(X\lvert \theta)- log \: p(Z\lvert X,\theta) - log \: p(X\lvert \theta) + log \: q(Z)) \\
&= \sum_Z q(Z) log \: \frac{q(Z)}{p(Z\lvert X,\theta)} \\
&= KL(q(Z)\lvert \lvert p(Z\lvert X,\theta))
\end{align}$$

Note that from line 2 to 3, we are using the fact that $$\sum_Z q(Z)=1$$. To summarize, we have so far:

$$log \: l(\theta)=L(q,\theta)+KL(q(Z)\lvert \lvert p(Z\lvert X,\theta))$$

This equation explains why we want $$q(Z)$$ to be as close to $$p(Z\lvert X,\theta)$$ as possible. It's because we want to have $$KL(q(Z)\lvert \lvert p(Z\lvert X,\theta))$$ smaller so as to make $$L(q,\theta)$$ a tighter lower bound for $$log \: l(\theta)$$. To sum up, we have:

- Original goal: $$argmax_{\theta} \ log \: l(\theta)$$
- New goal: $$argmax_{q,\theta} \ L(q,\theta)$$

## From Variational Inference to EM
Now that we introduced the new optimization problem we care about, let's actually solve it! This will yield the formula for the EM algorithm in the most general way possible. Since $$L(q,\theta)$$ has two parameters $$q$$ and $$\theta$$, let's optimize the function w.r.t. one parameter at a time.

### E Step: $$argmax_{q} \ L(q,\theta)$$

Let's maximize $$L(q,\theta)$$ w.r.t. $$q$$. Since we know that $$log \: l(\theta)=L(q,\theta)+KL(q(Z)\lvert \lvert p(Z\lvert X,\theta))$$ and also that $$log \: l(\theta)$$ is fixed and doesn't depend on $$q$$, this is equivalent to $$argmin_{q} \ KL(q(Z)\lvert \lvert p(Z\lvert X,\theta))$$. Since we know that the minimum value of Kullback–Leibler divergence is $$0$$, we want to set $$q(Z)$$ such that:

$$KL(q(Z)\lvert \lvert p(Z\lvert X,\theta))=0 \Leftrightarrow q(Z)=p(Z\lvert X,\theta)$$

Thus, we have the update formula for the E step:

$$q(Z)=p(Z\lvert X,\theta)$$

Because we are minimizing the KL divergence but the likelihood itself doesn't change, **E step is equivalent to making the lower bound tighter**.

### M Step: $$argmax_{\theta} \ L(q,\theta)$$

It is not so hard to maximize $$L(q,\theta)$$ w.r.t. $$\theta$$. But the intuition behind M step is trickier. This is because $$log \: l(\theta)$$ depends on $$\theta$$ and thus we first need to understand why $$\theta$$ that maximizes $$L(q,\theta)$$ also increases $$log \: l(\theta)$$. To see this, recall

$$log \: l(\theta)=L(q,\theta)+KL(q(Z)\lvert \lvert p(Z\lvert X,\theta))$$

In E step, we minimized the second term on RHS (KL divergence) to be its smallest possible value $$0$$. Hence, this term can only increase as we change $$\theta$$ in M step. The first term on RHS will obviously increase because that is what we are aiming for in M step. Hence, as a whole, we know that $$log \: l(\theta)$$ must also increase.

Now, since we are updating $$\theta$$, let's rewrite $$q(Z)$$ after the E step as $$q(Z\lvert X,\theta^{old})$$ so as to distinguish with the new $$q(Z)$$ derived from the M step. Using this new terminology:

$$\begin{align}
& L(q,\theta) \\
&= \sum_Z q(Z\lvert X,\theta^{old}) log \ \frac{p(X,Z\lvert \theta)}{q(Z\lvert X,\theta^{old})} \\
&= \sum_Z q(Z\lvert X,\theta^{old}) log \ p(X,Z\lvert \theta) - \sum_Z q(Z\lvert X,\theta^{old}) log \ q(Z\lvert X,\theta^{old}) \\
&= Q(\theta,\theta^{old}) + const
\end{align}$$

where $$Q(\theta,\theta^{old})\equiv \sum_Z q(Z\lvert X,\theta^{old}) log \ p(X,Z\lvert \theta)$$.
Hence,

$$argmax_{\theta} \ L(q,\theta)=argmax_{\theta} \ Q(\theta,\theta^{old})$$

$$Q(\theta,\theta^{old})$$ depends on how we parametrize the model. One case is GMM, and is exemplified below. We now have the update rule of the M step. In contrast to the E step, **M step is equivalent to raising the log likelihood higher**.

## GMM as EM

Now, we will show that GMM is a specific example of EM. To see this, let
- $$X=\{X_1,X_2,...,X_N\}$$ be the data.
- $$Z=\{Z_1,Z_2,...,Z_N\}$$ be the cluster membership.
- $$\theta=\{\mu_1,...,\mu_K,\Sigma_1,...,\Sigma_K,\pi_1,...,\pi_K\}$$ be the parameters. $$\mu,\Sigma$$ are parameters of the Gaussian, and $$\pi$$ are prior mixture probability.

The whole model is a mixture of MVN, as follows:
$$p(X,Z\lvert \theta)=\prod_{n=1}^N \prod_{k=1}^K \pi_k^{Z_{nk}} N(X_n\lvert \mu_k, \Sigma_k)^{Z_{nk}}$$

### E Step: $$q(Z)=p(Z\lvert X,\theta)$$
$$\begin{align}
& q(Z) \\
&=p(Z\lvert X,\theta) \\
&=\frac{p(X,Z\lvert \theta)}{p(X\lvert \theta)} \\
&=\frac{\prod_{n=1}^N \prod_{k=1}^K \pi_k^{Z_{nk}} N(X_n\lvert \mu_k, \Sigma_k)^{Z_{nk}}}{\sum_{Z_n} \prod_{n=1}^N \prod_{k=1}^K \pi_k^{Z_{nk}} N(X_n\lvert \mu_k, \Sigma_k)^{Z_{nk}}}
\end{align}$$

### M Step: $$argmax_{\theta} \ Q(\theta,\theta^{old})$$
$$\begin{align}
& Q(\theta,\theta^{old}) \\
&= \sum_Z q(Z\lvert X,\theta^{old}) log \ p(X,Z\lvert \theta) \\
&= E_{Z\sim q(\cdot \lvert X,\theta^{old})} log \ p(X,Z\lvert \theta) \\
&= E_{Z\sim q(\cdot \lvert X,\theta^{old})} \sum_{n=1}^N \sum_{k=1}^K Z_{nk} (log \ \pi_k + log \ N(X_n\lvert \mu_k, \Sigma_k)) \\
\end{align}$$
Let $$E_{Z\sim q(\cdot \lvert X,\theta^{old})}Z_{nk}=\gamma(Z_{nk})$$. Then,
$$\begin{align}
&= \sum_{n=1}^N \sum_{k=1}^K \gamma(Z_{nk}) (log \ \pi_k + log \ N(X_n\lvert \mu_k, \Sigma_k))
\end{align}$$

Since we have the constraint that $$\sum_k \pi_k =1$$ (the cluster membership probability sums to 1), the ultimate optimization problem becomes:

$$argmax_{\theta} \ Q'(\theta)=Q(\theta,\theta^{old})+\lambda(\sum_k \pi_k - 1)$$

Since $$\theta=\{\mu_1,...,\mu_K,\Sigma_1,...,\Sigma_K,\pi_1,...,\pi_K\}$$, we can solve this for $$\pi_k,\mu_k,\Sigma_k$$ one at a time:

$$\begin{align}
& \frac{\partial Q'}{\partial \pi_k} = 0 \Leftrightarrow \pi_k = \frac{N_k}{N} \\
& \frac{\partial Q'}{\partial \mu_k} = 0 \Leftrightarrow \mu_k = \frac{1}{N_k} \sum_{n=1}^N \gamma(Z_{nk})X_n \\
& \frac{\partial Q'}{\partial \Sigma_k} = 0 \Leftrightarrow \Sigma_k = \frac{1}{N_k} \sum_{n=1}^N \gamma(Z_{nk})(X_n-\mu_k)(X_n-\mu_k)^T \\
\end{align}$$


## K means as GMM

It is straightforward to see that K means is a specific instance of GMM. In GMM, assume the following:
- Constant variance: $$\Sigma_k = \sigma^2 I_D$$
- Constant membership probability: $$\pi_k=\frac{1}{k}$$
- Hard membership assignment: $$X(m,n)=
\begin{cases}
1, if \quad k=argmax_k \ N(X_n\lvert \mu_k,\Sigma_k)\\
0, otherwise
\end{cases}
$$

With these three additional assumptions, we have the K means! To see this, let's take a look at the E step and the M step.

### E Step: $$q(Z)=p(Z\lvert X,\theta)$$
Since the membership assignment is hard, for each point, we want to find a class that maximizes its likelihood:
$$\begin{align}
&argmax_k \ N(X_n\lvert \mu_k,\Sigma_k) \\
&= argmax_k \ exp(-\frac{(X_n-\mu_k)^2}{2\sigma^2}) \\
&= argmin_k \ (X_n-\mu_k)^2
\end{align}$$

Hence we see that E step is equivalent to assigning points to the nearest centroid.

### M Step: $$argmax_{\theta} \ Q(\theta,\theta^{old})$$
We only need to update $$\mu_k$$ according to the update rule derived above:

$$\mu_k = \frac{1}{N_k} \sum_{n=1}^N \gamma(Z_{nk})X_n$$

This is equivalent to taking the mean of data points in each cluster.

## Summary
I hope you were able to see the theoretical derivation of EM and that GMM and K means are both specific instances of EM. To read more about this material, I suggest you refer to the following:
- Information Theory, Inference, and Learning Algorithms (MacKay) Chapter 20, 22, 33.7
- Machine Learning: A Probabilistic Perspective (Murphy) Chapter 11.4
- Pattern Recognition and Machine Learning (Bishop) Chapter 9
