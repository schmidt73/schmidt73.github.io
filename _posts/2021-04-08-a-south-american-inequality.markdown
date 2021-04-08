---
title: Proof of a Central American Inequality
date: 21-04-08 12:37
comments: true
categories: [math]
---

I present a short proof of a Central American inequality found in the
2003 Centro-American mathematics Olympiad. As my friend Oliver pointed
out, the inequality is rather intuitive. It states that exponentiation
is always larger than multiplication; the difficulty is proving it
holds for small values of $a$ and $b$.

<!--end_excerpt-->

**Theorem:** The inequality $a^b + 1 \geq b(a+1)$ holds for all
integers $a > 1$ and $b > 2$.

The trick here is to use a technique I call "double-induction". That
is, we fix some $a > 1$ and prove the claim inductively on $b > 2$ for
a fixed $a$. But we also perform induction on the base case of $b = 3$
for all $a$.

![Induction Diagram](/images/posts/central_american/induction_figure.png)

The induction strategy can be thought of as in the preceding
diagram. In induction one, we prove the claim for a fixed $b = 3$.
And in induction two, we use that as a base case to prove the claim
for all other values of $b$.

**Proof:** First we prove that the claim holds for all $a > 1$ when $b
= 3$. The base case of $a = 2$ is trivial as, $$2^3 + 1 = 9 \geq 3(2 +
1) = 9$$.

For the inductive step, suppose that $a^3 + 1 \geq 3(a+1)$ holds for
some $a$. Then we have,

$$
\begin{align*}
(a + 1)^3 + 1 &= a^3 + 3a^2 + 3a + 1 + 1\\
&\geq 3(a+1) + 3a^2 + 3a + 1 & \text{ by the inductive hypothesis }\\
&\geq 3(a+1) + 3 & \text{ as } 3a^2 + 3a > 1\\
&= 3(a+2) 
\end{align*}
$$

This concludes our first round of induction.

Now, fix $a > 1$ and suppose that $a^b + 1 \geq b(a + 1)$ for some
$b > 2$. As a reminder, we proved this is true for $b = 3$ in the
first round of induction. From some computation we then have,

$$
\begin{align*}
a^{b+1}+1 &= a^b*a + 1\\
&\geq [b(a+1)-1]a + 1 & \text{ by the inductive hypothesis }\\
&= [(b+1-1)(a+1) - 1]a + 1\\
&= [(b+1)(a+1) - (a + 1) - 1]a + 1\\
&= [(b+1)(a+1) - a - 2]a + 1\\
&= (b+1)(a+1)a - (a^2 + 2a) + 1\\
&= (b+1)(a+1) + (b+1)(a+1)(a-1) - (a^2 + 2a) + 1\\
&= (b+1)(a+1) + (b+1)(a^2 - 1) - (a^2 + 2a) + 1\\
&= (b+1)(a+1) + b(a^2-1) + (a^2 - 1) - a^2 - 2a + 1\\
&= (b+1)(a+1) + b(a^2-1) - 2a\\
&\geq (b+1)(a+1) + 1 & \text{ as } b(a^2-1) - 2a \geq 1
\end{align*}
$$

$\blacksquare$
