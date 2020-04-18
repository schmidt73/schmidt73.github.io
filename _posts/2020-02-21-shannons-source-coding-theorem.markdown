---
layout: post
title: Why Entropy? (Part 1)
date: 2020-02-21 17:51
comments: true
categories: [Computer Science]
preview: "/assets/post_previews/shannon.jpeg"
---

> With few exceptions, when one is first handed the definition of
> entropy they are perplexed. In this series of posts, I hope to take
> you from this confusion to understanding as quickly as possible. I
> do this with Claude Shannon's help, through a presentation and proof
> of his famous Source Coding Theorem.

## Compressing dice rolls

Suppose that we roll a pair of dice and sum the result. There are 11
possible outcomes for the sum, namely the values 2 through 12, and the
outcomes appear with varying probability.  Now, imagine that we want
to send the outcome of repeated runs of our dice game over a wire.  If
we are not being very clever, we would need $$\lceil\log_2{11}\rceil =
4$$ bits for each run, however, if we are willing to accept some
error, we can use fewer bits.

In our case, let us ignore the outcomes 2, 3, and 12. Then, we only
need $$\lceil\log_2{8}\rceil = 3$$ bits to send the result of our game
over the wire.  Notice that this is a significant improvement on the
amount of bits sent, but we now make an error on 4/36 of dice rolls.
The natural question then is, how well can we compress our data while
maintaining a low probability of error?  It turns out that the answer
is closely tied to the entropy of our data generating distribution.

## Data generating distributions and smallest $$\delta$$-sufficient subset

We define a **data generating distribution** $$X$$ [^1] to be a discrete
random variable over the alphabet $A_X$ where,

$$A_X = \{a_1, ..., a_n\},\\
  P_X = \{p_1, ..., p_n\},\\
  \text{and}\hspace{.1cm} Pr[X = a_i] = p_i.$$

Letting the sum of two die rolls be our data generating distribution as
in the previous example, we have

$$A_X = \{2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12\}\\ P_X = \{\frac{1}{36},
  \frac{2}{36}, \frac{3}{36}, \frac{4}{36}, \frac{5}{36},
  \frac{6}{36}, \frac{5}{36}, \frac{4}{36}, \frac{3}{36},
  \frac{2}{36}, \frac{1}{36}\}$$

The compression strategy used in the die roll example set our error
$\epsilon = \frac{4}{36}$ and found a compression rate of 3 bits. For
analyzing how well we can compress our data, it will be useful to define more
formally where the rate of 3 bits comes from. 

We define the **compression rate** at
$\epsilon$-error for our data generating distribution as follows,

$$H_\epsilon(X) = \min_{S\ \in\ A_X(\delta)} \log_2{\vert S\vert}\\
  \text{where, } A_X(\delta) = \{A \subseteq A_X : \sum_{a \in A}Pr[X
  = a] \geq 1 - \epsilon\}.$$

Plugging in our dice example where we allow an error rate of $\epsilon = \frac{4}{36}$
we have that $$\{4, 5, 6, 7, 8, 9,
10\} \in A_X(\epsilon)$$ as,

$$\sum_{i=4}^{10}P[X=i] = \frac{3 + 4 + 5 + 6 + 5 + 4 + 3 + 2}{36} =
\frac{32}{36} \geq 1 - \frac{4}{36}$$

One interpretation of above is that if you randomly draw from our data
generating distribution, only $\epsilon$ of the time will you land
outside of the subset $$\{4, 5, 6, 7, 8, 9, 10\}$$. Following this,
then yields an interpretation of $$A_X(\epsilon)$$ is it contains all such
subsets of $A_X$ with this property $1 - \epsilon$ probability. An algorithm
to do so is included below:

```python
import numpy as np

def H(X, epsilon):
    AX, PX = X
    P = np.sort(PX)
    delta, i = 0, 0
    while delta < epsilon:   
        delta += P[i]
        i += 1
    return np.log2(len(AX) - i)
   
''' Sum of two dice example '''
A_X = [2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
P_X = [1/36, 2/36, 3/36, 4/36, 5/36, 6/36, 5/36, 4/36, 3/36, 2/36, 1/36]
X   = (A_X, P_X)

''' Outputs 3.0 as expected '''
print(H(X, 4/36)) 
```

Fortunately, for any finite alphabet, $H_\epsilon(X)$ is rather easy to compute
for any value of epsilon. Simple

Proving that
$$H_{\epsilon}(X) \leq \log_2\vert8\vert = 3$$.

## Typical Sets

We will start by defining entropy in the standard way. Let $$X$$ be a
discrete random variable distribution over an input alphabet $$A =
\{a_1, a_2, ..., a_n\}$$. Then the entropy of our distribution denoted
$$H(X)$$:

$$H(X) = \sum_{i=1}^np_i\log(\frac{1}{p_i})$$

[^1]: Information Theory, Inference and Learning Algorithms
