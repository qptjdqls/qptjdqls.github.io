---
layout: post
title: How to study RL
date: 2022-08-11 11:59:59 +0900
categories: RL
permalink: /5
---

# How to study RL

## 원본 글:
## [https://github.com/reinforcement-learning-kr/how_to_study_rl](https://github.com/reinforcement-learning-kr/how_to_study_rl)

## 우리는 어떻게 강화학습을 공부했는가
* 나의 강화학습 공부 스토리
    - [ ] David Silver RL Course
    - [ ] 수학으로 풀어보는 강화학습 원리와 알고리즘
    - [ ] Gitbook [Fundamental of Reinforcment Learning](https://dnddnjs.gitbooks.io/rl/content/)
    - [ ] [강화학습 대회](https://github.com/seungjaeryanlee/awesome-rl-competitions) 참여
    - [ ] John Schulmann 강의자료
    - [ ] UC berkely CS188
    - [ ] UC berkely CS294
    - 구현
      - [ ] REINFORCE
      - [ ] DQN
      - [ ] Actor-Critic
      - [ ] A3C
      - [ ] DDQN
      - [ ] Dueling Network
      - [ ] PER
      - [ ] NoiseNET
      - [ ] C51
      - [ ] RAINBOW
      - [ ] A2C
      - [ ] NPG
      - [ ] 피지여행
        - [ ] NPG
        - [ ] TRPO
        - [ ] DDPG
        - [ ] PPO
      - [ ] Inverse RL
        - [ ] Algorithms for Inverse Reinforcement Learning
        - [ ] APP
        - [ ] MMP
        - [ ] MaxEnt
        - [ ] GAIL
        - [ ] VAIL
      - [ ] 몬테주마 복수
        - [ ] TMC+CMC
        - [ ] FeUdal network
        - [ ] RND   

---

1. 이웅원의 강화학습 공부 스토리
   - David Silver RL Course + Richard Sutton 부교재
   - 3개월 스터디 후 홈페이지와 gitbook 에 관련 내용 정리
   - RLCode 모임을 통해 알까기 강화학습
   - 강화학습 책(파이썬과 케라스로 배우는 강화학습)을 작성
   - John Schulmann 강의자료 + UC berkely 의 CS188, CS294
   - 당시 등장한 Keras 로 REINFORCE, DQN, Actor-Critic, A3C 구현
   - 이후 DDQN, Dueling Network, PER, NoiseNET, C51, RAINBOW, A2C 공부
   - NIPS 2017 competitoin 인 Learning to run 에 참여
   - RLKorea 창설
   - 피지여행 프로젝트로 NPG, TRPO, DDPG, PPO 공부
   - Mujoco, Unitiy ML-agent 의 walker 환경에 PPO 적용
   - 알파오목 프로젝트로 알파고 제로 알고리즘을 오목에 적용
   - 몬테주마 복수 시리즈로 TMC+CMC, FeUdal network, RND 공부
   - 다양한 강화학습올 논문을 지속적으로 리뷰
   - 강화학습 논문은 트위터, arxiv sanity, 각종 학회 제출 논문, Google Brain 블로그, DeepMind 블로그, OpenAI 블로그에서 접하는 중

2. 이의령의 강화학습 공부 스토리
   - 강화학습이 게임 도메인 말고 어느 분야에서 발전해 나갈 수 있을까 고민
   - 3D Environment 환경에서 Visual interction 형식으로 학습 (RL+Vision+NLP)
  
3. 민규식의 강화학습 공부 스토리
   - Udacity 의 Aritificial intelligence 강의를 통해 RL 접함
   - DQN, Double DQN, Prioritized Experience Replay
   - Dueling network architectures for deep reinfocemetn learning
   - Deep recurrent Q-learning for partially observable MDPs
   - Noisy networks for exploration
   - A distributional perspective on reinfocement learning
   - 위 논문들을 읽고 tensorflow 로 구현
   - 알파고 제로 알고리즘을 오목에 적용한 알파오목 프로젝트
   - Distributional RL 스터디 진행
   - A distributional perspective on reinforcement learning
   - distributional reinforcement learning with quantile regression
   - Implicit quantile networks for distributoinal reinfocement learning
   - Distributional bellman and the C51 algorithm (블로그)
   - Distributional RL (블로그)
   - Quantile reinforcement learning (블로그)
   - 위 논문들을 리뷰 및 코드 정리
   - 1년정도 Unity ML-agents 공부
   - ML-agents 를 이용하여 간단한 자율주행 환경을 구성하여 2018 Intelligent vehicle symposium 에 논문 제출
   - 프로젝트로 Unity ML-agents 에서 간단한 환경을 만들고 알고리즘을 검증해볼 수 있도록 튜터리얼 정리 중
   - 최근에는 imitation learning 이나 multi-agent RL 도 공부 중
   - 논문이나 블로그 등에 대한 정보를 트위터나 페이스북을 통해 많이 접함
   - 트위터에서 deepmind, openAI, BAIR 혹은 강화학습 관련 유명한 연구자들을 팔로우

4. 유지원의 강화학습 공부 스토리
   - sungkim 교수의 모두를 위한 딥러닝 과 모두를 위한 RL 강좌로 입문
   - 주어진 시간이 별로 없었기에, 2주일에 걸쳐 모든 강의를 듣고 구현
   - 네이버 D2 세미나 & David Silver RL course
   - 이웅원의 gitbook & 파이썬과 케라스로 배우는 강화학습
   - gym 의 cartpole 과 atari 게임 등 여러 환경에 DQN, A3C 적용
   - Roboschool, Carla 같은 시뮬레이터 상에 value-based 접근이 가진 한계를 인식
   - 연속적인 동작에 대한 제어를 연구하기 위해 모두의 연구소에 지원
   - 피지여행 프로젝트를 팔로우업 하며 PG 에 대해 공부
   - RLCode 와 A3C 쉽고 깊게 이해하기 (네이버 D2) 를 돌려보며 기본에 대한 감각 유지
   - 피지여행 프로젝트를 통해 Sutton PG, NPG, TRPO 를 쉽게 이해
   - PR12 논문 읽기 모임 출퇴근 영상으로 보는 중
   - 각잡고 로봇팔 프로젝트 진행중
   - 계층형 강화학습, Data-efficient hierarchical reinforcement learning 을 구현하고자 함
   - Addressing Function approximatoin error in actor-critic methods, DDPG 리뷰
   - 빠르게 성장하는 강화학습을 이해하는데 있어 가장 중요한 점은 지속적인 관심
   - 개인적으로 모션캡쳐 장비를 이용해 실제 로보틱스에 구현하는 것을 목표로 삼고 있음
   - Deepmimic 완벽 이해 및 간단한 모션 모방을 단기 목표로 삼고 있음

5. 이동민의 강화학습 공부 스토리
   - 김성훈 교수의 Deep reinforcement learning 강의
   - 파이썬과 케라스로 배우는 강화학습 책 공부
   - MDP, SARSA, Q-learning, Deep SARSA, REINFORCE, DQN, Actor-critic, A3C
   - Richard Sutton 책으로 3개월간 공부
   - David Silver 의 RL  Course
   - 피지여행 프로젝트를 통해 총 7개의 논문 리뷰
   - Vanilla PG, DPG, DDPG, NPG, TRPO, GAE, PPO
   - Inverse RL 프로젝트 시작
   - Algorithms for Inverse reinforcement learning, APP, MMP, MaxEnt, GAIL, VAIL
   - Mountain Car 과 MuJoCo Hopper 환경을 통해 구현
   - Safe reinforcement learning 에 관심이 많음

6. 이승재의 강화학습 공부 스토리
   - Andrew Ng 의 Mahcine learning 과 Deep learning specialization 수업
   - 이후 Davild Silver RL Course
   - 6개월 정도 스터디를 통해 Sutton & Barto 를 읽으며 유명한 논문 (Rainbow, PPO) 몇 개에 초점을 맞춰 공부
   - [강화학습 대회](https://github.com/seungjaeryanlee/awesome-rl-competitions)에 참여
   - 처음 OpenAI Retro Contest 에 참여
   - 이후 NeurIPS 2018 AI Prosthetic Challenge, Unity Obstacle Tower Challenge 등 참여
   - 최신 논문이 나오는 속도를 따라잡기 위해 빠르게 논문의 중요성을 판별하는 법과 논문이 말하고자 하는 것을 찾는 법을 터득
   - 논문을 정리하며 강화학습 뉴스레터 시작
   - 구현하는 능력 필요
   - 2019 여름 현재 Google summer of code 라는 프로그램을 통해 Tensorflow 정식 강화학습 라이브러리 TF-Agents 에 기여중

7. 이정우의 강화학습 공부 스토리
   - 모두의 연구소에서 진행했던 AI college 활동을 하며 RL 시작
   - 스타크래프트를 이용한 환경에서 여러 task 를 DQN, A2C, PPO 를 구현해 해결
   - 당시 multi-agent RL 에 대한 공부를 토앻 시야를 넓힘
   - 레빈 교수의 CS285
   - DeepMind 의 주요 알고리즘 논문 리뷰를 하는 'Deepmind 따라가기'
   - Model-based RL 공부
   - 최근 관심사는 Continual learning, diversity, option 에 관련된 연구를 살펴보는 중
   - 현재는 행동중심의 강화학습 뿐만 아니라, NLP, CV 등 기존에 잘 적용되지 않았던 분야에서의 강화학습 분야에 대해 연구

8. 정규열의 강화학습 공부 스토리
   - 게임업계에서 프로그래머로 근무 중 Multi-agent rl 을 연구하고 싶어 대학원 진학
   - 기본이 부족한 상태여서 DQN, PG 직접 구현
   - 기본을 쌓고 MARL 본격적으로 연구
   - QMIX 코드와 논문 집중 분석 (SMAC 환경) 
   - Actor-critic 기반 COMA 모델 분석 후 현실적인 문제에 대응시키기 위한 모델을 고안
   - 이후 심화된 MARL 기법 파악
   - DOP, ROMA, RODE 분석
   - 게임업계에서 근무할때 사용했던 Unity3D 를 이용했기 때문에 ML-Agent 또한 공부
   - 텐서플로와 유니티 ML-Agnets 로 배우는 강화학습 공부
   - 비행슈팅게임을 직접 만들어 강화학습 진행
   - PreadatorPrey 게임을 만들어 QMIX 로 학습
   - 현재는 Continual Learning 공부

---

## 강화학습 관련 노하우
1. 시작
   - 딥러닝에 대한 기본적인 이해를 하고 Deep RL 을 공부하는게 이해하기 좋다
   - 강화학습 알고리즘의 분류 체계를 볼 때는 OpenAI Spinning Up 에 있는 [A Taxonomy of RL Algorithms](https://spinningup.openai.com/en/latest/spinningup/rl_intro2.html#) 을 참고하면 좋다
   - 강화학습의 세부분야의 흐름이나 중요한 논문들은 OpenAI Spinnig up 에 있는 [Key Papers in Deep RL](https://spinningup.openai.com/en/latest/spinningup/keypapers.html) 을 참고하면 좋다
   - 강화학습 기초 공부를 끝냈다면 domain 이나 task 를 하나 정해서 프로젝트 식으로 진행해보는 것이 좋다

2. 논문
   - 관련 블로그도 좋지만 원 저자의 자료를 보는 것이 좋다
   - 논문도 틀릴 수 있기 때문에 너무 틀린 부분에 시간을 쏟지 않는 것이 좋다
   - 단순히 논문만 읽고 알고리즘을 구현하기 보다는 다음과 같은 것들을 생각해보고 구현하는 것이 좋다
     - 어떠한 문제 때문에 나오게 되었으며, 무엇을 말하고자 하는지, 왜 그것이 중요한지
     - 장점
       - 강점
       - 의의
     - 단점
       - 부실한 점
       - 보충되어야할 점
       - 논문을 글로서 보는 것보다는 이 논문에서 하고자하는 방법론 측면에서의 단점
       - 꼭 이 개념 or 방법을 이렇게 쓸 필요가 있었을까? 다른 방법은 없을까?
   - ICLR 등 논문에 대한 리뷰가 공개되어있느 논문의 경우, 논문을 읽은 후 리뷰를 보면서 논문을 좀 더 비판적으로 보는 방법을 기를 수 있다

3. 환경
상세 내용은 [원본 글](https://github.com/reinforcement-learning-kr/how_to_study_rl) 참조

4. 구현
   - 블로그나 다른 사람이 구현한 github 코드들은 틀릴 수 있으니 참고한 한다
상세 내용은 [원본 글](https://github.com/reinforcement-learning-kr/how_to_study_rl) 참조

---

## 강화학습 관련 자료
상세 내용은 [원본 글](https://github.com/reinforcement-learning-kr/how_to_study_rl) 참조