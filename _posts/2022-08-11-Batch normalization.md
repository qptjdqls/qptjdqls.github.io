---
layout: post
title: Batch normalization
date: 2022-08-11 11:59:59 +0900
categories: DL
permalink: /4
---

# Batch normalization

## 원본 글:
## [https://gaussian37.github.io](https://gaussian37.github.io/dl-concept-batchnorm/)

## 1. Internal covariant shift
Batch 단위로 학습을 하게 되는 문제점을 저자들은 Internal covariant shift 라고 한다. <br/>
Internal covariant shift 는 아래 그림과 같이 학습 과정에서 계층 별로 입력의 데이터 분포가 달라지는 현상을 말한다. <br/>
이와 유사하게 batch 단위로 학습을 하게되면 batch 단위간에 데이터 분포의 차이가 발생한다. <br/>
![](/public/img/2022-08-11-Batch normalization/3.png){: width="50%" height="50%"}{: .center}


## 2. Batch normalization
batch normalization 은 학습 과정에서 각 배치 단위 별로 데이터가 다양한 분포를 가지더라도 각 배치별로 평균과 분산을 이용해 평균 0, 표준 편차 1의 가우시안 분포로 정규화 하는 것을 의미한다.<br/>
학습 단계의 배치 정규화를 수식으로 나타내면 다음과 같다. <br/>
$BN(X)=\gamma(\frac{X-\mu _{batch}}{\sigma _{batch}})+\beta$ <br/>
$\gamma$ 는 스케일링 $\beta$ 는 bias 이고 두 값 모두 backpropagation 을 통해 학습된다. <br/>


## 3. 추론 단계의 배치 정규화
추론 단계의 배치 정규화를 수식으로 나타내면 다음과 같다. <br/>
$BN(X)=\gamma(\frac{X-\mu _{BN}}{\sigma _{BN}})+\beta$ <br/>
$\mu _{BN} = \frac{1}{N} \sum _{i} \mu^i _{batch}$ <br/>
$\sigma^2 _{BN} = \frac{1}{N} \sum _{i} \sigma^{i} _{batch}$ <br/>
추론 과정에서는 $BN$ 에 적용할 평균과 분산에 고정값을 사용하고, 이러한 고정된 평균과 분산은 학습 과정에서 이동 평균 또는 지수 이동 평균을 통해 계산한 값으로 얻을 수 있다. $\gamma$, $\beta$ 는 학습 과정에서 학습한 파라미터를 사용한다.


## 4. 정리
딥러닝에서 Layer가 많아질 때 학습이 어려워지는 이유는 weight의 미세한 변화들이 가중이 되어 쌓이면 Hidden Layer의 깊이가 깊어질수록 그 변화가 누적되어 커지기 때문이다. <br/>
이 문제를 다루기 위하여 처음에는 weight 초기화를 잘 해주는 방법 또는 작은 learning rate를 주어서 변화량을 줄이려는 방법이 사용되기도 하였다. <br/>

__Batch normalization 은 이러한 Internal covariant shift 문제를 해결하기 위해 activation function 안에 들어가는 input range 제한하는 방법을 사용한다.__ <br/>
__Hidden Layer → Batch Normalization → Activation Function 순서로 적용되는 것이 기본적인 적용 방법이다.__ <br/>

Batch normalization 을 적용함으로써 높은 learning rate 를 사용할 수 있게 되었고, 또한 Batch normalization 에서는 평균과 분산이 지속적으로 변하고 weight 업데이트에도 계속 영향을 주어 한 weight 에 가중치가 큰 방향으로만 학습되지 않기 때문에 Regularization effect를 얻을 수 있다. <br/> 


## 5. Batch normalization 의 한계
Batch Normalization은 Batch의 크기에 영향을 많이 받는다. <br/>
만약 batch 의 크기가 작을 경우 큰수의 법칙과 중심 극한 이론을 활용할 만한 샘플 수를 얻을 수 없기 때문에 평균과 표준 편차가 데이터 전체 분포를 잘 표현하지 못할 수 있다. <br/>
~~반대로 batch 의 크기가 클 경우에도 잘 동작하지 않는다. 적절한 크기의 샘플은 중심 극한 이론을 통하여 적절한 정규 분포를 따르게 되나 굉장히 큰 샘플의 경우 multi modal 형태의 gaussian mixture 모델이 나타날 수 있기 때문이다.~~ <br/>
이러한 batch normalization의 한계를 개선하기 위하여 weight normalization이나, layer normalization 등이 사용되기도 한다. <br/>


