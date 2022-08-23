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
$G _t^{(n)}=R _{t+1}+\dots+\gamma^{n-1} R _{t+n}+\gamma ^n V(S _{t+n})$ <br>
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


---

## 원본 글: [Gitbook](https://dnddnjs.gitbooks.io/rl/content/monte-carlo_methods.html)

## MC learning

### MC prediction <br>

Incremental mean 에서 분모로 가있는 $N(S _t)$ 가 점점 무한대로 가게 되는데, 이를 $\alpha$ 로 고정시켜 놓으면 효과적으로 평균을 취할 수 있게 된다. <br>
맨 처음 정보들에 대해서 가중치를 덜 주는 형태 (Complementary filter 에 대해 알면 이해가 쉬움) 라고 생각하면 된다. <br>
이와 같은 이유는 강화학습이 stationary problem 이 아니기 때문이다. 매 episode 마다 새로운 policy 를 사용하기 때문에 non-stationary problem 이므로 update 하는 상수를 일정하게 고정하는 것이다. <br>
$V(S _t) \leftarrow V(S _t) + \frac{1}{N(S _t)}(G _t-V(S _t))$ <br>
$V(S _t) \leftarrow V(S _t) + \alpha(G _t - V(S _t))$ <br>


### MC Control <br>

**MC policy iteraion** <br>
Policy evaluation 은 MC policy evaluation 을 이용해 진행할 수 있지만, policy improvement 에서 세 가지 문제가 발생하게 된다. <br>


**MC Contorl** <br>
(1) Value function <br>
MC 를 사용한 이유는 model-free 를 하기 위함인데 value function 으로 policy 를 improve 하려면 MDP 의 모델을 알아야 한다. <br>
아래와 같이 다음 policy 를 계산하려면 reward 와 transition probability 를 알아야 할 수 있다. <br>
따라서 value function 대신에 action value function 을 사용해야 한다. <br>
Greedy policy improvement over $V(s)$ requires model of MDP <br>
$\pi'(s) = \underset{a\in A}{arg\max} R _s^a + P _{ss'}^a V(s')$ <br>
Greedy policy improvement over $Q(s,a)$ is model-free <br>
$\pi'(s)= \underset{a\in A}{arg\max} Q(s,a)$ <br>

(2) Exploration <br>
현재 policy improve 는 greedy policy improvement 를 사용하고 있는데, 이는 optimal 로 가는것이 아닌 local optimum 에 빠져버릴 수 있는 문제가 있다. <br>
따라서 그에 대한 대안으로 epsilon greedy policy improvement 를 사용한다. <br>

(3) Policy iteration <br>
Policy iteration 에서는 evaluation 과정이 true value function 으로 수렴할 때까지 해야 하는데, 그렇게 하지 않고 한 번 evaluation 한 다음에 policy improve 를 해도 optimal 로 간다고 말했었다. <br>
그것이 value iteration 이였는데 MC 에서도 마찬가지로 이 evaluation 과정을 줄임으로서 MC policy iteration 에서 MC control 이 된다. <br>

![](/public/img/2022-08-19-RLCoursebyDavidSilver-Lecture4/2.png){: width="50%" height="50%"}{: .center}

**GLIE** <br>

... <br>

---

## 원본 글: [Gitbook](https://dnddnjs.gitbooks.io/rl/content/temporal_difference_methods.html)


## Eligibility Traces

**Backward-veiw of TD($\lambda$)** <br>
Forward-veiw 의 경우 원래 TD 의 장점이었던 time-step 마다 update 할 수 있다는 장점이 사라진다. <br>
MC 의 장점을 살리면서 바로 바로 update 할 수 있는 방법을 찾기 위해 elgitibility trace 라는 개념이 나오게 된다. <br>

과거에 있었던 일 중에서 현재 내가 받은 reward 에 기여한 것이 무엇일까 라는 credit assignment 문제에서 "얼마나 최근에 일어났던 일이었나"와 "얼마나 자주 발생했었나" 라는 것을 기준으로 과거의 일들을 기억해놓고 현재 받은 reward 를 과거의 state 들로 분배해주게 된다. <br>

즉 TD(0) 처럼 현재의 value function 만 update 하는 것이 아니라, 과거에 지나왔던 모든 state 에 eligibility trace 를 기억해두었다가 그만큼 자신을 update 하게 된다. <br>
따라서 현재의 경험을 통해 한 번에 과거의 모든 state 들의 value funciton 을 update 하게 되는 것이다. <br>
현재 경험이 과거에 value function 에 얼마나 영향을 주고 싶은가는 $\lambdas$ 를 통해 조절할 수 있다. <br>

Sarsa 또한 $n$-step Sarsa, forward-veiw Sarsa($\lambda$), backward-vew Sarsa($\lambda$) 가 존재한다.<br>
Sarsa 는 model-free 이므로 action value  function 을 사용한다는 것을 제외하고는 위의 TD 와 동일한 방식으로 진행된다. <br>