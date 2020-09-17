---
title: "[Pytorch] nn.Embedding()" ## 포스트 제목
category:       
    - Pytorch
tags:           
    - NLP
comments:  true
use_math : true
last_modified_at : 2020-09-16
toc: true
# published: false
---

- 파이토치에서 임베딩 벡터를 사용하는 방법:  
    **1\.** 임베딩 층(embedding layer)을 만들어 훈련 데이터로부터 처음부터 임베딩 벡터를 학습하는 방법   
    **2\.** 사전 훈련된 임베딩 벡터(pre-trained word embedding)을 사용하는 방법  

- 현 포스트에서는 방법1을 다룬다.

---

# 1. Embedding layer == Lookup Table

- 임베딩 층의 입력으로 사용하기 위해 입력 시퀀스의 각 단어들은 모두 정수 인코딩이 되어 있어야 한다. 따라서 아래와 같은 프로세스로 입력 단어들은 임베딩 된다.  

`어떤 단어 -> 단어에 부여된 고유한 정수값 -> 임베딩 층 통과 -> 밀집 벡터`  
  
- 임베딩 층은 입력 정수를 밀집 벡터(dense vector)로 사상시키고, 이 밀집 벡터는 인공 신경망의 학습 과정에서 가중치가 학습되는 것과 같은 선형결합 형태로 나타나게 훈련된다. 

- 훈련 과정에서는 단어는 모델이 풀고자하는 작업에 맞는 값으로 업데이트 된다. 이러한 밀집 벡터를 `임베딩 벡터(embedding vector)` 라 부른다.

- 정수를 임베딩 벡터로 사상시킨다는 것은 특정 단어와 사상되는 정수를 인덱스로 가지는 테이블로부터 임베딩 벡터 값을 가져오는 룩업 테이블(lookup-table)이라고 볼 수 있다. 쉽게 말해 입력된 정수의 또다른 표현으로 임베딩 벡터를 사용하고, 이 때 정수와 임베딩 벡터 간 연결고리를 함수, 여기서는 룩업 테이블로 표현한다.

- 룩업 테이블은 단어 집합의 크기만큼 행을 가지므로 모든 단어는 고유한 임베딩 벡터를 가진다.

![embedded01](/assets/images/2020-09-16-embedding01.png)

- 위 그림에서 단어 great이 정수 인코딩 된 후 테이블로부터 해당 인덱스에 위치한 4차원 임베딩 벡터를 꺼내오는 모습을 볼 수 있다.

- 그리고 단어 great은 정수 인코딩 과정에서 1,918의 정수로 인코딩 되었고 그에 따라 단어 집합 크기만큼 행을 가지는 룩업 테이블에서 인덱스 1,918번에 위치한 행을 단어 great의 임베딩 벡터로 사용한다.

- 이 임베딩 벡터는 학습 모델의 입력이 되고, 역전파 과정에서 단어 great의 임베딩 벡터 값이 학습된다.

- 파이토치는 일반적인 방법인 단어를 정수 인덱스로 바꾸고 원-핫 벡터로 한번 더 바꾸고나서 임베딩 층의 입력으로 사용하는 것이 아니라, 단어를 정수 인덱스로만 바꾼채로 임베딩 층의 입력으로 사용해도 룩업 테이블을 만들어 임베딩 벡터를 리턴한다.

- 룩업 테이블을 만드는 과정을 직접 코딩하면 아래와 같다.  

**1\.** 훈련 데이터 준비

```
train_data = 'you need to know hot to code'
word_set = set(train_data.split())
vocab = {word: i+2 for i, word in enumerate(word_set)}
vocab['<unk>'] = 0
vocab['<pad>'] = 1
```
  
**2\.** 룩업 테이블 생성  
    - 현재는 수작업으로 훈련 데이터 내 단어 수와 동일한 행 개수를 가지는 룩업 테이블 생성
    - 이 룩업 테이블을 파이토치 nn.Embedding 함수에서 단어 정수 인덱스만 보고 자동 생성

```
embedding_table = torch.FloatTensor([
                               [ 0.0,  0.0,  0.0],
                               [ 0.0,  0.0,  0.0],
                               [ 0.2,  0.9,  0.3],
                               [ 0.1,  0.5,  0.7],
                               [ 0.2,  0.1,  0.8],
                               [ 0.4,  0.1,  0.1],
                               [ 0.1,  0.8,  0.9],
                               [ 0.6,  0.1,  0.1]])
```

**3\.** 룩업 테이블 사용  

```
sample = 'you need to run'.split()
indexes = []

for word in sample:
    try:
        indexes.append(vocab[word])
    except KeyError:
        indexes.append(vocab['<unk>'])
        
indexes = torch.LongTensor(indexes)

lookup_result = embedding_table[indexes, :]
print(lookup_result)
```

---

# 2. Embedding layer 사용하기

- 이제 nn.Embedding()으로 사용하는 경우를 보자. 먼저 전처리 과정은 동일한 과정을 거친다.

- nn.Embedding() parameters :  
    - num_embeddings : 임베딩을 할 단어 개수, 단어 집합 크기
    - embedding_dim : 임베딩할 벡터의 차원, 사용자 지정 파라미터
    - padding_idx : 선택적 사용, 패딩할 위치, 토큰 인덱스 지정, 0으로 채울 위치, 그러므로 전체 단어 개수를 넘어갈 수 없음


```
import torch.nn as nn

train_data = 'you need to know hot to code'
word_set = set(train_data.split())
vocab = {word: i+2 for i, word in enumerate(word_set)}
vocab['<unk>'] = 0
vocab['<pad>'] = 1

embedding_layer = nn.Embedding(num_embeddings = len(vocab),
                              embedding_dim = 3,
                              padding_idx = 7) #7번째 행은 모두 0

print(embedding_layer.weight)
```

---

## 참고

**1\.** [Pytorch로 시작하는 딥러닝 입문, 07. 파이토치(PyTorch)의 nn.Embedding()](https://wikidocs.net/64779)









