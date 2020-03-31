---
toc: true
layout: post
comments: true
categories: [Meta Pseudo Label]
hide: true
title: 논문 읽기 - Meta Pseudo Label
---
# Meta Pseudo Label
* [Meta Pseudo Labels](https://arxiv.org/abs/2003.10580)
* [Meta Pseudo Labels Review](https://youtu.be/yhItocvAaq0) by Henry AI Labs

# 훑기

* 심층신경망 학습은 네트웍의 예측과 타겟 분포 사이의 cross entropy를 최소화하는 것으로 해석할 수 있음.
* 지도학습에서 타겟 분포는 보통 실제 one-hot 벡터.
* 준지도학습(semi-supervised learning)에서는 보통 사전학습한 teacher model이 타겟 분포를 생성하고, 메인 네트웍을 학습함.
* 이 논문은 메인 네트웍의 학습 상태를 기반으로 타겟 분포 조정을 학습하면 더 나은 성능에 이르게 됨을 보임.
  * 메타-학습 알고리즘을 제안하는데, teacher가 메인 네트웍의 학습을 개선하는 방향으로 학습 데이터의 타겟 분포를 조정함.
  * 따로 떼놓은 검증 데이터(held-out validation data)로 계산한 policy gradient로 teacher를 학습함. 
* 실험은 CIFAR-10, SVHN, ImageNet 에서 state-of-the-art.

# 들어가기

* cross entropy loss : KL divergence from a target distribution over all the possible classes to the distribution predicted by a network
  * 여기서 자연스레 이어지는 질문: 이 타겟 분포는 무엇이어야 하지?
  * 지도학습의 경우, one-hot, smoothed version of one-hot(label smoothing[^1] [^2])
  * 준지도학습의 경우, pseudo label라고도 부르는 타겟 분포를 사용하는데,
  * label 있는 데이터로 학습한 날카로워지고 무뎌진(약해진) teacher 모델을 이용해서
  * label 없는 데이터를 처리한 분포를 타겟 분포로 사용한다.[^3] [^4]
  * 이 두 경우 모두 학습 전의 _사전 정보(prior)_로 디자인한 경험지식(heuristic)이라서, 학습되는 신경망의 학습 상태에 맞춰서 조정할 수 없다는 약점이 내재해 있다.

* 그래서 타겟 분포를 메타-학습할 수 있는 방법을 제안함.
  * teacher model 디자인 : 메인 모델(앞으로 student 모델이라고 부르겠음)을 학습할 때 사용할 input data에 대해서 분포를 할당함.
  * student model 학습 : student model을 검증 데이터셋으로 진행한 성능 평가를 근거로 해서, 다음번에 검증 데이터셋으로 더 나은 성능을 보일 만한 타겟 분포를 teacher가 생성함.
  * 이 방법이 pseudo label 기법과 유사해서 _Meta Pseudo Label_이라고 이름붙임.
  * MPL을 사용하면, student의 학습 상태에 따라서 타겟 분포를 조정할 수 있어서, pseudo label과 지도 학습을 사용한 분류기보다 더 나은 성능을 보여준다.

# 동기
* 세팅
  * C개의 클래스가 있고 $\Theta$로 파라미터화된 모델
  * 보통 타겟 분포 $q_* ( Y \mid X )$와 모델 분포 $p_\Theta ( Y \mid X )$ 사이의 크로스 엔트로피를 최소화함
* 완전 지도 학습에서는, 타겟 분포는 one-hot 벡터로 정의된다.
* knowledge distillation에서는 잘 학습된 더 큰 모델의 "dark knowledge"를 작은 모델로 압축한다. 더 큰 모델의 분포를 타겟 분포로 사용한다.
* 준지도 학습에서는, 제한된 label 데이터로 학습한 $q_\xi$ 모델로 unlabl 데이터의 클래스를 예측한 분포를 사용한다. 두 가지 버전이 있다. 그런데 이 둘은 일반적으로 잘 작동하지만 종종 최적의 선택이 아니라는 최근 연구가 있다.

Hard label : $q_* ( Y \mid x ) \overset{\Delta}{=} one-hot(argmax_y q_\xi ( y \mid x ))$

Soft label : $q_* ( Y \mid x ) \overset{\Delta}{=} q_\xi ( Y \mid x )$

* 타겟 분포를 약간 조정하는 휴리스틱 두 가지가 있다: label smoothing과 temperature tuning.

## Label smoothing

## Temperature tuning


# 용어
## Label Smoothing
## 준지도학습에서 pseudo label
## Pseudo Label

[Pseudo-Label](http://deeplearning.net/wp-content/uploads/2013/03/pseudo_label_final.pdf) 방법은, 사전학습된 모델로 label 없는 데이터를 처리한 예측 확률의 최대값을 해당 데이터의 타겟 클래스로 삼는 방법이다. 사전학습된 모델을 이용해서, label 데이터와 unlabel 데이터를 _동시에_ 학습한다. unlabel 데이터의 loss function은 지도학습의 loss와 동일한 형태를 가지되 regularizer 스타일로 loss function에 더하며 hyperparameter로 강도를 조절한다. 

_cluster assumption_[^5]에 따르면, 일반화를 위해서 결정 경계(decision boundary)는 저밀도 구간에 있어야 한다. 한편, _entropy regularization_[^6]에 따르면 레이블 없는 데이터가 보여주는 클래스 확률 분포의 조건부 엔트로피를 최소화할 때 클래스 사이의 밀도 분포를 낮게 만들어준다. 즉, 클래스 사이의 분포를 덜 겹치게하기 위해서 unlabel 데이터의 엔트로피를 최소화하는 것이다. Pseudo Label 논문은 pseudo label로 추가한 loss가 entropy regularization의 형태와 동일함을 보인다.

[^1]: [When Does Label Smoothing Help?](https://arxiv.org/abs/1906.02629)
[^2]: [Rethinking the Inception Architecture for Computer Vision](https://arxiv.org/abs/1512.00567)
[^3]: [Unsupervised Data Augmentation for Consistency Training](https://arxiv.org/abs/1904.12848)
[^4]: [MixMatch: A Holistic Approach to Semi-Supervised Learning](https://arxiv.org/abs/1905.02249)
[^5]: Chapelle, O., and Zien, A.  Semi-supervised classication by low density separation. AISTATS, 2005, (pp.5764).
[^6]: Yves  Grandvalet  and  Yoshua  Bengio,   Entropy  Regularization,    In: Semi-Supervised  Learning,  pages 151–168, MIT Press, 2006.