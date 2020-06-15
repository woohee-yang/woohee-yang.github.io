---
title: "Dimensionality Reduction" ## 포스트 제목
categories:       
    - Basic
tags:           
    - dimensionality
    - reduction
    - Hands on ML - 8장
comments:  true ## 댓글 기능
use_math : true
last_modified_at : 2020-01-29 01:21:00
published: false
---

&nbsp;&nbsp;&nbsp;&nbsp; 본 포스트는 [Hands-On Machine Learning with Scikit-Learn & TensorFlow] (저자 : Aurelien Geron / 부제: 핸즈온 머신러닝) 8장을 참고한 자료이다.

- 차원 축소를 위한 두 가지 주요 접근법인 투영(projection)과 매니폴드 학습(manifold learning)에 대해 알아본다.

## 투영 : Projection



<!-- - 실질적으로 고차원 훈련 데이터들은 모든 차원에 걸쳐 균일하게 퍼져 있지 않는 경우가 대부분이다. 이러한 경우, 몇 가지 서로 강하게 연관있는 특성들로만 이 데이터들을 표현할 수 있다. 예를 들어 아래 그래프에서 데이터들은 3차원 상에 있지만 2차원으로 충분히 표현 가능하게 보이는 것과 같다.

(그래프1)

- 따라서 고차원 공간내 모든 샘플들을 저차원 부분공간(subspace)에서 표현이 가능하다. 다음과 같이 새로운 축, 특성으로 이를 표현하게 된다.

(그래프2)

- 하지만, 투영이 언제나 최선의 방법이 아니다. 대표적으로 *스위스 롤(swiss roll)* 데이터셋처럼 부분 공간이 뒤틀리거나 휘어있을 수 있다. 이럴 때는 데이터를 펼쳐서 뚜렷한 경계를 얻을 수도 있는데 이 방법이 매니폴드 학습이다. -->

## 매니폴드 학습 : Manifold learning

- 간단히 말해 매니폴드는 고차원 공간에서 휘어지거나 뒤틀린 모양이다. 


실험용 추가 ????
