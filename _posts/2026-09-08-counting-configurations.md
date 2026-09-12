---
layout: post
title: 'Gaussian elimination and a rectangular counting problem'
date: 2026-09-11
published: True
---

It is presently the case that many previously difficult problems, e.g. the 
[Navier-Stokes problem](https://en.wikipedia.org/wiki/Navier%E2%80%93Stokes_existence_and_smoothness), 
are easily solved by invoking a modern LLM. However, we should not let this 
take away from the *simple* joy of solving a problem that is difficult for us. In this spirit,
here is one such problem, taken from *Mathematical Olympiad Treasures* by Andreescu and Enescu:

> In how many ways can one fill an $m \times n$ table with $\pm 1$ such that
> the product of the entries in each row and each column equals $-1$?

A concise solution to this problem is found in the referenced source, and the solution
proposed here will not differ substantially. However, we will take a slightly circuitous route
that highlights the power of abstraction and general-purpose tools in solving difficult problems.

<details class="solution-reveal" markdown="1">
<summary><span class="reveal-closed">Show solution</span><span class="reveal-open">Hide solution</span></summary>

**Solution.**
To start, let us notice that if we replace $1$ with $0$ and $-1$ with $1$ in the problem description, 
the problem reduces to counting the number of $m \times n$ binary matrices $A$ whose
row and column sums are odd. This is a trivial translation, but it suggests
that we consider the problem as a linear system.
Specifically, we aim to count the number of solutions $A$ to the linear equations

$$
\begin{align*}
\sum_{j=1}^nA_{ij} \equiv \sum_{i=1}^mA_{ij} \equiv 1 \text{ for all  } i \in [m], j \in [n],
\end{align*}
$$

where $a \equiv b$ if $a = b \pmod 2$. This is a system of linear equations over the finite field $\mathbf{F}_2$, and the standard 
linear algebraic tools apply; for example, see the 
[notes](https://sites.math.rutgers.edu/~sk1233/courses/ANT-F14/lec3.pdf).
We can count the number of solutions using Gaussian elimination. While this does not
directly answer the question, it at least shows that for fixed $m, n$, we can compute the
answer in polynomial time.

To illustrate this perspective, let us apply Gaussian elimination
(over $\mathbf{F}_2$) to solve the problem in the case where $n = m = 3$. 
The system has six equations and nine unknowns, corresponding to the 
entries of $A$:

$$\begin{array}{rcccccccccccl}
& A_{11} & A_{12} & A_{13}
& A_{21} & A_{22} & A_{23}
& A_{31} & A_{32} & A_{33} & & b & \\
\left(\vphantom{\begin{array}{c}1\\1\\1\\1\\1\\1\end{array}}\right.
& \begin{array}{c}1\\-\\-\\1\\-\\-\end{array}
& \begin{array}{c}1\\-\\-\\-\\1\\-\end{array}
& \begin{array}{c}1\\-\\-\\-\\-\\1\end{array}
& \begin{array}{c}-\\1\\-\\1\\-\\-\end{array}
& \begin{array}{c}-\\1\\-\\-\\1\\-\end{array}
& \begin{array}{c}-\\1\\-\\-\\-\\1\end{array}
& \begin{array}{c}-\\-\\1\\1\\-\\-\end{array}
& \begin{array}{c}-\\-\\1\\-\\1\\-\end{array}
& \begin{array}{c}-\\-\\1\\-\\-\\1\end{array}
& \left|\vphantom{\begin{array}{c}1\\1\\1\\1\\1\\1\end{array}}\right.
& \begin{array}{c}1\\1\\1\\1\\1\\1\end{array}
& \left.\vphantom{\begin{array}{c}1\\1\\1\\1\\1\\1\end{array}}\right)
\end{array}$$

Reducing the system to row echelon form and dropping redundant 
equations yields:

$$\begin{array}{rcccccccccccl}
& A_{11} & A_{12} & A_{13}
& A_{21} & A_{22} & A_{23}
& A_{31} & A_{32} & A_{33} & & b & \\
\left(\vphantom{\begin{array}{c}1\\1\\1\\1\\1\end{array}}\right.
& \begin{array}{c}1\\-\\-\\-\\-\end{array}
& \begin{array}{c}1\\1\\-\\-\\-\end{array}
& \begin{array}{c}1\\1\\1\\-\\-\end{array}
& \begin{array}{c}-\\1\\1\\1\\-\end{array}
& \begin{array}{c}-\\-\\1\\1\\-\end{array}
& \begin{array}{c}-\\-\\-\\1\\-\end{array}
& \begin{array}{c}-\\1\\1\\1\\1\end{array}
& \begin{array}{c}-\\-\\1\\1\\1\end{array}
& \begin{array}{c}-\\-\\-\\1\\1\end{array}
& \left|\vphantom{\begin{array}{c}1\\1\\1\\1\\1\end{array}}\right.
& \begin{array}{c}1\\0\\1\\0\\1\end{array}
& \left.\vphantom{\begin{array}{c}1\\1\\1\\1\\1\end{array}}\right)
\end{array}$$

The answer is then immediate: there are five dependent (pivot) variables and four free (non-pivot) 
variables. Thus, there are $2^{4}$ solutions to the system of equations. 
The useful information is not just the number of free variables, but their location: they form the bottom-right 
$2\times2$ submatrix. We can fill that rectangle arbitrarily, and the remaining entries are forced.
Specifically, noting that the free variables correspond to the bottom-right $2\times2$ submatrix of $A,$ 
the elimination procedure provides a
proof of the following claim:

> In a $3 \times 3$ binary matrix $A$, for each of the $2^4$ choices for the bottom-right $2 \times 2$ submatrix $A'$ of $A$, 
> there exists a *unique* setting of the remaining entries of $A$ such that the row and column sums
> of $A$ are odd.

This immediately prompts us to consider the following conjecture:

> In an $m \times n$ binary matrix $A$ with $m \equiv n$, for each of the $2^{(m-1)(n-1)}$ choices for the bottom-right $(m-1) \times (n-1)$ submatrix $A'$ of $A$, 
> there exists a *unique* setting of the remaining entries of $A$ such that the row and column sums
> of $A$ are odd.

<details class="solution-reveal" markdown="1">
<summary><span class="reveal-closed">Show conjecture proof and discussion</span><span class="reveal-open">Hide conjecture proof and discussion</span></summary>

To prove this conjecture, consider an arbitrary assignment to the bottom-right
submatrix $A'$ of $A$. Then, by the row and column constraints, the entries $A_{1, 2}, \ldots, A_{1, n}$ and $A_{2, 1}, \ldots, A_{m, 1}$
are uniquely determined, and all constraints except those for the first row and first column are satisfied. However,
these two remaining constraints may conflict, in which case no choice of $A_{1,1}$ will satisfy
both. Namely, the first-row and first-column constraints require that

$$A_{1,1} \equiv 1 - A_{1, 2} - \ldots - A_{1, n} \quad\text{and}\quad A_{1,1} \equiv 1 - A_{2, 1} - \ldots - A_{m, 1} $$

which yields a contradiction if $A_{1, 2} + \ldots + A_{1, n} \not\equiv A_{2, 1} + \ldots + A_{m, 1}$. Applying our constraints
to expand $A_{1, 2}, \ldots, A_{1, n}$ and $A_{2, 1}, \ldots, A_{m, 1}$ yields

$$A_{1, 2} + \ldots + A_{1, n} \equiv (n - 1) - \sum_{i=2}^{m}\sum_{j=2}^nA_{ij} \quad\text{and}\quad A_{2, 1} + \ldots + A_{m, 1} \equiv (m - 1) - \sum_{i=2}^{m}\sum_{j=2}^nA_{ij}$$

proving that the conjecture holds if $m$ and $n$ have the same parity. Conversely, if $m$ and $n$ have different parity,
there is no binary matrix $A$ whose row and column sums are odd. To see this, count in two ways by summing over the row and column
constraints. This completes the problem, proving that the answer is $2^{(m-1)(n-1)}$ if $m$ and $n$ have the same parity, 
and zero otherwise.

**Discussion.** We applied general-purpose linear algebraic machinery to a specific instance of the problem
and were led to a simple conjecture. The solution to the conjecture then solved the original counting problem. In this way, abstraction helped us to ask the *right* question,
after which the solution was immediate. I find this to be a compelling example that demonstrates the utility
of abstraction in solving concrete problems.

</details>

</details>
