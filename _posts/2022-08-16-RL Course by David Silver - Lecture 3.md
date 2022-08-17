---
layout: post
title: RL Course by David Silver - Lecture 3
date: 2022-08-16 11:59:59 +0900
categories: RL
permalink: /10
---

# [RL Course by David Silver - Lecture 3](https://www.youtube.com/watch?v=Nd1-UUMVfz4&list=PLqYmG7hTraZDM-OYHWgPebj2MfCFzFObQ&index=3&t=2s)


## Planning by Dynamic Programming

---

### Introduction

Dynamic sequential or temporal component to the problem <br>
Programming optimising a "program", i.e. a policy <br>

**two properties of dynamic programming**<br>
Optimal substrurcture <br>
Overlapping subproblems <br>

Markov decision processes satisfy both properties <br>
Bellman equation gives recursive decomposition <br>
Value function stores and reuses solutions <br>

Dynamic programming assumese full knowledge of the MDP <br>
It is uesed for planning in an MDP <br>
*For prediction:* <br>
Input: MDP $<S,A,P,R,\gamma>$ and policy $\pi$ <br>
Output: value function $v _{\pi}$ <br>
*Or for control:* <br>
Input: MDP $<S,A,P,R,\gamma>$ <br>
Output: optimal value function $v _{\*}$ <br>

---

### Policy evaluation

Problem: evaluate a given policy $\pi$ <br>
Solution: iterative application of Bellman expectation backup <br>
Using *synchronous backups*, we consider all states at every step <br>
Convergence to $v\pi$ will be proven later <br>

**Iterative policy evaluation** <br>
(iterative) Bellman expectation equation <br>
$v _{k+1} = \sum _{a\in A} \pi(a \|s) (R _s^a + \gamma \sum _{s'\in S} P _{ss'}^a v _k(s'))$ <br>
$v^{k+1} = R^{\pi} +\gamma P^{\pi} v^k$ <br>

---

### Policy iteration

How do we make the better policy? <br>
Given a policy $\pi$ ... <br>
**Evaluate** the policy $\pi$ <br>
$v _{\pi} (s) = E[R _{t+1} + \gamma R _{t+2} + \dots]$ <br>
**Improve** the policy by acting greedily with respect to $v _{\pi}$ <br>
$\pi' = greedy(v _{\pi})$ <br>
Iterating improvement / evaluation. This process of policy iteration always converges to $\pi^{\*}$ <br>

![](/public/img/2022-08-16-RL%20Course%20by%20David%20Silver%20-%20Lecture%203/1.jpg){: width="50%" height="50%"}{: .center}

Consider a deterministic policy, $a=\pi(s)$ <br>
We can improve the policy by acting greedily <br>
$\pi'(s) = \underset{a\in A}{arg\max}q _{\pi}(s,a)$ <br>
This improves the value from any state $s$ over one step, <br>
$q _{\pi}(s, \pi'(s)) = \underset{a\in A}{\max}q _{\pi}(s,a) \geq q _{\pi}(s, \pi(s))=v _{\pi}(s)$ <br>
It therefore improves the value function, $v _{\pi'}(s)\leq v _{\pi}(s)$ <br>
$v _{\pi}(s) \leq q _{\pi}(s,\pi'(s))=E _{\pi '}[R _{t+1}+\gamma v _{\pi}(S _{t+1}) \| S _t = s]$ <br>
$\leq E _{\pi'}[R _{t+1} + \gamma q _{\pi}(S _{t+1}, \pi ' (S _{t+1})) \| S _t = s]$ <br>
$\leq E _{\pi '}[R _{t+1} + \gamma R _{t+2} + \gamma^2 q _{\pi}(S _{t+2}, \pi ' (S _{t+2})) \| S _t = s]$ <br>
$\leq E _{\pi '}[R _{t+1} + \gamma R _{t+2} + \dots \| S _t = s] = v _{\pi '} (s)$ <br>

If improvements stop,
$q _{\pi}(s, \pi '(s)) = \underset{a\in A}{\max} q _{\pi} (s,a) = q _{\pi}(s, \pi(s)) = v _{\pi}(s)$ <br>
Then the Bellman optimality equation (*which is the definition of the optimal policy*) has been satisfied <br>
$v _{\pi}(s) = \underset{a\in A}{\max} q _{\pi}(s,a)$ <br>
Therefore $v _{\pi}(s) = v _{\*}(s)$ for all $s\in S$ <br>


---

### Value iteration

---

### Extensions to dynamic programming

---

### Contraction mapping

---

### Questions

1. Bellman equation 이 Local maximum 에 빠지지 않는다는 보장이 무엇인가요? <br>
   - [stackoverflow](https://stackoverflow.com/questions/20388453/global-minima-and-dynamic-programming) <br>



