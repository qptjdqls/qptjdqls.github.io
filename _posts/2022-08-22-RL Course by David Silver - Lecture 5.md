---
layout: post
title: RL Course by David Silver - Lecture 5
date: 2022-08-22 11:59:59 +0900
categories: RL
permalink: /16
---

# [RL Course by David Silver - Lecture 5](https://www.youtube.com/watch?v=0g4j2k_Ggc4&list=PLqYmG7hTraZDM-OYHWgPebj2MfCFzFObQ&index=5)


## Model-Free Control

---

### Introduction

On-policy learning <br>
-"Learn on the job" <br>
-Learn about policy $\pi$ from experience sampled from $\pi$ <br>
Off-policy learning <br>
-"Look over someone's shoulder" <br>
-Learn about policy $\pi$ from experience sampled from $\mu$ <br>

---

### On-policy MC control

![](/public/img/2022-08-22-RLCoursebyDavidSilver-Lecture5/1.png){: width="50%" height="50%"}{: .center} <br>

위 그림은 policy evaluation 과 policy improvement 를 반복하여 optimal policy 를 찾아가는 generalized policy iteration 방식을 나타낸다. <br>
그렇다면 MC의 경우 **policy evaluation** 과 **policy improvement** 를 각각 어떤 방식으로 진행해야 할까?

우선 Policy evaluation 을 통해 state value function 을 계산한다면, model-free 라는 성격을 위배한다. <br>
$\pi '(s) = \underset{a\in A}{arg\max} R _s^a + P _{ss'} ^a V(s')$ <br>
위 식에서 볼 수 있듯이 greedy policy improvement 를 위해서는 MDP 의 model (reward와 transition probability) 가 요구된다. <br>

따라서 model-free 인 greedy policy improvement over $Q(s,a)$ 를 사용한다. <br>
$\pi ' = \underset{a\in A}{arg\max} Q(s,a)$ <br>

Policy improvement 의 경우 greedy action 으로 진행할 경우 local maximum 에 빠지게 되는 문제가 발생한다. <br>

**$\epsilon$-Greedy Exploration** <br>
With probability $1-\epsilon$ choose the greedy action <br>
With probability $\epsilon$ choose an action at random <br>

$\epsilon$-greedy *guarantees policy improvement* <br>

-For any $\epsilon$-greedy policy $\pi$, the $\epsilon$-greedy policy $\pi '$ with respect to $q _{\pi}$ is an improvement, $v _{\pi '}(s) \geq v _{\pi}(s)$ <br>
$q _{\pi}(s, \pi '(s))=\sum _{a\in A} \pi '(a\|s)q _{\pi}(s,a)$ <br>
$=\epsilon / m \sum _{a\in A} q _{\pi}(s, a) + (1-\epsilon) \underset{a\in A}{\max} q _{\pi}(s,a)$ <br>
$\geq \epsilon/m \sum _{a\in A} q _{\pi}(s,a) + (1-\epsilon)\sum _{a\in A} \frac{\pi(a\|s)-\epsilon /m}{1-\epsilon}q _{\pi}(s,a)$ <br>
$=\sum _{a\in A}\pi(a\|s)q _{\pi}(s,a)=v _{\pi}(s)$ <br>
Therfore from policy improvement theorem, $v _{\pi '}(s) \geq v _{\pi}(s)$ <br>

**MC policy iteration** <br>

**MC control** <br>
policy improvement theorem 에 의해, 반드시 더 좋은 policy 를 얻을 수 있다. <br>
그렇다면 안좋은 기존 policy 로 샘플링을 하며 policy evaluation 을 할 바에는, 하나의 episode 가 끝나자 마자 바로 policy improvement 를 진행하여 새로운 policy 를 얻어보자. <br>

**GLIE** <br>
우리의 목표는 optimal policy 를 찾는 것(policy iteration 을 통해 optimal policy 까지 도달하는 것)인데, 그것이 $\epsilon$-greedy 와 같이 stochastic 할 확률은 매우 낮다. <br>
policy iteration 이 optimal policy 에 도달하는 조건을 정의한 것이 GLIE 이다. <br>

Greedy in the limit with infinite explocation (GLIE) <br>
All state-action paris are explored infinitely many times, <br>
$\underset{k\to \infty}{\lim} N _k(s,a) = \infty$ <br>
The policy converges on a greedy policy, <br>
$\underset{k\to \infty}{\lim} \pi _k(a\|s) = 1(a=\underset{a'\in A}{arg\max} Q _k(s,a'))$ <br>

For example, $\epsilon$-greedy is GLIE if $\epsilon$ reduces to zero at $\epsilon _k = \frac{1}{k}$ <br>

**GLIE MC Control** <br>
For each state $S _t$ and action $A _t$ in the episode, <br>
$N(S _t, A _t) \leftarrow N(S _t, A _t) + 1$ <br>
$Q(S _t, A _t) \leftarrow Q(S _t, A _t) + \frac{1}{N(S _t, A _t)}(G _t-Q(S _t, A _t))$ <br>
Improve policy based on new action-value function <br>
$\epsilon \leftarrow 1/k$ <br>
$\pi \leftarrow \epsilon - greedy(Q)$ <br>

---

### On-policy TD Learning

Use TD instead of MC inour control loop <br>
-Apply TD to $Q(S,A)$ <br>
-Use $\epsilon$-greedy policy improvement <br>
-Update every time-step <br>

**Updating action-value functions with Sarsa** <br>
$Q(S,A) \leftarrow Q(S,A) + \alpha (R + \gamma Q(S',A')-Q(S,A))$ <br>

**On-policy control with Sarsa** <br>
Every time-step: <br>
Policy evaluation Sarsa, $Q\approx q _{\pi}$ <br>
Policy improvement $\epsilon$-greedy <br>

![](/public/img/2022-08-22-RLCoursebyDavidSilver-Lecture5/2.png){: width="50%" height="50%"}{: .center} <br>

**Convergene of Sarsa** <br>
Sarsa converges to the optimal action-value function, $Q(s,a)\rightarrow q _{\*}(s,a)$, under the following conditions: <br>
-GLIE sequence of polices $\pi _t(a\|s)$ <br>
-Robbins-Monro sequence of step-sizes $\alpha _t$ <br>
$\sum _{t=1}^{\infty} \alpha _t = \infty$ <br>
$\sum _{t=1}^{\infty} \alpha _t ^2 < \infty$ <br>
실제로는 fixed step size, fixed experience rate 를 사용해도 잘 동작하기때문에, 위 조건을 무시하고 사용한다고 한다. <br>

**$n$-step Sarsa** <br>
Define the $n$-step Q-return <br>
$q _t^{(n)}=R _{t+1} + \gamma R _{t+2} + \dots + \gamma ^n Q (S _{t+n})$ <br>
$n$-step Sarsa updates $Q(s,a)$ towards the $n$-step Q-return <br>
$Q(S _t, A _t) \leftarrow Q(S _t, A _t) + \alpha (q _t ^{(n)}-Q(S _t, A _t))$ <br>

**Forward view Sarsa($\lambda$)** <br>
$q _t ^{\lambda} = (1-\lambda)\sum _{n=1}^\infty \lambda^{n-1} q _t^{(n)}$ <br>
$Q(S _t, A _t) \leftarrow Q(S _t, A _t) + \alpha(q _t ^\lambda - Q(S _t, A _t))$ <br>

**Backward view Sarsa($\lambda$)** <br>
![](/public/img/2022-08-22-RLCoursebyDavidSilver-Lecture5/3.png){: width="50%" height="50%"}{: .center} <br>

**Sarsa($\lambda$) algorithm** <br>
![](/public/img/2022-08-22-RLCoursebyDavidSilver-Lecture5/4.png){: width="50%" height="50%"}{: .center} <br>

---

### Off-policy learning

**Evaluate target policy $\pi(a\|s)$** to compute $v _{\pi}(s)$ or $q _{\pi}(s,a)$ while **following behaviour policy $\mu(a\|s)$** <br>

-Why is this important? <br>
-Learn from observing humans or other agents <br>
-Re-use experience generated from old polices $\pi _1, \pi _2, \dots, \pi _{t-1}$ <br>
-Learn about **optimal** policy while following **exploratory** policy <br>
-Learn about multiple policies while following one policy <br>

**Importance Sampling** <br>
Estiamte the expectation of a different distribution <br>
$E _{X\sim P}[f(X)] = \sum P(X) f(X)$ <br>
$=\sum Q(X) \frac{P(X)}{Q(X)} f(X)$ <br>
$=E _{X\sim Q}[\frac{P(X)}{Q(X)} f(X)]$ <br>

**Importance sampling for off-policy MC** <br>
Multiply importance sampling corrections along whole episode <br>
$G _t^{\pi/\mu} = \frac{\pi(A _t \|S _t)}{\mu(A _t\|S _t)}\frac{\pi(A _{t+1} \|S _{t+1})}{\mu(A _{t+1}\|S _{t+1})} \dots \frac{\pi(A _T \|S _T)}{\mu(A _T\|S _T)} G _t$ <br>
Update value towards corrected return <br>
$V(S _t) \leftarrow V(S _t) + \alpha(G _t^{\pi / \mu}-V(S _t))$ <br>

위와 같은 형태는 variacne 가 dramatically 하게 커지기 때문에 사용하기 어렵다. <br>
또한 $\mu$ 가 0 이고 $\pi$ 가 0 이 아닐경우 사용할 수 없다.

**Importance sampling for off-policy TD** <br>
MC 와 다르게 TD 의 경우 single improtance sampling correction 만을 요구하기 variance 가 작아진다. <br>
Weight TD target $R + \gamma V(S')$ by importance sampling <br>
Polices only need to be similar over a single step<br>
$V(S _t) \leftarrow V(S _t) + \alpha(\frac{\pi(A _t\|S _t)}{\mu (A _t\| S _t)}(R _{t+1} + \gamma V(S _{t+1}))-V(S _t))$ <br>

**Q-learning** <br>
**Importance sampling 을 사용하지 않으면서** off-policy learning 을 진행하기 위해 action-values $Q(s,a)$ 에 대해 학습을 진행한다. <br>
Next action is chosen using behaviour policy $A _{t+1}\sim \mu(\cdot \|S _t)$ <br>
But we consider alternative successor action $A' \sim \pi(\cdot \|S _t)$ <br>
And update $Q(S _t, A _t)$ towards value of alternative action <br>
$Q(S _t, A _t)\leftarrow Q(S _t, A _t) + \alpha(R _{t+1}+\gamma Q(S _{t+1}, A') - Q(S _t, A _t))$ <br>


**Off-policy control with Q-learning** <br>
We now allow both behaviour and target polices to **improve** <br>
The target policy $\pi$ is greedy w.r.t. $Q(s,a)$ <br>
$\pi(S _{t+1}) = \underset{a'}{arg\max}Q(S _{t+1}, a')$ <br>

The behaviour policy $\mu$ is e.g. $\epsilon$-greedy w.r.t. $Q(s,a)$ <br>
The Q-learning target then simplifies: <br>
$R _{t+1} + \gamma Q(S _{t+1}, A')$ <br>
$=R _{t+1} + \gamma Q(S _{t+1}, \underset{a'}{arg\max} Q (S {t+1}, a'))$ <br>
$=R _{t+1} + \underset{a'}{\max} \gamma Q (S _{t+1}, a')$ <br>

**Q-learning control algorithm** <br>
$Q(S, A)\leftarrow Q(S, A) + \alpha(R+\gamma \underset{a'}{\max} Q(S', a') - Q(S , A))$ <br>

![](/public/img/2022-08-22-RLCoursebyDavidSilver-Lecture5/5.png){: width="50%" height="50%"}{: .center} <br>

---

### Summary

![](/public/img/2022-08-22-RLCoursebyDavidSilver-Lecture5/6.png){: width="50%" height="50%"}{: .center} <br>

![](/public/img/2022-08-22-RLCoursebyDavidSilver-Lecture5/7.png){: width="50%" height="50%"}{: .center} <br>

---

## 원본 글: [Gitbook](https://dnddnjs.gitbooks.io/rl/content/q_learning.html)

**On-policy vs Off-policy**<br>
On-policy 에는 탐험의 문제라는 한계가 존재한다. <br>
현재 알고있는 정보에 대해 greedy 로 policy 를 정해버리면 optimal 에 가지 못할 확률이 커지기 때문에 agent 는 항상 탐험이 필요하다. <br>
따라서 on-policy 처럼 움직이는 policy 와 학습하는 policy 가 같은 것이 아니고 이 두개의 policy 를 분리시킨 것이 off-policy 이다. <br>

Off-policy 는 다음과 같은 장점이 있다. <br>
-다른 agent 나 사람을 관찰하고 그로부터 학습할 수 있다. <br>
-이전의 policy들을 재활용하여 학습할 수 있다. <br>
-탐험을 계속 하면서도 optimal 한 policy를 학습할 수 있다 (Q-learning) <br>
-하나의 policy 를 따르면서 여러개의 policy 를 학습할 수 있다. <br>

**Importance sampling** <br>
$f(X)$ 라는 함수를 value function 이라고 생각하고 강화학습에서는 이 value function = expected future reward 를 계속 추정해나가는데 $P(X)$ 라는 현재 policy 로 형성된 distribution 으로부터 학습을 하고 있었다. <br>
하지만 다른 $Q$ 라는 distribution 을 따르면서도 똑같이 학습할 수 있는데, 이때 아래와 같이 importance sampling 이 사용된다. <br>
$E _{X\sim P}[f(X)] = E _{X\sim Q}[\frac{P(X)}{Q(X)}f(X)]$ <br>

**Sarsa vs Q-learning** <br> [참조](https://www.youtube.com/watch?v=Fj5HBT1vloU&list=PL_iJu012NOxehE8fdF9me4TLfbdv3ZW8g&index=12) <br>

"Cliff Walking" 예제를 통해 두 방식에 대해 비교해보자. <br>

![](/public/img/2022-08-22-RLCoursebyDavidSilver-Lecture5/8.png){: width="50%" height="50%"}{: .center} <br>

이 예제의 목표는 S 라는 start state 에서 시작해 Goal 까지 가는 optimal path 를 찾는 것이다. <br>
Clif 에 빠지면 -100 dml reward 를 받고 time-step 마다 reward 를 -1 씩 받는다. <br>

눈으로 딱 보면 그림에 있는 optimal path 가 정답인걸 알 수 있다. <br>
Sarsa 와 Q-learning 모두 다 $\epsilon$-greedy 한 policy 로 움직인다. <br>
따라서 더러는 cliff 에 빠져버리기도 한다. <br>
차이는 Sarsa 는 on-policy 라서 그렇게 cliff 에 빠져버리는 결과로 인해 그 주변의 상태들의 value 를 낮다고 판다한다. <br>
하지만 Q-learning 의 경우에는 비록 $\epsilon$-greedy 로 인해 cliff 에 빠져버릴지라도 자신이 직접 체험한 그 결과가 아니라 greedy 한 policy 로 인한 Q function 을 이용해서 업데이트한다. <br>
따라서 cliff 근처의 길도 Q-learning 은 optimal path 라고 판단할 수 있어 해당 문제의 경우 sarsa 보다 Q-learning 이 적합하다고 할 수 있다. <br>

Sarsa 에서 탐험을 위해 $\epsilon$-greedy 를 사용했지만 결국은 그로인해 정작 agent 가 optimal 로 수렴하지 못하는 현상들이 발생한 것이다. <br>
따라서 Q-learning 의 등장 이후로 많은 문제에서 Q-learning 이 더 효율적으로 문제를 풀었기 때문에 강화학습에서 Q-learning 은 기본적인 알고리즘으로 자리를 잡게 된다. <br>
