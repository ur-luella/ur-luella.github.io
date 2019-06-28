---
layout: post
title: "Overfitting 문제에 대한 고민"
excerpt: "딥러닝 모델링 중 overfitting 문제가 발생하였을 때 해결하기 위한 방법을 고민해보았습니다."
categories: [blog]
tags: [overfitting]
author: Luella
comments: true
---


의료 데이터를 이용하여 타겟을 조기 예측하는 학위논문 연구 중 데이터의 overfitting 문제가 발생하여 어떻게 해결을 해야할지 고민하면서 공부했던 내용들을 정리해보려 합니다.

## 데이터의 수

데이터의 수가 너무 적어서 문제가 되는걸까? treat는 약 300개의 환자 수와 comparator는 4배수로 Propensity score matching(PSM)을 사용하여 추출하였다. 이를 해결하기 위해 Cross-validation 방법을 사용한다.

## treat/comparator의 imbalance

다른 데이터의 특성이기도 하지만 대부분 의료데이터의 경우, treat와 comparator는 imbalance하다. 하나의 타겟(질병)을 가질 경우 나머지 데이터는 모두 comparator의 후보들이 된다. 이 중에서 PSM을 이용하여 treat와 비슷한 환자군을 N배만큼 뽑아 comparator로 추출된다. 최종으로 나온 treat와 comparator의 비율은 약 1:4이다. 

2018년 01월 28일에 진행되었던 1st 함께하는 딥러닝 컨퍼런스에서 김세진님의 MRI를 이용한 치매 질환 예측 세미나에서 over sampling보다 data balancing이 효율적이라는 정보를 얻어 적용해보려 한다.

## dropout

overfitting이 생기지 않게 과하게 학습된 network를 keep probability만큼만 유지하게 설정해준다. 보통 keep probability는 50에서 70%으로 준다.

## xavier_initializer

weight의 초기값을 random이나 0으로 초기화하였으나 이 경우 시작점이 너무 작거나 컸을 때 성능이 좋게 나오지 않는다. 이를 해결하기 위해  Xavier initializer를 통해 초기화하는 것이다. xavier는 초깃값의 표준편차가 1/sqrt(n)이 되도록 설정하는 것이다. (n은 앞 layer의 노드 수)

## Weight decay

가중치 매개변수의 값이 커서 발생할 경우 사용한다. 보통 overfitting은 전자일 때 발생하는 경우가 많다. 모든 가중치 각각의 손실 함수에 L2 norm에 따른 값을 더해준다.

## Class별 loss weight

대부분의 데이터가 그렇듯이 class마다 데이터의 수가 현저히 다르다. 나 같은 경우 label이 1일 때의 데이터와 label이 0일 때의 데이터의 비율이 10:1이다. 기계가 맞추기에 label이 1일 때를 고르는 게 0일 때를 고르는 것보다 맞을 확률이 높기 때문에 잘못된 학습을 할 가능성이 크다. 그래서 이 문제점을 해결하기 위해 loss에 class별로 weight를 준다. 데이터의 수가 많은 쪽에 loss weight를 작게 줘서 loss 비율을 맞춰서 넣는 것이다. 이 방법을 사용했을 때의 validation loss가 엄청 뛰는 것을 확인했다.. 뭐가 문제인걸까

## Batch regularization

미니 배치를 단위로 정규화한다. 구체적으로 미니 배치 데이터의 분포가 평균이 0, 분산이 1이 되도록 하는 것이다.
