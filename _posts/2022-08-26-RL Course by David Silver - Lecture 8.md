---
layout: post
title: RL Course by David Silver - Lecture 8
date: 2022-08-26 11:59:59 +0900
categories: RL
permalink: /20
---

# [RL Course by David Silver - Lecture 7](https://www.youtube.com/watch?v=ItMutbeOHtc&list=PLqYmG7hTraZDM-OYHWgPebj2MfCFzFObQ&index=8)


## Integrating learning and planning

---

### Introduction

Last lecture: learn policy directly from experience <br>
Previous lectures: learn value function directly from experience <br>
*This lecture*: learn **model** directly from experience <br>
and use **planning** to construct a value funciton or policy <br>
Integrate learning and planning in to a single architegcture <br>

**Model-Based and Model-free RL** <br>
Model-free RL <br>
-No model <br>
-Learn value function (and/or policy) from experience <br>

Model-based RL <br>
-learn a model from experience <br>
-Plan value function (and/or policy) from model <br>

---

### Model-based reinforcement learning

![](/public/img/2022-08-26-RLCoursebyDavidSilver-Lecture8/1.png){: width="50%" height="50%"}{: .center} <br>

**Advantages of Model-based RL** <br>
Advantages: <br>
-Can efficiently learn model by supervised learning methods <br>
체스 같은 경우를 생각해보면, 체스가 한번 움직일때 마다 value function 과 policy 는 급격하게 변할 수 있다. <br>
반면 체스의 model 은 상대적으로 게임의 룰처럼 상당히 straightforward 한데, 이를 이용해 tree search 와 같은 planning 을 통해 value function 을 estimate 할 수 있다. <br>
-Can reason about model uncertainty <br>
Disadvantages: <br>
-First learn a model, then construct a value function <br>
=> two source of approximation error

**What is a model?** <br>
A model $M$ is a representation of an MDP $<S,A,P,R>$ parameterized by $\eta$ <br>
We will assume state space $S$ and action space $A$ are known <br>
so a model $M=<P _{\eta}, R _{\eta}>$ represents state transitions $P _{\eta} \approx P$ and reward $R _{\eta}\ approx R$ <br>
$S _{t+1} \sim P _{\eta}(S _{t+1}\| S _t, A _t)$ <br>
$R _{t+1} = R _{\eta}(R _{t+1} \| S _t, A _t)$ <br>
Typically assume conditional independence between state transitions and rewards <br>
$P[S _{t+1}, R _{t+1} \| S _t, A _t] = P[S _{t+1} \| S _t, A _t] P[R _{t+1} \| S _t, A _t]$ <br>

**Model learning** <br>
Goal: estimate model $M _n$ from experience ${S _1, A _1, R _2, \dots, S _T}$ <br>
This is a supervised learning problem <br>
$S _1, A _1 \to R _2, S _2$ <br>
$S _{T-1}, A _{T-1} \to R _t, S _T$ <br>
Learning $s,a\to r$ is a regression problem <br>
Learning $s,a\to s'$ is a densitiy estiamtion problem <br>
Pick loss function, e.g. MSE, KL divergence, ... <br>
Find parameters $\eta$ that minimize empirical loss <br>

---

### Integrated architectures

---

### Simulation-based search

---



