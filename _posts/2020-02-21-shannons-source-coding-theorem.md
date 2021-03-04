---
title: Why Entropy? (Part 1 of 3 on Shannon's Source Coding Theorem)
date: 2020-04-29 16:32
comments: true
categories: [Computer Science]
visible: 0
---

With few exceptions, when one is first handed the definition of
entropy they are perplexed. In this series of posts, I hope to take
you from this confusion to understanding as quickly as possible. I
do this with Claude Shannon's help, through a presentation and proof
of his famous Source Coding Theorem.

Compressing dice rolls using a simple strategy
=============================================

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
maintaining a low probability of error using this strategy?  It turns
out that the answer is closely tied to the entropy of our data
generating distribution.

## Data generating distributions and $$\epsilon$$-compression rates

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
analyzing how well we can compress our data using this strategy, it
will be useful to define more formally where the rate of 3 bits comes
from.

We define the **best compression rate** at $\epsilon$-error using our
strategy as follows,

$$H_\epsilon(X) = \min_{S\ \in\ A_X(\epsilon)} \log_2{\vert S\vert}\\
  \text{where, } A_X(\epsilon) = \{A \subseteq A_X : \sum_{a \in A}Pr[X
  = a] \geq 1 - \epsilon\}.$$

To make sense of the preceding definition, start by understanding the
set $A_X(\epsilon)$.  In informal language, $A_X(\epsilon)$ represents
all subsets of $A_X$ where the probability mass of each subset is at
least $1 - \epsilon$. That is for any subset of $U \subset
A_X(\epsilon)$, the probability of *not* landing in $U$ is in the
worst case $\epsilon$.

$$H_\epsilon(X)$$ is then defined as the number of bits required to
represent all elements in the smallest of those sets, where we allow
the number of bits to be non-integral for later analysis. Notice that
if we require no error in our compression scheme, then we make no
gains and $$H_0(X) = \log_2{\vert A_X \vert}$$, while if we allow any
amount of error, then we don't need any bits to compress our data and
$$H_1(X) = 0$$. 

To make things more concrete, lets look at the compression rate of our
dice example where we allow an error rate of $\epsilon = \frac{4}{36}$.
Notice that $$\{4, 5, 6, 7, 8, 9, 10, 11\} \in A_X(\epsilon)$$ as,

$$\sum_{i=4}^{11}P[X=i] = \frac{3 + 4 + 5 + 6 + 5 + 4 + 3 + 2}{36} =
\frac{32}{36} \geq 1 - \frac{4}{36}$$

This upperbounds the compression rate at $log_28 = 3$, which in this
case, is the compression rate (you can verify this yourself).

### An algorithm for finding the compression rate

The mathematical formulation is easy to understand, but it looks at
all $2^n$ subsets to compute the compression rate. A faster way to
compute the rate is to greedily remove letters from the alphabet of
lowest probability until no more letters can be removed and return
the number of bits required to represent the remaining letters. Code
that does this in python is included below:

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
## Maintaining a low error while improving compression rate

In the previous example, we achieved a compression rate of 3 bits with
an error of $$\epsilon = \frac{4}{36}$$. It is a good start, but our
error is much higher than we would have liked. Unfortunately, when
compressing only one symbol, we can't do any better: the discreteness
of the problem requires that our error changes in increments of at
minimum $$\frac{1}{36}$$. However, we can fix this issue if we allow
ourselves to compress *strings* of symbol from our data generating
distribution rather than individual symbols. That is, instead of
compressing one dice roll, we allow ourselves to compress a sequence
of $$N$$ independent dice rolls simultaneously. This helps us because
as $N$ increases, the probability of any specific string occuring
decreases, allowing us to fine tune the error ever more slightly.

We define $$X^N$$ as the **data generating distributing** of strings
of length $N$ where each letter of the string is drawn independently
from the distribution $X$. Our hope then, is that we can fix a small
$$\epsilon > 0$$ such that as $$N$$ goes to infinity, $$H_\epsilon(X^N)$$ small.

Why might one believe that they can expect some improvement in our
compression scheme by increasing $$N$$? I urge you to think a little
about this before continuing on. Well, we know that at the very least,
our compression rate at a specific rate of error will not decrease.



[^1]: Information Theory, Inference and Learning Algorithms
