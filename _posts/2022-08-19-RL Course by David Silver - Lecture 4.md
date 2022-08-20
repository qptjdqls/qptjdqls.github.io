---
layout: post
title: RL Course by David Silver - Lecture 4
date: 2022-08-19 11:59:59 +0900
categories: RL
permalink: /12
---

# [RL Course by David Silver - Lecture 4](https://www.youtube.com/watch?v=PnHCvfgC_ZA&list=PLqYmG7hTraZDM-OYHWgPebj2MfCFzFObQ&index=4&t=4ss)


## Model-Free Prdiction

---

### Introduction

Last lecture: <br>
planning by dynamic programming <br>
solve a known MDP <br>

This lecture: <br>
Model-free prediction <br>
<u>Estimate the value function</u> of an unknown MDP <br>

Next lecture: <br>
Model-free control <br>
<u>Optimize the value function</u> of an unknown MDP <br>

---

### Monte-Carlo Learning

**Monte-carlo reinforcement learning** <br>
MC method learn directly from episodes of experience <br>
MC learns from complete episdes: no bootstrapping <br>
value = mean return <br>
Caveat: can only apply MC to episodic MDPs <br>
all episodes must terminate <br>

**MC policy evaluation** <br>
$v _{\pi}(s)=E _{\pi}[G _t \| S _t = s]$ <br>
MC policy evaluation uses empirical mean return instead of expected return <br>

**Incremental Mean** <br>
$\mu _k = \frac{1}{k}\sum _{j=1}^{j} x _j$ <br>
$=\mu _{k-1} + \frac{1}{k}(x _k - \mu _{k-1})$ <br>
error term: $x _k - \mu _{k-1}$ <br>

**Incremental MC updates** <br>
Update $V(s)$ incrementally after episode <br>
$V(S _t) \leftarrow V(S _t)+\frac{1}{N(S _t)}(G _t - V(S _t))$ <br>
In non-stationary problems, it can be useful to track a running mean, i.e. forget old episodes. <br>
$V(S _t) \leftarrow V(S _t) + \alpha (G _t - V(S _t))$ <br>

---

### Temporal-Difference Learning

TD methods learn directly from episodes of experience <br>
TD is model-free: no knowldege of MDP transitions / rewards <br>
TD learns from *incomplete* episodes, by *bootstrpping* <br>
TD updates a guess towards a guess <br>

**MC and TD** <br>
Goal: learn $v _{\pi}$ online from experience under policy $\pi$ <br>

Incremental every-visit MC <br>
Update value $V(S _t)$ toward actual retrun $G _t$ <br>
$V(S _t) \leftarrow V(S _t) + \alpha (G _t - V(S _t))$ <br>

Simplest temporal-difference learning algorithm: $TD(0)$ <br>
Update value $V(S _t)$ toward estimated return $R _{t+1} + \gamma V(S _{t+1})$ <br>
$V(S _t) \leftarrow V(S _t) + \alpha (R _{t+1} + \gamma V(S _{t+1}) - V(S _t))$ <br>
$R _{t+1} + \gamma V(S _{t+1})$ is called *TD target* <br>
$\delta _t =R _{t+1} + \gamma V(S _{t+1}) - V(S _t)$ is called *TD error* <br>

**Advantages and disadvantages of MC vs TD (1)** <br>
TD can learn before knowing the final outcome <br>
TD can learn without the final outcome <br>

**Bias/Variance trade-off** <br>
Return $G _t=R _{t+1} + \gamma R _{t+2} + \dots + \gamma^{T-1}R _T$ is unbiased estimate of $v _\pi (S _t)$ <br>

'True TD target' $R _{t+1}+\gamma v _{\pi}(S _{t+1})$ is unbiased estiamte of $v _{\pi}(S _t)$ <br>
'TD target' $R _{t+1}+ \gamma V(S _{t+1})$ is biased estiamte of $v _{\pi} (S _t)$ <br>

TD target is much lower variance than the return: <br>
Return depends on many random actions, transitions, rewards <br>
TD target depends on one random action, transition, reward <br>

**Advantages and disadvantages of MC vs TD (2)** <br>
MC has high variance, zero bias <br>
Good convergence properties (even with funciton approximation) <br>

TD has low variance, some bias <br>
Usaually more efficient than MC <br>
$TD(0)$ converges to $v _{\pi}(s)$ <br>
(but not always with function approximation) <br>
More sensitive to initial value <br>

**Batch MC and TD** <br>
MC and TD convergeL $V(s) \rightarrow v _{\pi}(s)$ as experience $\rightarrow \infty$ <br>
But what about batch solution for finite experience? <br>

**Certainty Equaivalence** <br>
MC converges to solution wiht minimum mean-squeared error <br>
-Best fit to the observed returns <br>
$\sum _{k=1}^{K} \sum _{t=1}^{T _k} (g _t^k - V(s  _t^k))^2$ <br>

TD(0) converges to solution of max likelehood Markov model
-Solution to the MDP $<S,A,\hat{P},\hat{R}, \gamma>$ that best fits the data <br>
$\hat{P} _{s,s'}^a = \frac{1}{N(s,a)}\sum _{k=1}^{K} \sum _{t=1}^{T _k} 1(s _t^k, a _t^k, s _{t+1}^k = s,a,s')$ <br>
$\hat{R} _s^a = \frac{1}{N(s,a)}\sum _{k=1}^{K} \sum _{t=1}^{T _k} 1(s _t^k, a _t^k = s,a)r _t^k$ <br>

**Advantages and disadvantages of MC vs TD (3)** <br>
TD exploits Markov property <br>
environment 가 Markov property 를 만족한다고 가정하고 $V(S _t)$ 와 $V(S _{t+1})$ 사이의 관계를 활용 <br>
-Usually more efficient in Markov environemnts <br>
MC does not exploit Markov property <br>
-Usually more effective in non-Markov environments <br>

**Bootstrapping and Sampling** <br>
BootstrappingL update involves an estimate <br>
-MC doees not bootstrap <br>
-DP/TD bootstraps <br>

Sampling: update samples an expectation <br>
-MC samples <br>
-DP does not sample <br>
-TD samples <br>

---

### $TD(\lambda)$ 

**$n$-step prediction** <br>
$n=\infty$ is same as MC <br>

**$n$-step return** <br>
$G _t^{(n)}=R_{t+1}+\dots+\gamma^{n-1} R_{t+n}+\gamma ^n V(S _{t+n})$ <br>
$n$-step temporal-difference learning <br>
$V(S _t) \leftarrow V(S _t) + \alpha (G _t^{(n)} - V(S _t))$ <br>

**Averaging $n$-step returns** <br>
We can average $n$-step returns over differnt $n$ <br>

Can we efficiently combine information from all time-steps? <br>

**$\lambda$-return** <br>
The $\lambda$-return $G _t^{\lambda}$ combines all $n$-step returns $G _t^{(n)}$ <br>
Usign weight $(1-\lambda)\lambda ^{n-1}$ <br>
$G _t^{\lambda} = (1-\lambda)\sum _{n=1}^\infty \lambda^{n-1} G _t^{(n)}$

**Forward-view $TD(\lambda)$** <br>
Like MC, can only be computed from complete episodes <br>

**Backward-view $TD(\lambda)$** <br>
Forward view provides theory <br>
Backward view provides mechanism <br>
Update online, every step, from incomplete sequences <br>

**Eligibility traces** <br>
![](/public/img/2022-08-19-RLCoursebyDavidSilver-Lecture4/1.png){: width="50%" height="50%"}{: .center}

spike? <br>


**Backward-view $TD(\lambda)$** <br>
Keep an eligibility trace for every state $s$ <br>
Update value $V(s)$ for every state $s$ <br>
In proportion to TD-error $\delta _t$ and eligibility trace $E _t(s)$ <br>
$\delta _t = R _{t+1} + \gamma V(S _{t+1}) - V(S _t)$ <br>
$V(s) \leftarrow V(s) + \alpha \delta _t E _t (s)$ <br>

**$TD(\lambda)$ and $TD(0)$** <br>
When $\lambda = 0$, only current state is updated <br>
$E _t(s) =1(S _t)$ <br>
$V(s) \leftarrow V(s) + \alpha \delta _t (s)$ <br>
This is exactly equivalent to $TD(0)$ update <br>
$V(S _t)\leftarrow V(S _t) + \alpha \delta _t$ <br>

When $\lambda=1$, credit is defferd until end of episode <br>
Consider episodic environments with offline updates <br>
Over the course of an episode, total update for $TD(1)$ is the same as total update for MC <br>

> The sum of offline updates is identical for forward-view and backward-view $TD(\lambda)$ <br>
> $\sum _{t=1}^T \alpha \delta_t E _t (s) = \sum _{t=1}^T \alpha (G _t ^\lambda - V(S _t)) 1 (S _t =s)$ <br>

**Proof** <br>
...