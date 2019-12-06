---
title: "Must-know probability distributions from a single coin toss"
categories:
  - Probability
mathjax: true
---

There are countless numbers of probability distributions. Some of them are so widely used and beautiful that they deserve a name. Surprisingly, all of those distributions can be derived starting from a single coin toss. Here's the demonstration.

### Consider a coin toss...
Let's start again with a coin toss. This time, think of a coin that lands heads with probability $$p$$ and probability $$1-p$$. This is called a Bernoulli distribution, and we write this as $$Bern(p)$$ Surprisingly, almost all important distributions we encounter in statistics and machine learning can be derived by combining this single coin toss somehow. Let's start with a simple example.

### Binomial Distribution
This is equivalent to tossing the same coin $$n$$ times.
Namely, let $$X_i \sim_{iid} Bern(p)$$, meaning coin tosses with the same probability of landing heads, that are independent of each other. Then, $$Y$$ the total number of heads in these $$n$$ coin tosses are:

$$Y = \sum_{i=1}^n X_i \sim Bin(n,p)$$

This was a pretty straightforward example. Now, let's move on to something a bit more complicated.

### Uniform Distribution
If you know what a uniform distribution is, it might seem counterintuitive at first that it can be generated using coin tosses. But we can! Again, let $$X_i \sim_{iid} Bern(p)$$. Then,

$$U = \sum_{i=1}^{\infty} \frac{X_i}{2^i} \sim Unif(0,1)$$

Conceptually, we can generate uniform distributions by infinite tosses of a fair coin. To see why, let's think about the CDF of $$Unif(0,1)$$. Using the above expression of the uniform distribution, we hope to derive $$P(U \leq u) = u$$. To do so, think of $$U$$ as a binary expansion $$U=0.X_1X_2...$$. Similarly, $$u=0.u_1u_2...$$. To see when $$U \leq u$$, we only need to check the first decimal point in which $$X_i$$ and $$u_i$$ defer (e.g. comparison of $$0.001010$$ and $$0.000101$$ can be made by comparing the third decimal point). Conditioning on $$j$$, the decimal point in which the two numbers differ, we have:

$$P(U\leq u) =P(U<u)= \sum_{j=1}^{\infty} P(U\leq u|J=j)P(J=j)$$

The first equation follows from the fact that $$P(U=u)=0$$. The second equation is using [the law of total probability](https://en.wikipedia.org/wiki/Law_of_total_probability).

### Exponential Distribution
Exponential distribution can be defined using a uniform distribution:

$$X=-logU\sim Expo(1)$$

where $$U\sim Unif(0,1)$$. Because the uniform distribution is generated using coin tosses, we can also think of exponential distribution as generated from them.

If you haven't seen exponential distributions before, it is a distribution that comes up a lot in [Poisson Processes](https://en.wikipedia.org/wiki/Poisson_point_process) (which is an important probability concept I'll describe in another post). The most important thing to remember about the exponential distribution is the *memoryless property*:

$$P(X>a+b)=P(X>a)P(X>b)$$

To have an intuitive understanding, think of $$X$$ as a waiting time of a bus at a bus stop. $$X$$ following an exponential distribution means that the time you have waited so far tells you nothing about the time you will wait until the bus comes (hence the term "memoryless"). This property is crucial in modeling natural phenomena such as radioactive particle decays in physics. Surprisingly, a *continuous* random variable is memoryless if and only if it has an exponential distribution ([proof](https://en.wikipedia.org/wiki/Memorylessness)).

### Geometric Distribution
In contrast to Exponential, a *discrete* random variable is memoryless if and only if it has a Geometric distribution.

$$G=\lfloor X \rfloor$$

where $$X$$ is exponential.

### Gamma Distribution
Gamma distribution is just a sum (or *convolution*) of i.i.d. Exponentials. Let $$X_i \sim_{iid} Expo$$. Then,

$$G_r = \sum_{i=1}^r X_i \sim Gamma(r)$$

Note that this only defines the Gamma distribution when $$r$$ is a positive integer. More comprehensive definition in $$r > 0$$ case will be left to other sources.

Gamma distribution is important because it is the conjugate prior for Poisson distribution (coming soon). Hence, it is important in Bayesian statistics. [The inverse of Gamma distribution](https://en.wikipedia.org/wiki/Inverse-gamma_distribution) is also widely used in Bayesian statistics to model the prior variance of a Normal distribution (again, coming soon). I know this is a lot of information; for now, think of Gamma as some distribution derived from Exponential that is important in Bayesian statistics.

### Chi Squared Distribution
Chi Squared distribution is just a special case of Gamma distribution. Namely, Chi Squared distribution with $$n$$ degrees of freedom is

$$W^2 \sim \chi^2_n \sim 2Gamma(\frac{n}{2})$$

For now, don't worry too much about "$$n$$ degrees of freedom" part (it just means that there are $$n$$ parameters we can vary). Chi Squared distribution is often used in [hypothesis testing](https://en.wikipedia.org/wiki/Chi-squared_test). As noted in the next section, it has a close relationship with the more famous distribution, Normal distribution.

### Normal Distribution
Normal distribution is defined using Chi distribution (which is just the square root of Chi Squared distribution) and a random sign. A random sign is a random variable which takes the value $$1$$ with probability $$1/2$$ and $$-1$$ with probability $$1/2$$. Let the former be $$W$$ and the latter be $$S$$. Then,

$$Z=SW\sim N(0,1)$$

$$Z$$ is a standard normal, because it has a mean $$\mu=0$$ and a variance $$\sigma=1$$. Any normal distribution can be obtained from $$Z$$:

$$X=\mu+\sigma Z \sim N(\mu,\sigma)$$

Another relationship between normal and Chi distributions is that Chi Squared distribution is sum of i.i.d. standard normals. Since the sum of n iid Gamma(1/2) is Gamma(n/2).

$$ \begin{align}
Z_1^2+...+Z_n^2 &=W_1^2+...+W_n^2 \quad (\because Z_i \sim SW_i \Rightarrow Z_i^2\sim W_i^2) \\
&\sim Gamma(n/2) \\
&\sim \chi^2_n
\end{align} $$

### Student-t, Cauchy, Log Normal
These are all distributions that can be derived using Normals and/or Chi-Squared. I will not go into depth with each one, but you will for sure come across each of this as you study more statistics and machine learning.

- Student-t: $$T=\frac{Z}{\sqrt{V_n/n}}$$ where Z is a standard normal and $$V_n \sim \chi^2_n$$. Widely used in [hypothesis testing](https://en.wikipedia.org/wiki/Student%27s_t-test).
- Cauchy: $$Cauchy \sim \frac{Z_1}{Z_2}$$ where $$Z_1,Z_2$$ are i.i.d standard normals. Famous because mean and variance are undefined.
- Log Normal: $$Y=e^X$$ where X is Normal. It's just an exponential of a normal random variable, but widely used in modeling.


### Beta Distribution
Beta distribution is widely used in Bayesian statistics because it is the conjugate prior of Benroulli and Binomial distributions.

$$B = \frac{G_a}{G_a+G_b}$$

where $$G_a \sim Gamma(a)$$, $$G_b \sim Gamma(b)$$ and they are independent. As you can immediately see, Beta distribution has a close connection with the Gamma distribution. Fun fact is that $$\frac{X_1}{X_1+X_2}$$ and $$X_1+X_2$$ are independent if and only if $$X_1,X_2$$ are independent Gammas. In such case, $$\frac{X_1}{X_1+X_2}$$ is Beta and $$X_1+X_2$$ is also a Gamma. If we think of $$X_1$$ as waiting time in a bus stop and $$X_2$$ as waiting time at a station, this tells us that the proportion of time one waited at a bus stop tells nothing about the total time waited.

### Poisson Distribution
Last but not least, Possion distribution is defined using Exponential and a Poisson Process. This is tricky, so let me explain it in depth. Think of buses arriving at a bus stop. Let $$0<T_1<T_2<...$$ be their arrival times. Let $$T_n \sim Gamma(\lambda,n)$$ or equivalently, $$T_1,T_2-T_1,... \sim_iid Expo(\lambda)$$ (think about why). Then $$N_t=max\{n:T_n \leq t\}$$, the number of bus arrivals until time t follows a Poisson process:

$$N_t=max\{n:T_n \leq t\} \sim Pois(\lambda t)$$

### That's it!

It all started from a coin toss... We've come this far. Perhaps in the end, you've forgotten about that coin toss in the beginning. But if you carefully track how the distributions are related to each other, you'll start to see how a coin toss magically opens up the door to the world of probability distributions.
