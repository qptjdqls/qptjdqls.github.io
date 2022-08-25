---
layout: post
title: RL Course by David Silver - Lecture 6
date: 2022-08-24 11:59:59 +0900
categories: RL
permalink: /17
---

# [RL Course by David Silver - Lecture 6](https://www.youtube.com/watch?v=UoPei5o4fps&list=PLqYmG7hTraZDM-OYHWgPebj2MfCFzFObQ&index=6&t=4s)


## Value function approximation

---

### Introduction

How can we scale up the model-free methods for prediction and control from the last two lectures?

Solution for large MDPs: <br>
-Estimate value funciton with funciton approximation <br>
$\hat{v}(s,w) \approx v _{\pi}(s)$ <br>
or $\hat{q}(s,a,w) \approx q _{\pi}(s,a)$ <br>

Generalize from seen states to unseen states <br>
Update parameter $w$ using MC or TD learning

**Types of value function approximation** <br>
![](/public/img/2022-08-24-RLCoursebyDavidSilver-Lecture6/1.png){: width="50%" height="50%"}{: .center} <br>

**Which function approximator?** <br>
We consider differentiable function approximators, <br>
-Linear combinations of feature <br>
-Neural network <br>
-~~Decision tree~~ <br>
-~~Nearest neighbour~~ <br>
-~~Fourier /wavelet bases~~ <br>
Furthermore, we require a traning method that is suitable for non-stationary, non-iid data <br>

---

### Incremental methods

**Value function approx. by SGD** <br>
$J(w) = E _{\pi}[(v _{\pi}(S) - \hat{v}(S, a))^2]$ <br>
Gradient descent finds a local minimum <br>
$\Delta w = \frac{1}{2} \alpha \nabla _w J(w)$ <br>
$=\alpha E _{\pi}[(v _\pi (S) - \hat{v}(S,w))\nabla _w \hat{v}(S,w)]$ <br>
SGD samples the gradient <br>
$\Delta w = \alpha(v _{\pi}(S) - \hat{v}(S,w))\nabla _w \hat{v}(S,w)$ <br>
하지만 이런 방식은 $v _{\pi}(S)$ 를 알려주는 오라클이 필요하다. <br>

**Feature vectors** <br>
Represent state by a feature vector <br>
$x(S)=(x _1(S), \dots, x _n(S))^T$ <br>

**Linear value function approximation** <br>
represent value funciton by a linear combination of features <br>
$\hat{v}(S,w)=x(S)^T w = \sum _{j=1}^n x _j(S) w _j$ <br>
Objective function is quadratic in parameters $w$ <br>
$J(w)=E _{\pi}[(v _{\pi}(S)-x(S)^T w)^2]$ <br>
SGD converges on global optimum <br>
$\nabla _w \hat{v}(S,w)=x(S)$ <br>
$\Delta w = \alpha(v _{\pi}(S) - \hat{v}(S,w))x(S)$ <br>

**Table lookup feature** <br>
Table lookup is a special case of linear value funciton approximation <br>
$x^{table}(S)=(1(S=s_1),\dots, 1(S=s_n))^T$ <br>
Parameter vector $w$ gives value of each individual state <br>
$\hat{v}(S,w)=(1(S=s_1),\dots, 1(S=s_n))^T \cdot (w_1, \dots, w _n)^T$ <br>

**Incremental prediction algorithms** <br>
Have assumed true value function $v _{\pi}(s)$ given by supervisor <br>
But in RL there is no supervisor, only rewards <br>
In practice, we substitue a *target* fo $v _{\pi}(s)$ <br>
-For MC, the target is the return $G _t$ <br>
$\Delta w = \alpha(G _t - \hat{v}(S _t, w))\nabla _w \hat{v}(S _t, w)$ <br>

-For TD(0), the target is the TD target $R _{t+1}+\gamma \hat{v}(S _{t+1},w)$ <br>
$\Delta w = \alpha(R _{t+1}+\gamma \hat{v}(S _{t+1},w) - \hat{v}(S _t, w))\nabla _w \hat{v}(S _t, w)$ <br>

**질문**: 왜 gradient 를 $\hat{v}(S _t, w)$ 에만 적용하고 $\gamma \hat{v}(S _{t+1},w)$ 에 대해서는 적용해주지 않는가?

**답**: 두 항 모두 gradient 를 적용할 경우, 기준이 되는 항이 없어 전자가 후자에게 가까워 지려는 동시에 후자가 전자에게 가까워 지려는 time reversal 과 같은 상황이 발생하는데, 이럴 때는 정답 수렴하지 못하게 된다. <br>
TD 는 next step 에서 발생하는 value function 값을 믿기 때문에 후항을 고정시키고 학습을 진행해야 한다. <br>
후항이 더 믿을만한 이유는 시간이 지날수록 실제 reward 더 관찰할 수 있게 되고 그만큼 value function 의 정확도도 늘어날 것이기 때문이다 <br>
실제로 두 항 의 gradient 를 모두 고려하는 방법이 있는데 이를 **redsidual gradient method** 라고 한다. <br>

**Action-value function approximation** <br>
$\Delta w = \alpha(q _{\pi}(S,A)-\hat{q}(S,A,w))\nabla _w \hat{q}(S,A,w)$ <br>
Represent state and action by a feature vector <br>
$x(S,A)=(x _1(S,A),\dots,x _n(S,A))^T$ <br>

**Incremental control algorithms** <br>
-For MC, the target is the return $G _t$ <br>
$\Delta w = \alpha(G _t-\hat{q}(S _t, A _t, w))\nabla _w \hat{q}(S _t, A _t, w)$ <br>


**Study of $\lambda$: Should we bootstrap** <br>
![](/public/img/2022-08-24-RLCoursebyDavidSilver-Lecture6/2.png){: width="50%" height="50%"}{: .center} <br>

**Bairds's counterexample** <br>

![](/public/img/2022-08-24-RLCoursebyDavidSilver-Lecture6/3.png){: width="50%" height="50%"}{: .center} <br>

![](/public/img/2022-08-24-RLCoursebyDavidSilver-Lecture6/4.png){: width="50%" height="50%"}{: .center} <br>

TD isn't guarentee to be a stable algorithm. <br>
There are situation where you apply TD, and you can blow up. <br>

**Convergence of prediction algorithms** <br>

![](/public/img/2022-08-24-RLCoursebyDavidSilver-Lecture6/5.png){: width="50%" height="50%"}{: .center} <br>

**Gradient TD learning** <br>

TD does not follow the gradient of any objective function <br>
This is why TD can diverge when off-policy or using non-linear function approximation <br>

**Gradient TD** follows **true gradient** of projected Bellman error <br>

**Convergence of control algorithms** <br>

![](/public/img/2022-08-24-RLCoursebyDavidSilver-Lecture6/6.png){: width="50%" height="50%"}{: .center} <br>

---

### Batch methods

**Batch RL** <br>
Gradient descent is simple and appealing <br>
But it is not sample efficient <br>
Batch methods seek to find the best fitting value function Given the agent's experience ("training data") <br>

**Least squares predcition** <br>
Least squares algorithm find parameter vector $w$ minimizing sum-squared error between $\hat{v}(s _t, w)$ and target values $v _t^{\pi}$, <br>
$LS(w)=\sum _{t=1}^T (v _t^{\pi}-\hat{v}(s _t, w))^2$ <br>
$=E _D [(v ^{\pi}-\hat{v}(s,w))^2]$ <br>

**SGD with experience replay** <br>
Given experience consisiting of $<state, value>$ pairs <br>
$D={<s _1, v _1^{\pi}>, \dots, <s _t, v _T^{\pi}>}$ <br>
Repeat: <br>
-Sample state, value from experience, <br>
$<s,v^{\pi}>\sim D$ <br>
-Apply SGD update <br>
$\Delta w = \alpha(v ^{\pi}-\hat{v}(s,w))\nabla _w \hat{v}(s,w)$ <br>
Converges to least squares solution <br>
$w ^{\pi}=\underset{w}{arg\min}LS(w)$ <br>

**Experience replay in Deep Q-networks (DQN)** <br>
DQN uses **experience replay** and **fixed Q-targets** <br>

앞서 봤듯이 Sarsa 나 TD 는 blow up 할 수 있는 문제가 존재하는데 DQN 은 그것을 해결하고 stable 하게 학습했다는 것이 contribution 이다. <br>
학습을 stable 하게 만든 두 trick 은 experience replay 와 fixed Q-targets 이다. <br>
Experience replay 는 trajectory 를 decorrelate 하기 때문에 (기존에는 highly correlated 된 연속된 state action path 를 보는 반면 experience replay 는 order 를 randomize 하여 decorrelate 시킴) 더욱 stable 한 update 를 진행할 수 있다. <br>
fixed Q-targets 는 policy network 와 별도의 bootstrapping 에 사용될 target network 를 만들고 해당 paramter 를 일정 iteration 동안 forzen 시켜서 일종의 Supervised learning 과 동일한 상황으로 학습시킨다. <br>
일정 iteration 마다 target network 는 policy network 와 동일하게 업데이트 되는 과정을 반복한다. <br> 
상세한 과정은 추후 정리 <br>

-Take action $a _t$ according to $\epsilon$-greedy policy <br>
-Store transition $(s _t, a _t, r _{t+1}, s _{t+1})$ in replay memory $D$ <br>
-Sample random mini-batch of transitions $(s,a,r,s')$ from $D$ <br>
-Compute Q-learning targets w.r.t. old, fixed parameters $w^-$ <br>
-Optimize MSE between Q-network and Q-learning targets <br>
$L _i(w _i) = E _{s,a,r,s'\sim D _i}[(r + \gamma \underset{a'}{\max} Q(s',a';w _i ^-)-Q(s,a,;w _i))^2]$ <br>
-Using variant of SGD <br>

**DQN in Atari** <br>

![](/public/img/2022-08-24-RLCoursebyDavidSilver-Lecture6/7.png){: width="50%" height="50%"}{: .center} <br>

![](/public/img/2022-08-24-RLCoursebyDavidSilver-Lecture6/8.png){: width="50%" height="50%"}{: .center} <br>

**Montezuma's Revenge!**

**Linear least squares prediction** <br>
Experience replay finds least squares solution <br>
But it may take many iterations <br>
Using linear value function approximation $\hat{v}(s,w)=x(s)^T w$ <br>
We can solve the least squares solution **directly** <br>

At minimum of $LS(w)$, the expected update must be zero <br>
$E _D[\Delta w] = 0$ <br>
$\alpha \sum _{t=1}^T x (s _t)(v _t^{\pi}-x(s _t)^T w) = 0$ <br>
$\sum _{t=1}^T x (s _t)v _t^{\pi} = \sum _{t=1}^T x(s _t)x(s _t)^T w$ <br>
$w = (\sum _{t=1}^T x (s _t)x (s _t)^T)^{-1} \sum _{t=1}^T x (s _t)v _t^{\pi}$ <br>

For $N$ features, direct solution time is $O(N^3)$ <br>
Incremental solution time is $O(N^2)$ using Shermann-Morrison <br>

$v _t^{\pi}$ 는 오라클이 주는 value function 값으로 우리가 알 수 없기 때문에, 앞서 봤던 incremental prediction algorithm 에서 사용했던 MC, TD TD($\lambda$) 방식으로 사용해서 estimate 하면 된다. <br>

![](/public/img/2022-08-24-RLCoursebyDavidSilver-Lecture6/9.png){: width="50%" height="50%"}{: .center} <br>

![](/public/img/2022-08-24-RLCoursebyDavidSilver-Lecture6/10.png){: width="50%" height="50%"}{: .center} <br>

**Convergence of Linear least squares prediction algorithms** <br>

![](/public/img/2022-08-24-RLCoursebyDavidSilver-Lecture6/11.png){: width="50%" height="50%"}{: .center} <br>


**Least squares policy iteration** <br>
1:33:00 <br>
tbd <br>

---

### **Todo**
- [ ] Gradient TD and the true gradient of projected Bellman error
- [ ] Fixed Q-targets


---

## 원본 글: [Gitbook](https://dnddnjs.gitbooks.io/rl/content/value_function_approximator.html)

**Gradient descent on RL** <br>

Gradient Descent방법도 (1) Stochastic Gradient Descent(SGD)와 (2) Batch방법으로 나눌 수 있는데 위와 같이 (모든 state에서 true value function과의 error을 한 번에 함수로 잡아서 업데이트하는 방식은 Batch의 방식을 활용한 것으로서 step by step으로 업데이트하는 것이 아니고 한꺼번에 업데이트하는 것입니다.) <br>

이전 trajectory(?), 이전 experience step 까지 batch 로 활용하여 sample efficient 하게 학습하겠다는 것이 batch RL. <br>

위의 **Least squares prediction** 파트에서도 확인할 수 있듯이, experience 의 $<state, value>$ pair 는 같은 policy 에서 나왔다. <br>

**Q: 그런데 DQN 에서 사용하는 experience replay 의 경우 이전 trajectory 와 현재 trajectory 는 다른 policy 를 기반으로 생성되었는데, 같은 batch 에서 sampling 해서 mean square error 방향으로 학습하는 것이 맞나?** <br>


### Learning with funciton approximator

**Example: [Mountain Car](https://see.stanford.edu/materials/aimlcs229/problemset4.pdf)** <br>

문제의 정의는 다음과 같다. <br>
정상을 제외한 모든 곳은 time step 마다 reward -1 씩 받게 된다. <br>
따라서 최대한 빠른 시간 내에 goal 에 올라가는 것을 agent 는 목표로 하게 된다. <br>
또한 바로 uphill 을 할 출력은 차에게 없다고 가정을 한다면 차가 왔다 갔다하면서 중력으로 가속 시켜서 올라가야 하기 때문에 문제가 어려워진다. <br>