---
title: "[Pratice] LSTM Attention" ## 포스트 제목
category:       
    - Practice
tags:           
    - lstm
    - attention
comments:  true
use_math : true
last_modified_at : 2020-09-14
toc: true
# published: false
---

# 1. 데이터 파일 로딩

- 데이터 : 영어-프랑스 번역

---

# 2. Seq2Seq 모델

- 단일 RNN : 모든 입력에 해당하는 출력 예측  
    <-> seq2seq 모델 : 시퀀스 길이와 순서를 자유롭게 하므로 두 언어 사이의 번역에 이상적

- seq2seq 모델의 인코더(Encoder)는 하나의 벡터를 생성  
    이상적인 경우, 입력 시퀀스의 "의미"를 문장의 N차원 공간에 있는 단일 지점인 단일 벡터로 인코딩한다.  
    == 인코더가 입력 문장 정보를 N차 공간(특징 공간)벡터로 요약(사상)  
    == Embedding

---

# 3. 인코더(Encoder)

- seq2seq 네트워크의 인코더는 입력 문자의 모든 단어에 대해 어떤 값을 출력하는 RNN이다. 인코더는 모든 입력 단어에 대해 벡터와 은닉 상태를 출력하고 다음 입력 단어를 위해 그 은닉 상태를 사용한다.

![att_pro01](/assets/images/2020-09-14-att_pro01.png)

---

# 4. 디코더(Decoder)

- 디코더는 인코더 출력 벡터를 받아 번역을 생성하기 위한 단어 시퀀스를 출력한다.

- 가장 간단한 seq2seq 디코더는 인코더의 마지막 출력만을 이용한다. 이 마지막 출력은 전체 시퀀스에서 문맥을 압축한 정보이므로 *컨텍스트 벡터(context vector)*로 불린다. 이 컨텍스트 벡터가 디코더의 초기 은닉 상태로 사용된다.

- 디코딩의 매 단계에서 디코더에게 입력 토큰과 은닉 상태가 주어진다. 

- 초기 입력 토큰은 문자열-시작(start-of-string), <SOS>토큰이고, 첫 은닉 상태는 컨텍스트 벡터이다.

![att_pro02](/assets/images/2020-09-14-att_pro02.png)

---

# 5. 어텐션 디코더(Attention Decoder)

- 컨텍스트 벡터만 인코더와 디코더 사이로 전달 된다면, 단일 벡터가 전체 문장을 인코딩 해야하므로 정보 손실이 발생한다.

- 어텐션은 디코더 네트워크가 자기 출력의 모든 단계에서 인코더 출력의 현 출력과 가장 적합한 부분에 "집중"할 수 있게 한다.  
    -> 어텐션 스코어 계산 : 스코어 조합을 만들기 위해 인코더 출력 벡터와 곱해진다. 따라서 어텐션은 입력 시퀀스의 특정 부분에 관한 정보를 포함하고, 디코더가 알맞은 출력 단어를 선택하는 것을 도와준다.

![att_pro03](/assets/images/2020-09-14-att_pro03.png)

- 어텐션 스코어 계산은 디코더의 입력 및 은닉 상태를 입력으로 사용하는 다른 feed-forward 계층인 `attn`으로 수행된다.

- 학습데이터에는 모든 크기의 문장이 있기 때문에 이 계층을 실제로 만들고 학습시키려면 적용할 수 있는 최대 문장 길이(인코더 출력을 위한 입력 길이)를 선택해야 한다. 최대 길이의 문장은 모든 어텐션 스코어를 사용하지만 더 짧은 문장은 처음 몇개만 사용한다.

![att_pro04](/assets/images/2020-09-14-att_pro04.png)

*There are other forms of attention that work around the length limitation by using a relative position approach. Read about “local attention” in Effective Approaches to Attention-based Neural Machine Translation.*

---

# 6. 학습

- 학습을 위해 인코더에 입력 문장을 넣고 모든 출력과 최신 은닉 상태를 추적한다. 그런 다음 디코더에 첫 번째 입력으로 <SOS> 토큰과 인코더의 마지막 은닉 상태를 사용한다.

- "Teacher forcing"은 다음 입력으로 디코더의 예측을 사용하는 대신 실제 목표 출력을 다음 입력으로 사용하는 컨셉이다.

- "Teacher forcing"을 사용하면 수렴이 빨라지지만, **학습된 네크워크가 잘못 사용될 때 불안정성을 보인다.**

- Pytorch의 autograd를 사용하면 if문으로 이 기법을 사용할지 안할지 선택할 수 있고, teacher_forcing_ratio로 조절할 수 있다.

- 전체 학습 과정:  
    **1\.** 타이머 시작  
    **2\.** optimizers와 criterion 초기화  
    **3\.** 학습 쌍 세트 생성  
    **4\.** 도식화를 위한 빈 손실 배열 시작  
    **5\.** train 반복  

---

# 7. 평가

- 평가는 대부분 학습과 동일하지만, 목표가 없으므로 각 단계마다 디코더의 예측을 되돌려 전달한다. 단어를 예측할 때마다 그 단어를 출력 문자열에 추가한다. 만약 EOS 토큰을 예측하면 거기에서 멈춘다. 

- 나중에 도식화를 위해서 디코더의 어텐션 출력을 저장한다.

---

8. Attention 시각화

- Attention 출력을 행렬로 표현하려면 `plt.matshow(attentions)`로 저장되었던 모든 attention값을 출력하면 된다.

---

## 참고

1. [기초부터 시작하는 NLP: SEQUENCE TO SEQUENCE 네트워크와 ATTENTION을 이용한 번역](https://tutorials.pytorch.kr/intermediate/seq2seq_translation_tutorial.html#nlp-sequence-to-sequence-attention)
    




