---
layout: post
title: Law of total probability, expectation, and variance
date: 2022-08-12 11:59:59 +0900
categories: DL
permalink: /7
---

# Law of total ...

## [Law of total probability](https://en.wikipedia.org/wiki/Law_of_total_probability) 

$P(A) = \sum _n P(A\cap B _n)$ <br>
$P(A) = \sum _n P(A \|B _n) p(B _n)$ <br>
$P(A) = \int _{-\infty}^{\infty} P(A\|X=x)dF _X (x)$ <br>
$P(A) = \int _{-\infty}^{\infty} P(A\|X=x)f _X (x) dx$ <br>

Marginalization
- $p(x) = \sum _y p(x,y)$
- $p(x) = \int _y p(x,y)dy$


## [Law of total expectation](https://en.wikipedia.org/wiki/Law_of_total_expectation)

$E(X) = E(E(X \| Y))$ <br>
the expected value of the conditional exepcted value of $X$ given $Y$ is the same as the expected value of $X$. <br>
$E(X) = \sum _i E(X \| A _i)P(A _i)$ <br>
Note: the conditional expected value $E(X \| Z)$ is a random variable whose value <u>depend on the value of $Z$</u>. <br>

### Proof in the finite and countable cases

$E(E(X \| Y))=E[\sum _x x\cdot P(X=x | Y)]$ <br>
$=\sum _y [\sum _x \cdot P(X=x \| Y=y)] \cdot P(Y=y)$ <br>
$=\sum _y \sum_x \cdot P(X=x, Y=y)$ <br>
if the series is infinte, then we can switch the summations around, and the previous expression will become <br>
$\sum _x, \sum_y x \cdot P(X=x, Y=y)=\sum _x x \sum _y P(X=x, Y=y)$ <br>
$=\sum _x x \cdot P(X=x) $ <br>
$=E(X)$ <br>
If, on the other hand, the serises is infinte, then its convergence annot be conditional, due to the assumption that $min(E[X _+], E[X _-])<\infty$. The series converges absolutely if both $E[X _+]$ and $E[X _-]$ are finite, and diverges to an infinity when either $E[X _+]$ or $E[X _-]$ is infininte. In both scenarios, the above summations may be exchanged without affecting th sum. <br>

### [Proof in the continuous case](https://math.stackexchange.com/questions/1353418/expected-value-proof-law-of-total-expectation)

$E _Y(E _X[X\|Y]) = E _X[X]$ <br>
where $E _Y$ is the expectation w.r.t. $Y$ and $E _X$ w.r.t. X. <br>
Let $S _X = supp(X)$ and $S _Y = supp(Y)$ <br>
$E _Y(E _X[X \|Y]) = \int _{S _Y} E _X[X \| Y] f(y)dy$ <br>
$E _Y(E _X[X \|Y]) = \int _{S _Y} \int _{S _X} x \frac{f(x,y)}{f(y)} dx f(y) dy$ <br>
$f(y)$ is constant w.r.t. $X$. Thus, <br>
$E _Y(E _X[X \|Y]) = \int _{S _Y} \int _{S _X} x f(x,y)dx dy$ <br>
By Fubini's theorem <br>
$E _Y(E _X[X \|Y]) = \int _{S _X} \int _{S _Y} x f(x,y)dy dx$ <br>
Now $X$ is constant w.r.t. $Y$. <br>
$E _Y(E _X[X \|Y]) = \int _{S _X} x \int _{S _Y} f(x,y) dy dx$ <br>
The inner integral is the margianlization from $f(x,y)$ to $f(x)$. Thus, <br>
$E _Y(E _X[X \|Y]) = \int _{S _X} x f(x)dx$ <br>
$=E _X[X]$ <br>
This proof also exists in a measure theoretic context which is more general. The Widipedia article is quite good. <br>

## [Law of total Variance](https://en.wikipedia.org/wiki/Law_of_total_variance)