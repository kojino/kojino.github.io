---
title: "AI in Society: 0. Limitations of Machine Learning Today"
mathjax: true
excerpt: "Introduction to AI in Society blog series. The impact of AI today is already enormous. But there still are many obstacles to overcome."
---

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/2018-05-29-deep-learning-limitations/bias.png){: .align-center}

Machine learning has had immense success in wide range of fields, from autonomous driving, natural language translation, to diagnosing serious diseases.

However, as ML gets integrated to many social systems, predictive accuracy (or whatever other metrics we optimize for) is no longer our only concern. Terms like safety, interpretability and fairness are starting to be used by researchers and ML practitioners.

Here are a couple of examples. In automated hiring decision making, ML systems trained on biased past hiring data can favor people with a certain race or gender. In medical diagnosis, ML systems might be able to predict whether a treatment cures diseases with high probability but might not be able to explain to a patient *why* a certain treatment might work. In education, ML might be able to *associate* certain attributes with high future income, but might not be able to explain what changes can *cause* students to increase their future income. These are all limitations of ML systems that cannot be fully tackled by the breakthrough AI technologies you see on media everyday.

This will be a series of blog post about such limitations machine learning faces today. I will (nonexhaustively) introduce some of the many important issues that arises when ML systems are put into practice. For each topic, I listed some of the research directions proposed in recent academic papers. Feel free to use this as a reference to further exploration of each field.

The fields that I'm going to cover are the following:

1. [Interpretability]({{ site.url }}{{ site.baseurl }}/interpretability): Deep learning or AI in general is often referred to as a "black box". It is very hard to understand how sophisticated models actually reason about the data. This is troublesome, for example in medical or legal domains where how models draw conclusions is as equally important as what the conclusions are.

2. [Algorithmic Fairness]({{ site.url }}{{ site.baseurl }}/fairness): ML models can be biased when the data they are trained on is biased. How can we quantify (un)fairness? How can we enforce models to be fair?

3. Robustness: Commonly used ML models like neural networks are shown to classify inputs horribly when the input is perturbed even by a little. This can raise serious concerns in ML based security systems like facial recognition. Can we save the humanity from ML savvy hackers?

4. Strategic Considerations: People are not static points. If you use AI to automatically admit students to college or accept who to offer credit cards to, you will almost always have people who will try to "game the system". Strategic considerations applies principles from game theory to handle such issues.

5. Causality: ML models can understand the association between two things very well. But they have a long way to go for understanding the cause and effect. In domains like policy making, we are more interested in the consequence of certain policies. Can AI also reason causality?

6. Advising: Have you met people who say the right things, but the way they say them is very annoying? Your AI assistant can be like that. When advising humans, AI should not only have high IQ, but it should also have high EQ. This is also an open area of research under HCI (Human Computer Interaction).

Enjoy!
