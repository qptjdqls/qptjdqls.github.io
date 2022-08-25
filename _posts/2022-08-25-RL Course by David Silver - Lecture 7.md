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

**Why does stochastic policy is necessary?** <br>
![](/public/img/2022-08-25-RLCoursebyDavidSilver-Lecture7/2.png){: width="50%" height="50%"}{: .center} <br>

POMDP <br>
![](/public/img/2022-08-25-RLCoursebyDavidSilver-Lecture7/3.png){: width="50%" height="50%"}{: .center} <br>

![](/public/img/2022-08-25-RLCoursebyDavidSilver-Lecture7/4.png){: width="50%" height="50%"}{: .center} <br>

![](/public/img/2022-08-25-RLCoursebyDavidSilver-Lecture7/5.png){: width="50%" height="50%"}{: .center} <br>

**Policy objective functions** <br>
Goal: given policy $\pi _{\theta}(s,a)$ with parameters $\theta$, find best $\theta$ <br>
But how do we measure the quality of a policy $\pi _{\theta}$? <br>
In episodic environments we can use the start value <br>
$J _1 (\theta) = V ^{\pi _{\theta}} (s _1) = E _{\pi _{\theta}} [v _1]$ <br>
In continuing environments we can use the average value <br>
$J _{avV}(\theta) = \sum _s d^{\pi _{\theta}} (s) V ^{\pi _{\theta}} (s)$ <br>
Or the average reward per time-step <br>
$J _{avR}(\theta) = \sum _{s} d^{\pi _{\theta}} (s) \sum _{a} \pi _{\theta}(s,a) R _s ^a$ <br>
where $d^{\pi _{\theta}} (s)$ is stationary distribution of Markov chain for $\pi _{\theta}$ <br>

**Policy optimization** <br>
Some approaches do not use gradient <br>
-Hill climbing <br>
-Simplex / amoeba / Nelder Mead <br>
-Genetic algorithms <br>
Greater efficiency often possible using gradient <br>
-GD <br>
-Conjugate gradient <br>
-Quasi-newton <br>
We focus on gradient descent, many extensions possible and on methods that exploit sequential structure <br>

---

### Finite difference policy gradient

**Computing gradients by finite differences** <br>
![](/public/img/2022-08-25-RLCoursebyDavidSilver-Lecture7/6.png){: width="50%" height="50%"}{: .center} <br>
This method will collapse in high dimension <br>

---

### MC policy gradient 

**Score function** <br>
We now compute the policy gradient analytically <br>
Assum policy $\pi _{\theta}$ is differentiable whenever it is non-zero <br>
and we know the gradient $\nabla _{\theta} \pi _{\theta}(s,a)$ <br>
Likelihood ratios exploit the following identity <br>

$\nabla _{\theta} \pi _{\theta} (s, a) = \pi _{\theta}(s, a) \frac{\nabla _{\theta} \pi _{\theta} (s,a)}{\pi _{\theta}(s,a)}$ <br>
$=\pi _{\theta}(s,a) \nabla _{\theta} \log \pi _{\theta} (s,a)$ <br>

This is called **likelihood ratio trick** <br>
이러한 형태는 expectation 을 계산 하기 쉽게 만들어준다 ($\pi _{\theta}(s,a)$ 가 probability distribution 이니까). <br>

The score function is $\nabla _{\theta} \log \pi _{\theta}(s,a)$ <br>

**Sofmax policy** <br>
-We will use a softmax policy as a running example <br>
-Weight actions using linear combination of features $\phi(s,a)^T \theta$ <br>
-Probability of action is proportional to exponentiated weight <br>
$\pi _{\theta}(s,a) \propto e^{\phi(s,a)^T \theta}$ <br>
-The score function is <br>
$\nabla _{\theta} \log \pi _{\theta}(s,a) = \phi(s,a) - E _{\pi _{\theta}} [\phi(s, \cdot)]$ <br>

**Gaussian policy** <br>
-In continous action sapces, a Gaussian policy is natural <br>
-Mean is a lienar combination of state features $\mu(s) = \phi(s)^T \theta$ <br>
-Variance may be fixed $\sigma ^2$, or can be also parameterized <br>
-Policy is Gaussian, $a \sim N(\mu(s), \sigma^2)$ <br>
-The score function is <br>
$\nabla _{\theta} \log \pi _{\theta} (s,a) = \frac{(a-\mu(s))\phi(s)}{\sigma ^2}$ <br>

**One-step MDPs** <br>
Consider a simple class of one-step MDPs <br>
-Starting in state $s\sim d(s)$ <br>
-Terminating after one time-step with reward $r=R _{s,a}$ <br>
Use likelihood ratios to compute the policy gradient <br>
$J(\theta) = E _{\pi _{\theta}}[r]$ <br>
$=\sum {s\in S} d(s) \sum _{a\in A} \pi _{\theta}(s,a) R _{s,a}$ <br>
$\nabla _{\theta} J(\theta) = \sum _{s\in S} d(s) \sum _{a \in A} \pi  _{\theta} (s,a) \nabla _{\theta} \log \pi _{\theta} (s,a) R _{s,a}$ <br>
$= E _{\pi _{\theta}}[\nabla _{\theta} \log \pi _{\theta} (s,a) r]$ <br>

**Policy gradient theorem** <br>
-The policy gradient theorem generalize the likelihood ratio approach to multi-step MDPs <br>
-**Replaces instantaneous reward $r$ with long-term value $Q ^{\pi}(s,a)$** <br>
-Policy gradient theorem applies to start state objective, average reward and average value objective <br>

Theorem <br>
For any differentiable policy $\pi _{\theta}(s,a)$, <br>
for any of the policy objective functions $J=J _1, J _{avR}, or \frac{1}{1-\gamma} J _{avV}$ , the policy gradient is <br>
$\nabla _{\theta} J(\theta) = E _{\pi _{\theta}} [\nabla _{\theta} \log \pi _{\theta}(s,a) Q ^{\pi _{\theta}} (s,a)]$ <br>

**MC policy gradient(REINFORCE)** <br>
![](/public/img/2022-08-25-RLCoursebyDavidSilver-Lecture7/7.png){: width="50%" height="50%"}{: .center} <br>

---

### Actor-critic policy gradient

**Reducing variance using a critic** <br>
-MC policy gradient still has high variance <br>
-We use a critic to estimate the action-value function, <br>
$Q _w (s,a) \approx Q^{\pi _{\theta}}(s,a)$ <br>
-Actor-critic algorithms maintain two sets of parameters <br>
Critic updates action-value funciton parameters $w$ <br>
Actor updates policy parameters $\theta$, in direction suggested by critic <br>
-Actor-critic algorithms follow an approximate policy gradient <br>
$\nabla _{\theta}J(\theta) \approx E _{\pi _{\theta}}[\nabla _{\theta} \log \pi _{\theta}(s,a) Q _w (s,a)]$ <br>
$\Delta \theta = \alpha \nabla _{\theta} \log \pi _{\theta} (s,a) Q _w (s,a)$ <br>

**Estimating the action-value function** <br>
The critic is solving a familiar problem: policy evaluation <br>
How good is policy $\pi _{\theta}$ for current parameters $\theta$ ? <br>
This problem was explored in previous two lectures, e.g. <br>
-MC policy evaluation <br>
-TD learning <br>
-TD($\lambda$) <br>
Could aso use e.g. least-squares policy evaluation <br>

**Action-value actor-critic** <br>
![](/public/img/2022-08-25-RLCoursebyDavidSilver-Lecture7/8.png){: width="50%" height="50%"}{: .center} <br>

**Bias in Actor-critic algorithms** <br>
![](/public/img/2022-08-25-RLCoursebyDavidSilver-Lecture7/9.png){: width="50%" height="50%"}{: .center} <br>

**Compatible function approximation** <br>
![](/public/img/2022-08-25-RLCoursebyDavidSilver-Lecture7/10.png){: width="50%" height="50%"}{: .center} <br>

**Proof of compatible function approximation theorem** <br>
![](/public/img/2022-08-25-RLCoursebyDavidSilver-Lecture7/11.png){: width="50%" height="50%"}{: .center} <br>

**Reducing variance using a baseline** <br>
We subtract a baseline function $B(s)$ from the policy gradeint <br>
This can reduce variance, without changing expectation <br>
$E _{\pi _{\theta}}[\nabla _{\theta} \log \pi _{\theta}(s,a)B(s)]=\sum _{s\in S} d^{\pi _{\theta}} d ^{\pi _{\theta}} (s) \sum _a \nabla _{\theta} \pi _{\theta} (s,a) B(s)$ <br>
$=\sum _{s\in S} d^{\pi _{\theta}} B(s) \nabla _{\theta} \sum _{a\in A} \pi _{\theta} (s,a)$ <br>
$=0$ <br>

A good baseline is the state value function $B(s) = V^{\pi _{\theta}}(s)$ <br>
So we can rewrite the policy gradient using the advantage function $A ^{\pi _{\theta}}(s,a)$ <br>
$A ^{\pi _\theta}(s,a) = Q ^{\pi _{\theta}}(s,a) - V ^{\pi _{\theta}}(s)$ <br>
$\nabla _{\theta} J(\theta) = E _{\pi _{\theta}}[\nabla _{\theta} \log \pi _{\theta} (s,a) A ^{\pi _{\theta}}(s,a)]$ <br>

**Estimating the advantage function (1)** <br>
![](/public/img/2022-08-25-RLCoursebyDavidSilver-Lecture7/12.png){: width="50%" height="50%"}{: .center} <br>

**Estimating the advantage function (2)** <br>
![](/public/img/2022-08-25-RLCoursebyDavidSilver-Lecture7/13.png){: width="50%" height="50%"}{: .center} <br>

**Critics at different time-scales** <br>
![](/public/img/2022-08-25-RLCoursebyDavidSilver-Lecture7/14.png){: width="50%" height="50%"}{: .center} <br>

**Actors at different time-scales** <br>
![](/public/img/2022-08-25-RLCoursebyDavidSilver-Lecture7/15.png){: width="50%" height="50%"}{: .center} <br>

---
---

**Policy gradient with eligibility traces(1:21:40)** <br>
![](/public/img/2022-08-25-RLCoursebyDavidSilver-Lecture7/16.png){: width="50%" height="50%"}{: .center} <br>

**Alternative policy gradient direction(1:23:49)** <br>
![](/public/img/2022-08-25-RLCoursebyDavidSilver-Lecture7/17.png){: width="50%" height="50%"}{: .center} <br>

**DDPG(?)**

**Natural policy gradient** <br>
![](/public/img/2022-08-25-RLCoursebyDavidSilver-Lecture7/18.png){: width="50%" height="50%"}{: .center} <br>

**Natural Actor-critic** <br>
![](/public/img/2022-08-25-RLCoursebyDavidSilver-Lecture7/19.png){: width="50%" height="50%"}{: .center} <br>

**Summary of policy gradient algorithms** <br>
different variance <br>
![](/public/img/2022-08-25-RLCoursebyDavidSilver-Lecture7/20.png){: width="50%" height="50%"}{: .center} <br>

---

### Todo
- [ ] Policy gradient with eligibility traces
- [ ] Alternative policy gradietn direction