---
layout: post
title: RL Course by David Silver - Lecture 2
date: 2022-08-15 11:59:59 +0900
categories: RL
permalink: /9
---

# [RL Course by David Silver - Lecture 2](https://www.youtube.com/watch?v=lfHX2hHRMVQ&list=PLqYmG7hTraZDM-OYHWgPebj2MfCFzFObQ&index=2)


## Markov Decision Process

---

### Markov Processes

Markov decision processes formally describe an environment for RL where the environment is fully observable <br>
Partially observable problems can be converted into MDPs <br>
Bandits are MDPs with one state <br>
$P[S _{t+1} \| S _t] = P[S _{t+1} \| S _1, \dots, S _t]$ <br>
the state capture all relevant information from the history <br>

**State Transition Matrix** <br>
$P _{ss'} = P[S _{t+1}=s' \| S _t=s]$ <br>

A **Markov Process** (or Markov Chain) is a tuple $<S,P>$ <br>
$S$ is a (finite) set of states <br>
$P$ is a state transition probability matrix <br>

---

### Markov Reward Processes

A markov reward process is a markov chain with values <br>
A Markov reward process is a tuple $<S,P,R,\gamma>$ <br>
$R$ is a reward function $R _s=E[R _{t+1} \| S _t=s]$ <br> 
$\gamma$ is a discount factor, $\gamma \in [0,1]$ <br>

The return $G _t$ is the total discounted reward from time-step $t$ <br>
$G _t = R _{t+1} + \gamma R _{t+2} + \dots = \sum _{k=0}^\infty \gamma^k R _{t+k+1}$ <br>

**Why discount?** <br>
Mathematically convenient to discount rewards <br>
Avoids infinite returns in cyclic Markov processes <br>
Uncertainty about the future may not be fully represented <br>
If the reward is financial, immediate rewards may earn more interest than delayed rewards <br>
Animal/human behaviour shows preference for immediate reward <br>
It is sometimes possible to use undiscounted Markov reward processes if all sequences terminate <br>

**Value Function** <br>
The value function $v(s)$ gives the long-term value of state $s$ <br>
The state value function $v(s)$ of an MRP is the expected return starting from state $s$ <br>
$v(s)=E[G _t \| S _t =s]$ <br>

**Bellman Equation for MRPs** <br>
immediate rewrad $R _{t+1}$ <br>
discounted value of successor state $\gamma v(S _{t+1})$ <br>
$v(s)=E[R _{t+1}+\gamma G _{t+1} \| S _t=s]$ <br>
$=E[R _{t+1}+\gamma v(S _{t+1}) \| S _t=s]$ (Law of iterated expectation $E[G _{t+1}]=E[E[G _{t+1} \|S _{t+1}]]$) <br>
$v(s) = R _s + \gamma \sum _{s'\in S} P _{SS'} v(s')$ <br>

The Bellman equation can be expressed concisely unsing matrices, <br>
$v = R + \gamma P v$ <br>
The Bellman equation is a linaer equation, and it can be solved directly: <br>
$v = (I-\gamma P)^{-1} R$ <br>
There are many iterative methoeds for large MRPs e.g. <br>
Dynamic programming <br>
Monte-Carlo evaluation <br>
Temporal-Difference learning <br>

---

### Markov Decision Processes

A Markov dicision process (MDP) is a Markov reward process with decision. It is an *environment* in which all states are Markov. <br>
A Markov decision process is a tuple $<S,A,P,R,\gamma>$ <br>
$A$ is a finite set of actions <br>
$P$ is a state transition probability matrix, $P _{SS'}^a = P[S _{t+1}=s'\| S _t=s, A _t=a]$ <br>

**Polices** <br>
A *policy* $\pi$ is a distribution over actions given states, <br>
$\pi(a\|s)=P[A _t=a| S _t=s]$ <br>
MDP policy depend on the current state (not the history) <br>
Policies are stationary (time-independent) <br>
The state and reward sequence $S _1, R _2, S _2, \dots$ is a Markov reward process $<S,P^{\pi},R^{\pi},\gamma>$ where <br>
$P _{s,s'}^\pi = \sum _{a\in A} \pi(a\|s) P _{s,s'}^a$ <br>
$R _s^\pi = \sum _{a\in A} \pi(a\|s) R _s^a$ <br>

**Value function** <br>
The state-value function $v _\pi (s)$ of an MDP is the expected return starting from state $s$, and then following policy $\pi$ <br>
$v _\pi (s) = E _{\pi}[G _t \| S _t=s]$ <br>
The action-value function $q _\pi (s, a)$ is the expected return starting from state $s$, taking action $a$, and then following policy $\pi$ <br>
$q _{\pi}(s,a) = E _{\pi}[G _t\| S _t=s, A _t=a]$ <br>

**Bellman Expectation Equation** <br>
$v _{\pi}(s) = E _{\pi}[R _{t+1} + \gamma v _{\pi}(S _{t+1}) \| S _t=s]$ <br>
$q _{\pi}(s,a) = E _{\pi}[R _{t+1} + \gamma q _{\pi}(S _{t+1}, A _{t+1}) \| S _t=s, A _t=a]$ <br>

$v _\pi(s) = \sum _{a\in A} \pi(a\|s) q _\pi (s,a)$ <br>
$q _\pi(s,a) = R _s^a + \gamma \sum _{s'\in S} P _{ss'}^a v _{\pi}(s')$ <br>

$v _{\pi}(s) = \sum _{a\in A}\pi(a\|s)(R _s^a + \gamma \sum _{s'\in S} P _{ss'}^a v _{\pi} (s'))$ <br>
$q _{\pi}(s,a) = R _s^a + \gamma \sum _{s'\in S}P _{ss'}^a \sum _{a'\in A} \pi(a' \| s') q _{\pi}(s',a')$ <br>

**Optimal value function** <br>
The optimal state-value function $v _* (s)$ is the maximum value funciton over all polices <br>
$v _* (s)=\underset{\pi}{\max} v _{\pi} (s)$ <br>
The optimal action-value function $q _* (s)$ is the maximum value function over all polices <br>
$q _* (s,a) = \underset{\pi}{\max} q _{\pi} (s,a)$ <br>

**Optimal Policy** <br>
Define a partial ordering over polices <br>
$\pi \geq \pi'$ if $v _\pi(s) \geq v _{\pi'}(s), \forall a$ <br>
For any Markov Decision Process <br>
There exists an optimal policy $\pi _\*$ that is better than or equal to all other policies, $\pi _\* \geq \pi, \forall \pi$ <br>
All optimal policies achieve the optimal value function, $v _{\pi _\*}(s)=v _\*(s)$ <br>
All optimal policies achieve the optimal action-value funciton, $q _{\pi _\*}(s,a)=q _\*(s,a)$ <br>

**Bellman Optimality Equation for $V _{\*}$** <br>
$v _\* (s) = \underset{a}{\max} q _\*(s,a)$ <br>
**Bellman Optimality Equation for $Q _{\*}$** <br>
$q _\* (s,a) = R _s^a + \gamma \sum _{s'\in S}P _{ss'}^a v _\*(s')$ <br>
**Bellman Optimality Equation for $V _{\*} (2)$** <br>
$v _\* (s) = \underset{a}{\max}R _s^a + \gamma \sum _{s'\in S}P _{ss'}^a v _\*(s')$ <br>
**Bellman Optimality Equation for $Q _{\*}$ (2)** <br>
$q _\* (s, a) = R _s^a + \gamma \sum _{s'\in S}P _{ss'}^a \underset{a'}{\max} q _\*(s',a')$ <br>

**Solving the Bellman Optimality Equation** <br>
Bellman optimality equation is non-linear <br>
No closed form solution (in general) <br>
Many iterative solution methods <br>
Value iteration, Policy iteration, Q-learning, Sarsa <br>

---

### Extensions to MDPs

Infinite and continous MDPs <br>
Partially obersvable MDPs <br>
Undiscounted, average reward MDPs <br>

(next class) <br>

---

### Questions

Everything is talking about maximing the reward without explicitly considering about <u>risk or variance of returns of MDP</u> <br>
Risk MDP 를 또다른 reward MDP 로 변환 하는 방법이 있고, 아니면 실제로 MDP 의 risk 나 variance 에 관한 연구도 있다. <br>