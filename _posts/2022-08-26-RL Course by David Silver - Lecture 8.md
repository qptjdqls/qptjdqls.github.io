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

**Examples of models** <br>
-Table lookup model <br>
-Linear expectation model <br>
-Linear Gaussian model <br>
-Gaussian process model <br>
-Deep belif network model <br>

**Table lookup model** <br>
Model is an explicit MDP, $\hat{P}, \hat{R}$ <br>
Count visits $N(s,a)$ to each state action pair <br>
$\hat{P} _{s,s'}^a = \frac{1}{N(s,a)}\sum _{t=1}^T 1 (S _t, A _t, S _{t+1}=s,a,s')$ <br>
$\hat{R} _{s,s'}^a = \frac{1}{N(s,a)}\sum _{t=1}^T 1 (S _t, A _t=s, a)R _t$ <br>
Alternatively <br>
-At each time-step $t$, record experience tuple $<S _t, A _t, R _{t+1}, S _{t+1}>$ <br>
-To sample model, randomly pick tuple match $<s,a,\cdot, \cdot>$ <br>

**AB Example** <br>

![](/public/img/2022-08-26-RLCoursebyDavidSilver-Lecture8/2.png){: width="50%" height="50%"}{: .center} <br>

**Planning with a model** <br>
Given a model $M _n = <P _n, R _n>$ <br>
Solve the MDP $<S, A, P _{\eta}, R _{\eta}>$ <br>
Use favourite planning algorihtm <br>
-Value iteration <br>
-Policy iteration <br>
-Tree search <br>

**Sample-based planning** <br>
A simple but powerful approach to planning <br>
Use the model only to generate samples <br>
**Sample** experience from model (agent 가 미래에 어떻게 될 지 상상하는 것으로 생각할 수도 있다) <br>
$S _{t+1} \sim P _{\eta}(S _{t+1}\|S _t, A _t)$ <br>
$R _{t+1} = R _{\eta}(R _{t=1}\|S _t, A _t)$ <br>
Apply **model-free** RL to samples, e.g. <br>
-MC control <br>
-Sarsa <br>
-Q-learning <br>
Sample-based planning methods are often more efficient <br>

**Back to the AB example** <br>

![](/public/img/2022-08-26-RLCoursebyDavidSilver-Lecture8/3.png){: width="50%" height="50%"}{: .center} <br>
Model 을 생성하고, 생성된 모델에서 sampling 을 하고, 해당 sample 을 이용해 model-free RL (여기서는 MC 를 이용) <br>

**Planning with an inaccurate model** <br>
Given an imperfect model $<P _{\eta}, R _{\eta}>\neq <P, R>$ <br>
Performance of model-based RL is limited to optimal policy for approximate MDP $<S,A,P _{\eta}, R _{\eta}>$ <br>
i.e. Model-based RL is only as good as the estimated model <br>
When the model is inaccurate, planning process will compute a suboptimal policy <br>
Solution 1: when model is wrong, use model-free RL <br>
Soultion 2: reason explicity about model uncertainty (i.e. bayesian approach) <br>

---

### Integrated architectures

**Real and simulated experience** <br>
We consider two sources of experience <br>
Real expereince sampled from environment (true MDP) <br>
$S'\sim P _{s,s'}^a$ <br>
$R = R _s ^a$ <br>
Simulated experience sampled from model (approximate MDP) <br>
$S' \sim P _{\eta}(S' \|S, A)$ <br>
$R = R _{\eta}(R \|S, A)$ <br>

**Integrating learning and plannin** <br>
Model-based RL (using sample-based planning) <br>
-learn a model from real experience <br>
-plan value function (and/or policy) from simulated experience  <br>

Dyna <br>
-Learn a model from real experience <br>
-Learn and plan value function (and/or policy) from real and simulated experience <br>

**Dyna architecture** <br>
![](/public/img/2022-08-26-RLCoursebyDavidSilver-Lecture8/4.png){: width="50%" height="50%"}{: .center} <br>

**Dyna-Q algorithm** <br>
![](/public/img/2022-08-26-RLCoursebyDavidSilver-Lecture8/5.png){: width="50%" height="50%"}{: .center} <br>

---

### Simulation-based search



---



