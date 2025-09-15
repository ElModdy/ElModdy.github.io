---
title: "Berghain Challenge"
date: 2025-09-15
---

## The Problem
This challenge is inspired by the [Berghain Challenge](https://berghain.challenges.listenlabs.ai/), originally promoted by [Listen Lab](https://www.linkedin.com/company/listenlabss/about/). In it, you play the role of a nightclub bouncer tasked with filling 1,000 spots while satisfying strict demographic constraints—like ensuring at least 40% Berlin locals, 80% wearing black, and so on.<br><br>
The twist? Guests arrive one by one, and you must make immediate accept or reject decisions with no second chances. Your goal is to meet all constraints while rejecting as few people as possible.

## Results
I first enrolled as “Davide Modesto,” but realized that being scarce on [LinkedIn](https://www.linkedin.com/in/davide-m-561915128) and [Instagram](https://www.instagram.com/davidemodesto_/) meant nobody would contact me. With only 25 characters for a name, I combined the reason people might reach me and an email into:
**`looking4job.orphd@gmx.at`**.<br>
Regretted it immediately, but too late.
<br>
Now to the actual numbers (remember: **lower = better**):
<br>
* **1st place**: 7,609
* **Me**: 8,092
* **GPT-5-pro**: 24,913
<br>
So yes, I actually did beat GPT-5-pro. I ended up in **32nd place**, and I’m genuinely proud of that, especially because my approach was just a minimal, lightweight algorithm that can run on a laptop, with no machine-learning machinery in sight.

## Mathematical Framework
Let $F$ be the set of binary features we care about (e.g. {local/tourist, black outfit/other, young/old}). Each candidate is represented as a binary feature vector $X_n \in \{0,1\}^{|F|}$, where each coordinate represents presence or absence of a feature. These candidates arrive i.i.d. from some unknown distribution $\mu$.<br><br>
The challenge provides marginal probabilities and pairwise correlations, but this information doesn't uniquely determine the joint distribution for $|F| > 2$. To resolve this, I estimated an empirical distribution $\mu_{\text{init}}$ from initial gameplay, then solved an optimization problem minimizing KL divergence from $\mu_{\text{init}}$ subject to the given marginal and correlation constraints.

## Decision Policy
Our strategy is a stochastic policy $a: \{0,1\}^{|F|} \rightarrow [0,1]$, where $a(x)$ represents the probability of accepting a candidate with feature vector $x$. For the $n$-th candidate with features $X_n = x$, our decision $A_n \in \{0,1\}$ satisfies:
$$\Pr(A_n = 1 | X_n = x) = a(x)$$

## The Core Optimization Problem
The idea is to strategically vary $a$ to "steer" the demographics of accepted candidates toward the required constraints while minimizing rejections.
<br>
To frame this mathematically, let's introduce:<br><br>
**Acceptance Time $\tau(a)$:** The number of candidates we must see until we accept one:
$$\tau(a) := \inf\{k \geq 1: A_k = 1\}$$

**Accepted Candidate $Y(a)$:** The feature vector of the candidate we actually accept:
$$Y(a) := X_{\tau(a)}$$
Now we can state the idea mathematically as follows.<br>
At any point with $N$ remaining spots and $c_i$ missing people with feature $i$, we want to solve:

$$\begin{aligned}
&\text{minimize} && \mathbb{E}[\tau(a)]\\
&\text{subject to} && N \cdot \mathbb{E}[(Y(a))_i] \geq c_i \quad \text{for } i=1,\ldots,|F|
\end{aligned}$$

## Computing the Distribution of $\tau(a)$
The probability of accepting any given candidate is
$$
Z(a) := \Pr(A_n(a)=1) = \sum_{x\in S}\mu(x)a(x).
$$
For each integer $k\ge1$,
$$
\begin{aligned}
\Pr(\tau(a)=k)
&= \prod_{n=1}^{k-1} \Pr(A_n=0)\Pr(A_k=1) \\
&= (1-Z(a))^{k-1}Z(a).
\end{aligned}
$$

Thus $\tau(a)$ follows a geometric distribution with parameter $Z(a)$, so its expected value is $\mathbb{E}[\tau(a)]=\frac{1}{Z(a)}$.
## Computing the Distribution of $Y(a)$
Now we can calculate the distribution of $Y(a)$
$$\begin{align}
\Pr(Y(a) = x) &= \sum_{k=1}^{\infty} \Pr(\tau(a) = k, X_k = x) \\
&= \sum_{k=1}^{\infty} \Pr(X_k = x, A_k = 1) \prod_{j=1}^{k-1} \Pr(A_j = 0) \\
&= \sum_{k=1}^{\infty} \mu(x) a(x) (1-Z(a))^{k-1} \\
&= \frac{\mu(x) a(x)}{Z(a)}
\end{align}$$

Therefore: $$\mathbb{E}[(Y(a))_i] = \sum_{x: x_i = 1} \frac{\mu(x) a(x)}{Z(a)}$$.

## The Complete Reformulation
Now we can substitute our computed expectations back into the original problem:

$$\begin{aligned}
&\text{maximize} && \sum_{x \in \{0,1\}^{|F|}} \mu(x) a(x) \\
&\text{subject to} && N \sum_{x: x_i = 1} \mu(x) a(x) \geq c_i \sum_{x \in \{0,1\}^{|F|}} \mu(x) a(x) \quad \text{for } i=1,\ldots,|F|\\
&& & 0 \leq a(x) \leq 1 \quad \text{for all } x \in \{0,1\}^{|F|}
\end{aligned}$$

## Implementation
Here’s the GitHub link 🙃 [GitHub Repository](https://github.com/ElModdy/listenlab-challenge)
Honestly, an AI could probably fill in the implementation from this post, minor tweaks aside, it’s all there.

For some extra luck running multiple simulations, I ended up using **GitHub Actions**: free, fast, and surprisingly perfect for the job.
