---
toc: false
layout: post
comments: true
categories: [fastpages]
hide: true
title: <Meta Pseudo Label> 읽기
---

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
  * 지도학습의 경우, one-hot, smoothed version of one-hot(label smoothing[^1][^2])
  * 준지도학습의 경우, pseudo label이라고 부르는 타겟 분포를 부르는데,
  * label 있는 데이터로 학습한 날카로워지고 무뎌진(약해진) teacher 모델을 이용해서
  * label 없는 데이터를 처리한 분포를 타겟 분포로 사용한다.[^3][^4]
  * 이 두 경우 모두 학습 전의 _사전 정보(prior)_로 디자인한 경험지식(heuristic)이라서, 학습되는 신경망의 학습 상태에 맞춰서 조정할 수 없다는 약점이 내재해 있다.


# 참고

* https://arxiv.org/abs/2003.10580

[^1]: When Does Label Smoothing Help? https://arxiv.org/abs/1906.02629
[^2]: Rethinking the Inception Architecture for Computer Vision https://arxiv.org/abs/1512.00567
[^3]: Unsupervised Data Augmentation for Consistency Training https://arxiv.org/abs/1904.12848
[^4]: MixMatch: A Holistic Approach to Semi-Supervised Learning https://arxiv.org/abs/1905.02249
