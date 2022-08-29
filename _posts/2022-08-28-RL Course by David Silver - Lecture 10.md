---
layout: post
title: RL Course by David Silver - Lecture 9
date: 2022-08-27 11:59:59 +0900
categories: RL
permalink: /22
---

# [RL Course by David Silver - Lecture 9](https://www.youtube.com/watch?v=sGuiWX07sKw&list=PLqYmG7hTraZDM-OYHWgPebj2MfCFzFObQ&index=9)


## Exploration and Exploitation

---

### Introduction

**Exploration vs. Exploitation Dilemma** <br>
Online decision-making involves a fundamental choice: <br>
-Exploitation Make the best decision given current information <br>
-Exploration Gather more information <br>
The best long-term strategy may involve short-term sacrifices <br>
Gather enough inforamtion to make the best overall decisions <br>

**Principles** <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/1.png){: width="50%" height="50%"}{: .center} <br>

---

### Multi-Armed Bandits

-A multi-armed bandit is a tuple $<A,R>$ (no state, transition) <br>
-$A$ is a known set of actions (or "arms") <br>
-$R^a(r)=P[R=r\|A=a] is an unknown probability distribution over rewards <br>
-At each step $t$ the agent selects and action $A _t \in A$ <br>
-The environment generates a reward $R _t \sim R^{A _t}$ <br>
-The goal is to maximize cumulative reward $\sum _{\tau =1}^t R _{\tau}$ <br>

**Regret** <br>
-The action-value is the mean reward for action $a$, <br>
$q(a)=E[R \| A=a]$ <br>
-The optimal value $v _{\*}$ is <br>
$v _{\*}=q(a^{\*}) = \underset{a\in A}{\max} q(a)$ <br>
-The total regret is the opportunity loss for one step <br>
$l _t = E[v _{\*}-q(A _t)]$ <br>
-The is the total opportunity loss
$L _t = E[\sum _{\tau}^t v _{\*}-q(A _{\tau})]$ <br>
-Maximize cumulative reward $\equiv$ minimize total regret <br>

**Counting regret** <br>
The count $N _t(a)$ is expected number of selections for action $a$ <br>
The gap $\Delta _a$ is the difference in value between action $a$ and optimal action $a^{\*}$, $\Delta _a = v _{\*} - q(a)$ <br>
Regret is a function of gaps and the counts <br>
$L _t = E[\sum _{\tau}^t v _{\*}-q(A _{\tau})]$ <br>
$=\sum _{a \in A} E[N _t(a)](v _{\*}-q(a))$ <br>
$=\sum _{a \in A} E[N _t(a)] \Delta _a$ <br>
A good algoritm ensures small counts for large gaps <br>
Problem: gaps are not known! <br>

**Linaer or sublinear regret** <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/2.png){: width="50%" height="50%"}{: .center} <br>

**Greedy algorithm** <br>
We consider algorithms that estimate $Q _t(a)\approx q(a)$ <br>
Estimate the value of each action by MC evaluation <br>
$ Q _t(a) = \frac{1}{N _t(a)}\sum _{t=1}^T 1 (A _t=a)R _t$ <br>
The greedy algorithm selects action wiht highest value <br>
$A _t = \underset{a\in A}{arg\max} Q _t(a)$ <br>
Greedy can lock onto a suboptimal action forever <br>
$\Rightarrow$ Greedy has linear total regret <br>

**Optimistic Initialziation** <br>
Initialzie values to maximum possible, $Q(a)=r _{\max}$ ($N(a)$ 또한 높은 값으로 init) <br>
Then act greedily <br>
$A _t = \underset{a\in A}{arg\max}Q _t(a)$ <br>
Encourages exploration of unknown values <br>
But a few unlucky samples can lock out optimal action forever <br>
$\Rightarrow$ Optimistic greedy has linear total regret <br>

**$\epsilon$-Greedy algorithm** <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/3.png){: width="50%" height="50%"}{: .center} <br>
($A \rightarrow \|A\|$) <br>

**Decaying $\epsilon _t$-Greedy algorithm** <br>
Pick a decay schedule for $\epsilon _1$, $\epsilon _2$, $\dots$ <br>
Consider the following schedule <br>
$c >0$ <br>
$d = \underset{a \| \Delta _a >0}{\min} \Delta _a$ <br>
$\epsilon _t = \min \{1, \frac{c\|A\|}{d^2 t}\}$ <br>
Decaying $\epsilon _t$-greedy has logarithmic asymptotic total regert! <br>
Unfortunately, schedule requires advance knowledge of gaps <br>
Goal: find an algorithm with sublinear regret for any multi-armed bandit (without knowledge of $R$) <br>

**Lower bound** <br>
The performance of any algorithms is determined by similarity between *optimal arm* and *other arms* <br>
Hard problems have similar-looking arms with differnet means <br>
This is described formally by the gap $\Delta _a$ and the similarity in distribution $KL(R^a \| \| R^{a^{\*}})$ <br>
**Theorem (lai and Robbins)** <br>
Asymptotic total regret is at least logarithmic in number of steps <br>
$\underset{t \to \infty}{\lim}L _t \geq \log t \sum _{a \| \Delta _a >0} \frac{\Delta _a}{KLK(R ^t \| \| R^{a^{\*}})}$ <br>

**Optimism in the face of uncertainty** <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/4.png){: width="50%" height="50%"}{: .center} <br>

![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/5.png){: width="50%" height="50%"}{: .center} <br>

**Upper confidence bounds** <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/6.png){: width="50%" height="50%"}{: .center} <br>

**Hoeffding's inequality** <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/7.png){: width="50%" height="50%"}{: .center} <br>

**Calculating upper confidence bounds** <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/8.png){: width="50%" height="50%"}{: .center} <br>

**UCB1** <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/9.png){: width="50%" height="50%"}{: .center} <br>

**Upper condfidence bounds** <br>
Upper confidence bounds can be applied to other inequalityes <br>
-Bernstein's inequality <br>
-Empirical Bernstein's inequality <br>
-Chernoff inequality <br>
-Azuma's inequality <br>

**Bayesian bandits** <br>
Bayesian bandits exploit prior knowledge about rewards, $p[R^a]$ <br>
Consider a distribution $p[Q \| w]$ over action-value function with parameter $w$ <br>
-e.g. indep. Gaussians: $w=[\mu _1, \sigma _1^2,\dots , \mu _k, \sigma _k^2]$ for $a\in [1,k]$ <br>
Bayesian methods compute posterior distribution over $w$ <br>
$p[w \| R _1, \dots, R _t]$ <br>
Use posterior to guide exploration <br>
-Upper confidence bounds <br>
-Probability matching <br>
Better performance if prior knowledge is accurate <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/10.png){: width="50%" height="50%"}{: .center} <br>

**Bayesian UCB Example: Independent Gaussains** <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/11.png){: width="50%" height="50%"}{: .center} <br>

**Probability matching** <br>
Probability matching selects action $a$ according to probability that $a$ is the optimal action <br>
$\pi(a)=P[Q(a)=\underset{a'}{\max}Q(a')\|R _1, \dots, R _{t-1}]$ <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/12.png){: width="50%" height="50%"}{: .center} <br>

**Thompson sampling** <br>
Thompson sampling is sample-based probability matching <br>
$\pi(a) = E[1(Q(a)=\underset{a'}{\max}Q(a')) \| R _1, \dots, R _{t-1}]$ <br>
Use bayes law to compute posterior distribution $p _w(Q \| R _1, \dots, R _{t-1})$ <br>
Sample an action-value function $Q(a)$ from posterior <br>
Select action maximizing sample, $A _t=\underset{a\in A}{arg\max} Q(a)$ <br>
For Bernoulli bandits, Thompson sampling achieves Lai and Robbins lower bound on regert! <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/13.png){: width="50%" height="50%"}{: .center} <br>


**Value of information** <br>
Exploration is useful because it gains information <br>
Can we quantify the value of information ? <br>
-How much reward a decision-maker would be prepared to pay in order to have that information, prior to making a decision <br>
-Long-term reward after getting information -immediate reward <br>
Information gain is higher in uncertain situations <br>
Therfore it makes sense to explore uncertain situations more <br>
If we know value of information, we can trade-off exploration and exploitation optimally <br>

**Information state space** <br>
We have viewed bandits as *one-step* decision-making problems <br>
Can also view as *sequential* decision-making problems <br>
At each step there is an information state $\tilde{S}$ summarizing all information accumulated so far <br>
Each action $A$ causes a transition to a new information state $\tilde{S}'$ (by adding information), with probability $\tilde{P}_{\tilde{S}, \tilde{S}'}^A$ <br>
So now we have an MDP $\tilde{M}$ in the arugmented information state space <br>
$\tilde{M}=<\tilde{S}, A,\tilde{P},R,\gamma>$ <br>

**Example: Bernoulli bandit** <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/14.png){: width="50%" height="50%"}{: .center} <br>

**Solving information state space bandits** <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/15.png){: width="50%" height="50%"}{: .center} <br>

**Bayes-adpative Bernoulli Bandits** <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/16.png){: width="50%" height="50%"}{: .center} <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/17.png){: width="50%" height="50%"}{: .center} <br>

**Gittins indices for Bernoulli bandits** <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/18.png){: width="50%" height="50%"}{: .center} <br>

**Summary of Multi-armed bandit algorithms** <br>
Random exploration <br>
-$\epsilon$-greedy <br>
-Softmax <br>
-Gaussian noise <br>
Optimism in the face of uncertainty <br>
-Optimistic initialization <br>
-UCB <br>
-Thompson sampling <br>
Information state space <br>
-Gittins indices <br>
-Bayes-adaptive MDPs <br>

---

### Contextual Bandits

![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/19.png){: width="50%" height="50%"}{: .center} <br>

**Linear regression** <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/20.png){: width="50%" height="50%"}{: .center} <br>

**Linear UCB** <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/21.png){: width="50%" height="50%"}{: .center} <br>

**Geometric interpretation** <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/22.png){: width="50%" height="50%"}{: .center} <br>

**Calculating linear upper confidence bounds** <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/23.png){: width="50%" height="50%"}{: .center} <br>

---

### MDPs **1:31:58** <br>

**Exploration/Exploitation principles to MDPs** <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/24.png){: width="50%" height="50%"}{: .center} <br>

**UCB for MDPs: Model-free RL** <br>
The UCB approach can be generalized to full MDPs <br>
To give a model-free RL algorithm <br>
Maximize upper confidence bound on action-value function <br>
$A _t = \underset{a\in A}{arg\max} Q(S _t, a) + U(S _t, a)$ <br>
For example <br>
-Kaelbling's interval estimation (table lookup) <br>
-LSTD with confidence ellipsoid (linear function approximation) <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/27.png){: width="50%" height="50%"}{: .center} <br>

**Optimistic initialization: Model-free RL** <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/25.png){: width="50%" height="50%"}{: .center} <br>

**Optimistic initialization: Model-based RL** <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/26.png){: width="50%" height="50%"}{: .center} <br>

---

**Bayesian model-based RL** <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/28.png){: width="50%" height="50%"}{: .center} <br>

**Thompson sampling: Model-based RL** <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/29.png){: width="50%" height="50%"}{: .center} <br>

**Information state search in MDPs** <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/30.png){: width="50%" height="50%"}{: .center} <br>

**Bayes adaptive MDPs** <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/31.png){: width="50%" height="50%"}{: .center} <br>

**Conclusion** <br>
![](/public/img/2022-08-27-RLCoursebyDavidSilver-Lecture9/32.png){: width="50%" height="50%"}{: .center} <br>

---

### Todo
- [ ] Upper confidence bounds
- [ ] Probability matching
- [ ] Information state search


