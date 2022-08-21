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


---

## 원본 글: [Gitbook](https://dnddnjs.gitbooks.io/rl/content/reinforcement_learning.html)

강화학습이 기본적으로 MDP 로 정의 되는 문제를 풀기 때문에 state 는 Markov 라고 가정하고 접근한다. <br>
하지만 절대적인 것은 아니며 **Non-Markovian MDP** 도 존재하며 그러한 문제를 풀기 위한 강화학습들도 있지만 상대적으로 연구가 덜 되었으며 처음에 접하기에는 적합하지 않다. <br>

> 어떠한 문제를 강화학습으로 풀 수도 있고 다른 machine learning 기법으로 풀 수도 있기 때문에 강화학습을 적용시키기 전에 왜 강화학습을 써야하고 다른 머신러닝 기법에 비해서 나은 점이 무엇인가를 따져보고 사용해야 한다. <br>
> **강화학습은 "시간"이라는 개념이 있는 문제를 푸는 인공지능 기법이다.** <br>
> **이는 결국 강화학습의 목표가 Policy(일련의 행동들)이 된다는 의미를 함포한다.** <br>

Agent 입장에서 다음 행동을 다음으로 가능한 state 들의 value funciton 으로 판단하는데, 그러려면 다음 state 들에 대한 정보를 다 알아야 하고 그 state 로 가려면 어떻게 해야하는지도 알아야 한다. <br>
따라서 state 에 대한 value function 말고 action 에 대한 value funciton 을 구할 수 있는데 (action value function), 이를 사용하면 state value function 과 달리 단지 어떤 행동을 할지 action value function 의 값을 보고 판단하면 되기 때문에 다음 state 들의 value function 을 알고 어떤 행동을 했을 떄 거기게 도달하게 될 확률도 알아야하는 일이 사라진다. <br>

---

## 원본 글: [Gitbook](https://dnddnjs.gitbooks.io/rl/content/bellman_equation.html)

보통 우리가 부르는 Bellman equation 은 뒤에서 언급될 DP 같이 discrete 한 time 에서의 최적화 문제에 적용되는 식을 의미한다 <br>
하지만 시간이 연속적인 문제의 경우에는 (Hamilton-Jacobi-Bellman equation)[https://en.wikipedia.org/wiki/Bellman_equation] 이라고 부르는 식이 따로 존재한다. <br>

미래의 값들(next state-value function) 으로 현재의 value function 을 구한다는 것이 Back-up 이다. <br>
Backup 에는 one-step backup, multi-step backup 이 있고, 또한 Full-width backup 과 sample backup 이 있다. <br>
Full-width backup 은 DP, sample backup 은 RL 이다. <br>

The optimal state-value funciton $v _{\*}(s)$ is the maximum value funciton over all polices <br>
$v _{\*}(s) = \underset{\pi}{\max} v _{\pi}(s)$ <br>
The optimal action-value function $q _{\*}(s, a)$ is the maximum action-value function over all policies <br>
$q _{\*}(s, a) = \underset{\pi}{\max} q _ {\pi}(s,a)$ <br>

An optimal policy can be found by maximizing over $q _{\*} (s,a)$ <br>
$\pi _{\*}(a \|s)= 1$ if $a=\underset{a\in A}{arg\max}q _{\*}(s,a)$ <br>
$\pi _{\*}(a \|s)= 0$ otherwise <br>