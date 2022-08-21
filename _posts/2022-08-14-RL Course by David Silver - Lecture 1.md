---
layout: post
title: RL Course by David Silver - Lecture 1
date: 2022-08-14 11:59:59 +0900
categories: RL
permalink: /8
---

# [RL Course by David Silver - Lecture 1](https://www.youtube.com/watch?v=2pWv7GOvuf0)

## Textbooks
- Algorithms for Reinforcement Learning, Szepesvari
  - http://www.ualberta.ca/~szepesva/papers/RLAlgsInMDPs.pdf


## About reinforcement learning

RL is about decision making <br>
- Many faces of RL
  - Neuroscience and dopamine
- Characteristics of RL
  - There is no supervisor, only a reward signal
  - Feedback is delayed, not instantaneous
  - Time really matters (sequential, non i.i.d data)
  - Agent's actions affect the subsequent data it receives
- The RL problems
  - Reward
    - a reward is a scalar(need to be able to compare with other value) feedback signal
    - All goals can be described by the maximization of expected cumulative reward
  - Agent and environment
    - observation, reward, action 
    - history is the sequence of observations, actions, rewards
  - Formally, "State" is a function of the history
    - $S _t=f(H _t)$
    - Environment state
      - the environment state $S _t^e$ is the environment's private representation
      - the environment state is not usually visible to the agent
      - Even if $S _t^e$ is visible, it may contain irrelevant information
    - Agent state
      - the agent state $S _t^a$ is the agent's internal representation
      - it is the information used by rl algorithms
      - it can be any function of history: $S _t^a=f(H _t)$
    - Information state
      - an information state (a.k.a Markov state) contains all useful information from the history
      - $P[S _{t+1}|S _t] = P[S _{t+1}|S _1, \dots, S _t]
      - The environment state $S _t^e$ is Markov
      - The history $H _t$ is Markov
    - Fully observable environments
      - agent directly observes environment state
      - $O _t = S _t^a = S _t^e$
      - formally, this is a Markov decision process (MDP)
    - Partially observable environments
      - agent inderectly observes environment
      - now agent state $\neq$ environemnt state
      - formally this is a partially observable Markov decision process (POMDP)
      - agent must contruct its own state representation $S _t^a$
        - Complete history: $S _t^a = H _t$
        - Belifs of environment state: $S _t^a (P[S _t^e=s^1], \dots , P[S _t^n=s^n])$
        - Recurrent neural network: $S _t^a=\sigma(S _{t-1}^a W _s + O _t W _o)$
- Inside RL Agent
  - Major components of an RL agnet
    - Policy: agent's behaviour function
    - Value function: how good is each state and/or action
    - Model: agent's representation of the environment
  - Policy
    - deterministic policy: $a=\pi(s)$
    - stochastic policy: $\pi(a\|s) = P[A=a\|S=s]$
  - Value function  
    - value function is a prediction of future reward
    - $v _\pi(s) = E _{\pi} [R _t + \gamma R _{t+1} + \dots \| S _t = s]$
  - Model
    - a model predicts what the environment will do next
    - Transition: $P$ predicts the next state
    - Rewards: $R$ predicts the next (immediate) reward
      - $P _{SS'}^a = P[S'=s' \| S=s, A=a]$
      - $R _{S}^a = E[R\|S=s, A=a]$
  - Categorizing RL agents
    - Value based
      - no policy (implicit)
      - contains value function
    - Policy based
      - containt policy
      - no value function
    - Actor critic
      - containt policy and value function
    - Model free
      - policy and/or value function
      - no model
    - Model based
      - policy and/or value function
      - model
- **Problems within Reinforcement learning**
  - Learning and planning
    - two fundamental problems in sequential decision making
    - Reinforcement learning
      - the environment is initally unknown
      - the agent interacts with the environment
      - the agent improves its policy
    - Planning
      - a model of the environmetn is known
      - the agent performs computations with its model (without any external interaction)
      - the agent improves its policy 
  - Exploration and Exploitation
    - Exploration finds more information about the environment
    - Exploitation exploits known information to maximize reward
  - Prediction and Control
    - prediction: evaluate the future
      - given a policy
    - control: optimize the future
      - find the best policy

---

## 원본 글: [Gitbook](https://dnddnjs.gitbooks.io/rl/content/reinforcement_learning.html)

> Reinforcement learning is defined not by characterizing learning methods, but by characterizing a learning problem 

[DP lecture](http://adpthu2014.weebly.com/slides--materials.html)
[MDP book](https://www.wiley.com/en-sg/Markov+Decision+Processes%3A+Discrete+Stochastic+Dynamic+Programming-p-9780471727828)

