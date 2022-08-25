---
layout: post
title: RL Course by David Silver - Lecture 6
date: 2022-08-25 11:59:59 +0900
categories: RL
permalink: /18
---

# [RL Course by David Silver - Lecture 7](https://www.youtube.com/watch?v=KHZVXao4qXs&list=PLqYmG7hTraZDM-OYHWgPebj2MfCFzFObQ&index=7)


## Policy gradient

---

### Introduction

In the last lecture we approximated the value or action-value function using parameter $\theta$, <br>
$V _{\theta}(s)\approx V ^{\pi}(s)$ <br>
$Q _{\theta}(s,a)\approx Q ^{\pi}(s,a)$ <br>

A policy was generated directly from the value funciton (e.g. using $\epsilon$-greedy) <br>

In this lecture we will directly parametrize the policy <br>
$\pi _{\theta}(s,a) = P[a \|s, \theta]$ <br>
We will focus again on model-free RL <br>

**Value-based and policy-based RL** <br>
![](/public/img/2022-08-25-RLCoursebyDavidSilver-Lecture7/1.png){: width="50%" height="50%"}{: .center} <br>

**Advantages of policy-based RL** <br>
Advantages: <br>
-Better convergence properties <br>
-Effective in hihg-dimensional or continous action spaces <br>
-Can learn stochastic polices <br>
Disadvantages: <br>
-Typically converge to a local rather than global optimum <br>
-Evaluating a policy is typically inefficient and high variance <br>

---

### Finite difference policy gradient

---

### MC policy gradient 

---

### Actor-critic policy gradient

---