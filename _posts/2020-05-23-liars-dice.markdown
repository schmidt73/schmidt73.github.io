---
layout: post
title: A Brief Analysis Of Liar's Dice
date: 2020-05-23 13:43
comments: true
categories: [math]
preview: "/assets/post_previews/liarsdice.jpg"
---

> During the last week of quarantine with my friends from college, we
> spent a lot of time playing a drinking game called Liar's Dice [^1]. I
> loved playing it and was curious as to optimal decision during play,
> so I decided to do some analysis on it. Despite the game's
> simplicity, the questions I wanted to answer ended up being more
> difficult than anticipated. I hope you enjoy my unique perspective
> on a rather fun game!

Initial Surprises
================

Suppose my friends and I are in the first round of liar's dice, so
that there are $n = 25$ total dice in our cups. Would you be surprised
if I told you that greater than or equal to $\lceil\frac{25}{5}\rceil = 5$
of a certain number is always on the table and that with high
probability, greater than $8$ of a certain number is always on the
table? Further I ask, "how surprised would you be?" which is precisely
the question I attempt to answer here.

Before performing any analysis, we need to define the random variables
we care about. In liar's dice, this is the quantity of a certain number
$k$ between one and six on the table in any given hand, double counting
ones per the rules of the game. We call this quantity $S_{n, k}$ and
define it more formally as follows.

Let $X_i$ be the outcome of the $i$-th die role and define,

$$\begin{aligned}
  S_{n, 1} &= \sum_{i=1}^{n}\unicode{x1D7D9}[X_i = 1]\\
  S_{n, k} &= S_1 + \sum_{i=1}^{n}\unicode{x1D7D9}[X_i = k] \text{ for } k \in \{2 \ldots 6\}\end{aligned}$$

If the above definition is too opaque, just understand that $S_{n, k}$
is the quantity of the number $k$ rolled on the table in a given round
when $n$ dice are rolled.

What are we sure to see?
------------------------

The simplest observation we can make does not involve any probability
whatsoever. It is the observation that at least $5$ of a certain roll
are on the table in any given hand. It is a simple consequence of the
pigeonhole principle. We have 6 "holes" and 25 pigeons so there must be
at least $5$ pigeons in one hole. Unpacking the analogy implies that
when we roll 25 dice there must be at least $5$ of one number.

Of course, our situation is further complicated by the fact that we
double count 1s in our game. You can think that every time we land a
one, we really are putting a pigeon in every other hole. As we want to
minimize the number of pigeons in each hole, we never put any pigeon in
the hole corresponding to one. This, reduces the number of holes to $5$
and yields the following theorem which is easy to prove by contradiction
or by using the pigeonhole principle.

**Theorem** *Suppose there are $n$ dice rolled in a round of liar's dice. Then, we
have that that,*

$$\mathbb{P}[\exists k \in \{1 \ldots 6\} : S_{n, k} \geq \lceil\frac{n}{5}\rceil] =
  1$$ 

*Namely, it is always true that for some outcome between one and
six, there is at least $\lceil\frac{n}{5}\rceil$ of that outcome in our cups.*

Probabilistic Analysis
=========================

First, notice that $S_{n, 1}$ is the sum of $n$ independent Bernoulli
trials and is thus a binomial distribution with parameter
$p = \frac{1}{6}$. Also, notice that we can rewrite $S_{n, k}$ as
follows: 

$$S_{n, k} = S_1 + \sum_{i=1}^{n}\unicode{x1D7D9}[X_i = k]
= \sum_{i=1}^{n}\unicode{x1D7D9}[X_i = k \vee X_i = 1]$$ 

Which shows us that
$S_{n, k}$ is a binomial distribution with $p =
\frac{1}{3}$. From this, we can compute the probability that an outcome
has quantity greater than or equal to $m$ as,

$$P[S_{n,k} \geq m] = \sum_{i=m}^{n}P[S_{n,k} = i]
= \sum_{i=m}^{n}{n \choose i}
\left(\frac{1}{3}\right)^i\left(\frac{2}{3}\right)^{n-i}$$ 

However, I
will argue that this is not the quantity that we care about.

Liar's dice is a very human game in that success is rather dependent on
correctly predicting if your opponent is lying or telling the truth. As
such, I think it is reasonable to assume that players are more likely to
say outcomes that they have in their hand. This leads me to believe that
the event we care about is the probability that there is at least one
outcome with high quantity, namely,

$$\mathbb{P}[\exists k \in \{1 \ldots 6\} : S_{n,k} \geq m]$$ 

In the
first section we proved a deterministic lower bound on $m$ using the
pigeonhole principle, now we will look at larger values of $m$ and their
probabilities. Unfortunately, due to the lack of independence between
events and the large combinatorial nature of the problem, finding an
exact solution is tedious to say the least. Fortunately, for values of
$n$ and $k$ that we care about, we can find a relatively tight lower
bound on this probability by taking a look at the inclusion-exclusion
principle.

Lower bounding probability using the inclusion-exclusion principle
------------------------------------------------------------------

The inclusion-exclusion principle is a straightforward generalization of
the way we count the size of union of two sets. In the probabilistic
case it generalizes,
$$\mathbb{P}(A \cup B) = \mathbb{P}(A) + \mathbb{P}(B) - \mathbb{P}(A \cap B)$$.
to:

$$\mathbb{P}(\bigcup_{i=1}^nA_i) = \sum_{k=1}^{n}\left((-1)^{k-1}\sum_{\substack{I \subset \{1 \ldots n \} \\ |I|=k}}
  \mathbb{P}[A_I]\right) \text{ where } A_I = \bigcap_{i \in I}A_i$$

It is a very useful result, however, using it directly results in
analyzing the probabilities of $2^n$ intersections. Fortunately, it also
yields the following lower bound which we supply as a lemma:

**Lemma 1** *For any set of events $\{A_i\}_{i=1}^n$ we have that,*

$$\mathbb{P}(\bigcup_{i=1}^nA_i) \geq \sum_{i=1}^{n}\mathbb{P}[A_i] - \sum_{i = 1}^{n}\sum_{j=i+1}^{n}\mathbb{P}[A_i \cap A_j]$$

I won't prove the lemma here; it is an obvious consequence of set
intersections and is proved in several places [^2].

The probability of some outcome being large in quantity
-------------------------------------------------------

First let us assume that we are no longer counting duplicates. Then,
$S_{n,k}$ are binomial distributions with parameter $p = \frac{1}{6}$
and observe that,

$$\mathbb{P}[\exists k \in \{1 \ldots 6\} : S_{n,k} \geq m] = \mathbb{P}[\bigcup_{k=1}^m S_{n,k} \geq m]$$

We can now use lemma 1 to lower bound this probability as,

$$\begin{aligned}
  \mathbb{P}[\bigcup_{k=1}^6 S_{n,k} \geq m] &\geq \sum_{k=1}^{6}\mathbb{P}[S_{n,k} \geq m]
                                        - \sum_{k=1}^{6}\sum_{j=k+1}^{6}\mathbb{P}[S_{n,k} \geq m, S_{n, j} \geq m]\\
  &= 6\cdot\mathbb{P}[S_{n,1} \geq m] - 15\cdot\mathbb{P}[S_{n,1} \geq m, S_{n, 2} \geq m]\end{aligned}$$

Because we already have the first term in (2) bounded, all that is left
is to compute the last term. The last term is easy to compute if we know
$\mathbb{P}[S_{n,i}=m_i, S_{n,j} = m_j]$ which we can acquire from a
simple counting argument.

Observe that whenever $S_{n,i} = m_i \text{ and } S_{n,j} = m_j$, there
is a set of $m_i + m_j$ dice with outcomes $i$ or $j$ and there are
${n \choose m_i + m_j}$ ways to pick this set of dice. Within this set
of $m_i + m_j$ dice, there must be $m_i$ dice taking outcome $i$ and
$m_j$ dice taking outcome $j$. And there are another
${m_i + m_j \choose m_i}$ ways that this can happen. Putting this all
together noting that the
$\mathbb{P}[X_k \neq i, X_k \neq j] = \frac{4}{6} = \frac{2}{3}$ yields,

$$\mathbb{P}[S_{n,i} = m_i, S_{n,j} = m_j] = {n \choose m_i + m_j}{m_i + m_j \choose m_i}
\left(\frac{1}{6}\right)^{m_i+m_j}\left(\frac{2}{3}\right)^{n -
  (m_i+m_j)}$$ 

Now we finally have the inequality, which is very tight for large values of $m$ relative to $n$:

$$\begin{aligned}
Pr[\exists k \in \{1 \ldots 6\} : S_{n,k} \geq m] &\geq 6\sum_{i=m}^{n}{n \choose i}
\left(\frac{1}{6}\right)^i\left(\frac{5}{6}\right)^{n-i}\\ &-
                                                             15 \sum_{i=m}^{n}\sum_{j=m}^{n}{n \choose i + j}{i + j\choose i}
\left(\frac{1}{6}\right)^{i+j}\left(\frac{2}{3}\right)^{n - (i+j)}\end{aligned}$$

Defying expectations 
====================
Conflating expectation and probability is a typical error in
probabilistic thinking. In a round of liar's dice, one might be tempted
to call a bluff when someone deviates significantly from the expected
quantity of a certain outcome on the table, but this isn't always
reasonable to do.

Case 1: not double counting ones 
--------------------------------
Using the formula derived in the last section, we can show that we
defy expectations.

Say $n = 15$ dice are rolled on the table. The expected number of each
outcome is $2.5$. However, using our formula we can show that with
greater than $90\%$ probability, there are at least $4$ of some
outcome and that with greater than $50\%$ probability, there are at
least $5$ of some outcome. This defies our intuition as it is over
twice the expected value, yet it still has high probability!  Remember
this when betting on outcomes as they are often more probable than you
expect.

Below is a table where each entry is a lower bound on $P[\exists k \in
\{1 \ldots 6\} : S_{n,k} \geq m]$. For example, when rolling twenty
dice, the probability that there are at least 6 of one outcome is
greater than 55%. 

| Number of Rolls | $m \geq 4$ | $m \geq 5$ | $m \geq 6$ | $m \geq 7$ | $m \geq 8$ | $m \geq 9$ | $m \geq 10$ | $m \geq 11$ |
|:----------------|:-----------|:-----------|:-----------|:-----------|:-----------|:-----------|:------------|:------------|
| $n = 25$        | 1.00       | 1.00       | 0.88       | 0.58       | 0.26       | 0.093      | 0.0284      | 0.0074      |
| $n = 24$        | 1.00       | 0.98       | 0.85       | 0.50       | 0.20       | 0.070      | 0.0200      | 0.0048      |
| $n = 23$        | 1.00       | 0.92       | 0.79       | 0.42       | 0.16       | 0.051      | 0.0137      | 0.0031      |
| $n = 22$        | 1.00       | 0.91       | 0.72       | 0.34       | 0.12       | 0.036      | 0.0091      | 0.0019      |
| $n = 21$        | 1.00       | 0.90       | 0.64       | 0.28       | 0.09       | 0.025      | 0.0058      | 0.0011      |
| $n = 20$        | 1.00       | 0.89       | 0.55       | 0.21       | 0.06       | 0.017      | 0.0035      | 0.0006      |
| $n = 19$        | 1.00       | 0.85       | 0.46       | 0.16       | 0.04       | 0.010      | 0.0021      | 0.0003      |
| $n = 18$        | 0.99       | 0.79       | 0.37       | 0.12       | 0.03       | 0.006      | 0.0011      | 0.0001      |
| $n = 17$        | 0.99       | 0.70       | 0.29       | 0.08       | 0.02       | 0.003      | 0.0006      | 0.0         |
| $n = 16$        | 0.95       | 0.60       | 0.22       | 0.06       | 0.01       | 0.002      | 0.0002      | 0.0         |
| $n = 15$        | 0.90       | 0.50       | 0.16       | 0.03       | 0.00       | 0.001      | 0.0001      | 0.0         |
| $n = 14$        | 0.85       | 0.39       | 0.11       | 0.02       | 0.00       | 0.000      | 0.00        | 0.0         |
| $n = 13$        | 0.77       | 0.30       | 0.07       | 0.01       | 0.00       | 0.000      | 0.00        | 0.0         |
| $n = 12$        | 0.66       | 0.21       | 0.04       | 0.00       | 0.00       | 0.00       | 0.00        | 0.0         |
| $n = 11$        | 0.53       | 0.14       | 0.02       | 0.00       | 0.00       | 0.00       | 0.00        | 0.0         |
| $n = 10$        | 0.40       | 0.09       | 0.01       | 0.00       | 0.00       | 0.00       | 0.00        | 0.0         |
| $n = 9$         | 0.28       | 0.05       | 0.00       | 0.00       | 0.00       | 0.00       | 0.00        | 0.0         |
| $n = 8$         | 0.18       | 0.02       | 0.00       | 0.00       | 0.00       | 0.00       | 0.00        | 0.0         |
| $n = 7$         | 0.10       | 0.01       | 0.00       | 0.00       | 0.00       | 0.00       | 0.00        | 0.0         |
| $n = 6$         | 0.05       | 0.00       | 0.00       | 0.00       | 0.00       | 0.00       | 0.00        | 0.0         |
| $n = 5$         | 0.02       | 0.00       | 0.00       | 0.00       | 0.00       | 0.00       | 0.00        | 0.0         |

Case 2: double counting ones 
----------------------------
I have to re-derive the formula for the case where ones are wild. As
it took a while to derive the inequality in the non-wild case, I'm
going to leave this for later.

Python Code
----------

If you want to play with this yourself, you can use the following
python code to compute the lower bound in the case where ones are not
double counted.

```python
import math

def p_exists_no_repeats(n, m):
    s1 = 0
    for i in range(m, n + 1):
        s1 += math.comb(n, i) * math.pow(1/6, i) * math.pow(5/6, n - i)
    s1 *= 6

    s2 = 0
    for i in range(m, n + 1):
        for j in range(m, n + 1):
            c = math.comb(n, i + j) * math.comb(i + j, i)
            pr = math.pow(1/6, i +j) * math.pow(2/3, n - (i + j))
            s2 += c * pr

    s2 *= 15

    return s1 - s2
```

[^1]: [Liar's Dice Wikipedia Entry](https://en.wikipedia.org/wiki/Liar%27s_dice)

[^2]: [Inequalities associated with the Principle of Inclusion and Exclusion](https://sms.math.nus.edu.sg/smsmedley/Vol-19-2/Inequalities%20associated%20with%20the%20Principle%20of%20Inclusion%20and%20Exclusion(KM%20Koh).pdf)
