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

**Modified policy iteration** <br>
Does policy evaluation need to converge to $v _{\pi}$? <br>
Why not update policy every iteration? i.e. stop after $k=1$ <br>
This is equivalent to *value iteration* <br>

---

### Value iteration

**Principle of optimality** <br>
Any optimal policy can be subdivided into two components: <br>
An optimal first action $A _{\*}$ <br>
Followed by an optimal policy from successor state $S'$ <br>

Theorem (Principle of Optimality) <br>
A policy $\pi(a\|s)$ achieves the optimal value from state $s$, $v _{\pi}(s) = v _{\*}(s)$, iff <br>
For any state $s'$ reachable from $s$ <br>
$\pi$ achieves the optimal value from state $s'$, $v _{\pi}(s')=v _{\*}(s')$ <br>

**Deterministic value iteration** <br>
If we know the solution to subproblems $v _{\*}(s')$ <br>
Then solution $v _{\*}(s)$ can be foudn by one-step lookahead <br>
$v _{\*}(s)\leftarrow \underset{a\in A}{max}R _s^a + \gamma \sum _{s'\in S} P _{ss'}^a v _{\*}(s')$ <br>
The idea of value iteration is to apply these updates iteratively <br>
Intuition: start with final rewards and work backwards <br>
Still works with loopy, stochastic MDPs <br>

**Value iteration** <br>
Problem: find optimal policy $\pi$ <br>
Solution: iterative application of Bellman optimality backup <br>
Using synchronous backups <br>
convergence to $v _{\*}$ will be proven later <br>
**Unlike policy iteration, there is no explicit policy** <br>
Intermediate value functions may not correspond to any policy <br>

**Example of value iteration in practice** <br>
[value iteration demo](http://www.cs.ubc.ca/~poole/demos/mdp/vi.html) <br>

Problem | Bellman equation | Algorithm
--|--|--
Prediction | Bellman expectation equation | Iterative policy evaluation
Control | Bellman expectation equation + Greedy policy improvement | Policy iteration
Control | Bellman Optimality equation | Value iteration

Algorithms are based on state-value function $v _{\pi}(s)$ or $v _{\*}(s)$<br>
Complexcity $O(mn^2) per iteratoin, for $m$ actions and $n$ states <br>
Could aso aply to action-value function $q _{\pi}(s,a)$ or $q _{\*}(s,a)$ <br>
Complexity $O(m^2 n^2)$ per iteration <br>
(more expensive version, but still used in **model-free prediction**) <br>

---

### Extensions to dynamic programming

**Asynchronous DP** <br>
In-place dynamic programming <br>
Prioritised sweeping <br>
Real-time dynamic programming <br>

**In-place dynamic programming**
Synchronous value iteration stores two copies of value function (variable for old/new value) <br>
In-place value iteration only stores one copy of value function <br>

**Prioritised sweeping** <br>
In-place dynamic programming with prioritised order <br>
Use magnitude of Bellman error to guide state selection, e.g. <br>

**Real-time dynamic programming** <br>
Idea: only states that are relevant to agent <br>
Use agent's experience to guide the selection of states <br>

**Full-width backups** <br>
DP uses full-diwth backups <br>
For each backup (sync or async) every successor state and action is considered using knowldege of the MDP transitions and reward function <br>
For large problems DP suffers Bellman's curse of dimensionality <br>
Even one backup can be too expensive

**Sample backups** <br>
In subsequent lectures we will consider **sample backups** (DP to model-free)<br>

Using sample rewards and sample transitions $<S,A,R,S'>$ <br>
Instead of reward function $R$ and transitoin dynaimcs $P$ <br>

**Approximate Dynamic programming** <br>
Approximate the value function <br>
using a funciton approximator $\hat{v}(s,w)$ <br>
Apply dynamic programming to $\hat{v}(s,w)$ <br>

e.g. Fitted Value iteration repeats at each iteration $k$ <br>
Sample states $\tilde{S}\subseteq S$ <br>
For each state $s\in \tilde{S}$, estimate target value using Bellman optimality equation, <br>
$\tilde{v} _k (s) = \underset{a\in A}{\max}(R _s^a + \gamma \sum _{s'\in S} P _{ss'}^a \hat{v}(s', w _k))$ <br>
Train next value function $\hat{v}(\cdot, w _{k+1})$ using targets $\{<s, \tilde{v} _k (s)>\}$ <br>

---

### Contraction mapping

**Some technical questions** <br>
* How do we know that value iteration converges to $v _{\*}$?
* Or that iterative policy evaluation converges to $v _{\pi}>$
* And therfore that policy iteration converges to $v _{\*}$?
* Is the solutoin unque?
* How fast do these algorithms converge? <br>
  
These questions are resolved by **contraction mapping theorem**

---

### Questions

1. Bellman equation (or dynamic programming) 이 Local maximum 에 빠지지 않는다는 보장이 무엇인가요? <br>
   - [stackoverflow](https://stackoverflow.com/questions/20388453/global-minima-and-dynamic-programming) <br>



