---
title: "One-Hot Encoding + Numpy로 코딩하기" ## 포스트 제목
category:
    - Python
tags:
    - basic
    - numpy
last_modified_at : 2020-02-08 17:27:00
use_math: true
toc: true
published: false
---

## Numpy로 One-Hot Encoding

one-hot encoding에 대해 설명하기 앞서 numpy로 간단하게 변환하는 코드 예시를 보이고 소개하고 시작하겠다.

```
import numpy as np
target = np.array([1,2,1,3])
# target에서 class 개수 구하기
n_classes = np.unique(target).shape[0]

one_hot_target = np.eye(n_classes)[target-1]

print(one_hot_target)

>>> array([[1., 0., 0.],
       [0., 1., 0.],
       [1., 0., 0.],
       [0., 0., 1.]])
```

## 서론

많은 기계학습 모델을 훈련시 범주형인 target값을 수치형으로 표현하여 one-hot encoding을 적용해 아래와 같이 사용한다. 

```
target = [0,1,2,1]

(one-hot encoding)

one_hot_target = [
    [1,0,0],
    [0,1,0],
    [0,0,1],
    [0,1,0]
]
```

또한, scikit-learn library에도 one-hot encoding에 대한 설명은 아래와 같이 나와 있다.

> "Encode categorical integer features using a one-hot aka one-of-K scheme."

이렇듯 얼핏보면 단순히 범주형->수치형->one-hot 으로 이어져 범주형 자료들은 항상 이렇게 표현하여 학습하여야 할 것 같이 보인다. 하지만 이는 one-hot에 대한 깊은 이해가 없어 기계적으로 외우는 것에 불과하다.

## 왜 One-Hot Encoding이 필요할까?

예를 들어 종류별 꽃 한 송이 당 가격 데이터가 아래와 같이 있다고 하자. (해바라기 가격이 같지 않은 이유는 단순하게 파는 가게가 다르다고 가정하자.)


|꽃 종류|수치형 종류|가격|
|:--:|:--:|:--:|
|장미|1|100|
|백합|2|200|
|해바라기|3|400|
|해바라기|3|500|


이제 이 데이터를 이용하여 꽃 종류별 가격을 예측하는 모델을 만든다고 한다. 이때, 모델이 내부적으로 mean square error값을 사용한다면 수치형 종류에 따른 예측 값은 아래와 같다.

1 - (1+2+3+3)/4 = 


## One-Hot Encoding의 약점

one-hot encoding으로 변환된 target 행렬은 수학적으로 보면 한 행이 `희소벡터(sparse vector)`이다. 따라서, 행렬 내에서 선택된 한 쌍의 행은 원본 데이터가 같은 값을 나타내지 않는 이상 두 벡터의 내적(inner product)값은 0이 되어 두 벡터가 이루는 각은 90도로 직교(orthogonal)를 이루게 된다. 결국 두 벡터가 전혀 관련없는 독립적인 상태임을 내포하고 있다. 즉, target 값들이 크게 유의미한 상관관계를 보일 가능성이 없거나, 설계한 모델이 target 값들의 상관관계를 무시한 독립이라 가정한 상황에는 크게 문제가 되지 않을 수 있다. 하지만, 그렇지 않은 경우는 원하는 결과가 안 나올 가능성이 있다.

더불어 one-hot encoding으로 변환된 target 행렬은 희소행렬이므로 `차원의 저주(curse of dimensionality)` 문제가 발생할 수 있다. 이는 당연한 결과로 범주가 N개였다면 target 행렬은 NxN 행렬이 된다. 보통 자연어 처리(Natural Language Processing)에서 Bag-of-Words(BoW)를 구성시에 one-hot encoding을 사용할 수 있다. 이때 사용하는 단어의 개수는 문제 크기에 따라 천차만별이겠지만 못해도 몇 만단위 단어를 사용할 수 있다. 즉, N이 10,000개 된다는 뜻이며, NxN은 기하급수적으로 늘어나게 된다. 그리하여 학습 데이터가 고차원 공간으로 투영되므로 학습에 필요한 충분한 데이터가 되지 않을 수 있다. 


<hr>

### 참고 사이트

1. [kakao brain : 단어 간 유사도 파악 방법](https://www.kakaobrain.com/blog/6)

2. [Rakshith Vasudev(Medium blog) : What is One Hot Encoding? Why And When do you have to use it?](https://medium.com/hackernoon/what-is-one-hot-encoding-why-and-when-do-you-have-to-use-it-e3c6186d008f)

3. [sokoban(Branch blog) : One Hot Encoding](https://brunch.co.kr/@sokoban/8)

