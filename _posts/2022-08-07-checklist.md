---
layout: post
title: checklist
date: 2022-08-07 14:06:00 +0900
categories: etc
permalink: /0
---

# checklist

0. What did you do during all day?
   - 하루 동안 한 일 자세하게 작성

1. [Bill Evans](https://www.youtube.com/watch?v=anH8Y8vAz2Q&t=110s)
   - 자신이 어느 단계에 있는지 알고 진실되고, 현실적이고 정확하게 수행해야 한다.
   - 사람들은 작은 부분을 현실적으로 해결하려는 대신 하나로 뭉뚱그려서 커다란 문제로 본다.
   - 한번에 모든 것을 할 수 없다. 모든 문제를 가져와서 이것을 하나의 큰 문제로 만들면 무언가 잡히는 것 같겠지만 전혀 그렇지 않다.

2. [Kurzgesagt](https://www.youtube.com/watch?v=75d_29QWELk)
   - easy situation 이 trigger 가 되게 해서 hard action 을 습관으로 만들수 있다. 

|situation|condition|hard action|
|---|---|---|
|컴퓨터를 켰을 때|가장 먼저|blog 에 어떤 글이라도 작성|
|컴퓨터를 켰을 때|가장 먼저|'Effective python' 에 대해 작은 분량이라도 정리|
|누웠을 때|핸드폰 보기 전에|'수학으로 풀어보는 강화학습' 보기|
|누웠을 때|핸드폰 볼때|누워서 하는 운동|
|누웠을 때|핸드폰 볼때|유명 연구/개발자 SNS 확인|
|잠에서 깨면|핸드폰 보기 전에|일어나서 물 마시기|
|이상할 때, 내가 잘못 이해한것 같을 때, 수상할 때|가장 먼저|질문, 문제 해결|
|**한 세션이 끝나면**|**가장 먼저**|**기록**|

---
- **IN PROGRESS**
  - David Silver lecture
  - Gitbook [Fundamental of Reinforcment Learning](https://dnddnjs.gitbooks.io/rl/content/)
  - Effective python
  - 수학으로 풀어보는 강화학습 원리와 알고리즘

- **ICE BOX (<U>MAXLEN=5</U>)**
  - [ ] Build env for RL
    - [ ] [DQN tutorial](https://tutorials.pytorch.kr/intermediate/reinforcement_q_learning.html)
    - [ ] [mario turtorial](https://tutorials.pytorch.kr/intermediate/mario_rl_tutorial.html)
  - [ ] https://sites.ualberta.ca/~szepesva/papers/RLAlgsInMDPs.pdf
  - [ ] Importance sampling
  - [ ] [VAE, reparametrization trick](https://jaejunyoo.blogspot.com/2017/04/auto-encoding-variational-bayes-vae-1.html)
    - [PR-010: Auto-Encoding Variational Bayes, ICLR 2014](https://www.youtube.com/watch?v=KYA-GEhObIs&list=PLlMkM4tgfjnJhhd4wn5aj8fVTYJwIpWkS&index=12)
  - [ ] [InfoGAN 논문 재작성](https://www.inference.vc/infogan-variational-bound-on-mutual-information-twice/)
    - uppder bound of mutual information
    - [On Information Theoretic Bounds for SGD](https://www.inference.vc/on-information-theoretic-bounds-for-sgd/)
    - [openai code](https://github.com/openai/InfoGAN/blob/master/infogan/algos/infogan_trainer.py)
  
- **FREEZER (<U>MAXLEN=10</U>)**
  - [ ] Inverse RL
  - [ ] ICML 2022 review
  - [ ] Information Geometry
    - [ ] https://franknielsen.github.io/
    - [ ] https://www.youtube.com/watch?v=FlyJJIQo-g4&list=PLHZhjPByiV3L94AeJ9FcK1yrnRDOt3Vit
  - [ ] AlphaGo Zero
  - [ ] Multi armed bandit
  - [ ] [Self-Normalizing Neural Networks](https://arxiv.org/abs/1706.02515)

- **COMPLETE**
  - [x] Batch normalization
  - [x] [How to study RL](https://github.com/reinforcement-learning-kr/how_to_study_rl)
  - [x] Central limit theorem 
  - [x] Law of total probability, expectation, and variance

---

Twitter list <br>
John Schulman, OpenAI, DeepMind, NeurIPS, Ian Goodfellow, Andrew Ng, ICML, ICLR,

---

- 2022/08/07
  - [x] 블로그에 checklist 작성
  - [x] 트위터 팔로우
  - [ ] 누워서 하는 운동 정하기 (하나씩 추가)
    - [x] [브릿지](https://brunch.co.kr/@tenbody/1486)
    - [x] [레그레이즈](https://brunch.co.kr/@tenbody/1486)
  - [ ] ~~InfoGAN 논문 review~~
  - [x] 새로운 Denoising network 로 sample 생성
  - [x] 쉬운 condition 의 simulation 이미지로 unmixing 실험
  - [x] 화장실 배수구 뚫기
  - [x] 빨래
- 2022/08/08
  - [x] Denoising network 결과 정리
  - [x] InfoGAN 논문 review
    -  앞으로 review code 작성할 때 눈으로 먼저 코드 전체 동작 구조 확인
  - [ ] ~~CycleGAN 논문 review~~
- 2022/08/09
  - [x] [github.io 에 mathjax 추가](http://csega.github.io/mypost/2017/03/28/how-to-set-up-mathjax-on-jekyll-and-github-properly.html)
  - [x] github.io 의 latex 문법 오류
    - p_{data} => $p_{data}$ / p _{data} => $p _{data}$
    - p(x|y) => $p(x|y)$ / p(x\|y) => $p(x\|y)$
    - * -> \*
    - image 경로에 포함된 space 는 %20 로 인코딩
  - [x] github.io 의 이미지 상대경로 오류
    - html 문법 이용하면 오류 발생 (이유 모르겠음)
    - permalink 설정 필요
    - image center align css 추가
  - [x] ice box, freezer, maxlen 추가
- 2022/08/10
  - [ ] ~~Information theory book 정리~~
  - [x] Batch normalization
  - [x] [How to study RL](https://github.com/reinforcement-learning-kr/how_to_study_rl)
- 2022/08/11
  - [x] Central limit theorem
  - [x] Law of total ... 시리즈 정리
- 2022/08/12
  - [ ] ~~Information theory book 정리~~
  - Reinforcement learning
    - [ ] ~~David Silver lecture~~
    - [ ] ~~수학으로 풀어보는 강화학습 원리와 알고리즘~~
- 2022/08/13
  - [ ] ~~KLD, JSD 증명 정리~~
- 2022/08/14
  - [ ] ~~**Information theory book 정리**~~
  - **Reinforcement learning**
    - [x] David Silver lecture 1
    - [x] 수학으로 풀어보는 강화학습 원리와 알고리즘
    - [ ] https://dibyaghosh.com/blog/probability/kldivergence.html
    - [ ] https://theeluwin.postype.com/post/6080524
- 2022/08/15
  - [x] 도서관 책 반납
  - [x] David Silver lecture 2
- 2022/08/16
  - [ ] ~~Deep infomax~~
  - [x] David Silver lecture 3
    - [ ] Contraction mapping
- 2022/08/17
  - [ ] ~~실험 결과 정리~~
  - [ ] ~~**Information theory book 정리**~~
- 2022/08/18
  - [x] 실험 결과 정리
  - [ ] **Information theory book 정리**
  - [x] Deep infomax youtube 정리
- 2022/08/19
  - [x] David Silver lecture 4
    - [ ] Proof
  - [ ] KLD, JSD 증명 정리
  - [x] Effective python - preface
- 2022/08/20
  - [x] [Gitbook](https://dnddnjs.gitbooks.io/rl/content/) reading chapter 1
  - [x] [Gitbook](https://dnddnjs.gitbooks.io/rl/content/) reading chapter 2
  - [x] [Gitbook](https://dnddnjs.gitbooks.io/rl/content/) reading chapter 3
  - [x] [Gitbook](https://dnddnjs.gitbooks.io/rl/content/) reading chapter 4
- 2022/08/21
  - [x] Effective python - chapter 1 (Better way 1~2)
  - [x] [Gitbook](https://dnddnjs.gitbooks.io/rl/content/) reading chapter 5
  - [x] [Gitbook](https://dnddnjs.gitbooks.io/rl/content/) reading chapter 6
- 2022/08/22
  - [x] Effective python - chapter 1 (Better way 3)
  - [x] David Silver lecture 5
    - [ ] Cliff walking question
- 2022/08/23
  - [ ] ~~David Silver lecture 6~~
  - [x] [Gitbook](https://dnddnjs.gitbooks.io/rl/content/) reading chapter 7
- 2022/08/24
  - [x] David Silver lecture 6
    - [ ] Least squares policy iteration