---
title: "[Pratice] LSTM Attention" ## 포스트 제목
category:       
    - Deep Learning
tags:           
    - lstm
    - attention
    - coding
comments:  true
use_math : true
last_modified_at : 2020-09-14
toc: true
# published: false
---

# 1. 데이터 파일 로딩

- 데이터 : 영어-프랑스 번역

**1\.** 단어 정수 인덱싱, 카운팅을 위한 클래스 정의  

  
**CODE :**  

```
SOS_token = 0
EOS_token = 1

class Lang:
    def __init__(self, name):
        self.name = name
        self.word2index = {}
        self.word2count = {}
        self.index2word = {0:"SOS", 1:"EOS"}
        self.n_words = 2
    
    def addSentence(self, sentence):
        for word in sentence.split(' '):
            self.addWord(word)
        
    def addWord(self, word):
        if word not in self.word2index:
            self.word2index[word] = self.n_words
            self.word2count[word] = 1
            self.index2word[self.n_words] = word
            self.n_words += 1
        else:
            self.word2count[word] += 1
```
  
**2\.** unicode to ASCII code 변환  

**3\.** 문자열 다듬기 : 소문자 변환, 문자 아닌 문자 제거  

**4\.** 파일 읽기 함수 정의 : 줄 별 읽기, 문자열 다듬기, 영-프 단어쌍 저장  

**5\.** 문자열 최대 길이 지정, 문장 부호 대체 처리  

**6\.** 읽기  


---

# 2. Seq2Seq 모델

- 단일 RNN : 모든 입력에 해당하는 출력 예측  
    <-> seq2seq 모델 : 시퀀스 길이와 순서를 자유롭게 하므로 두 언어 사이의 번역에 이상적

- seq2seq 모델의 인코더(Encoder)는 하나의 벡터를 생성  
    이상적인 경우, 입력 시퀀스의 "의미"를 문장의 N차원 공간에 있는 단일 지점인 단일 벡터로 인코딩한다.  
    == 인코더가 입력 문장 정보를 N차 공간(특징 공간)벡터로 요약(사상)

---

# 3. 인코더(Encoder)

- seq2seq 네트워크의 인코더는 입력 문자의 모든 단어에 대해 어떤 값을 출력하는 RNN이다. `인코더는 모든 입력 단어에 대해 벡터와 은닉 상태를 출력하고, 다음 입력 단어를 위해 그 은닉 상태를 사용한다.`

- NLP task이므로 임베딩(embedding)된 단어 시퀀스들을 GRU 입력으로 사용한다.

![att_pro01](/assets/images/2020-09-14-att_pro01.png)    

  
**CODE :**  

```
class EncoderRNN(nn.Module):
    def __init__(self, input_size, hidden_size):
        super(EncoderRNN, self).__init__()
        self.hidden_size = hidden_size
        
        self.embedding = nn.Embedding(input_size, hidden_size)
        self.gru = nn.GRU(hidden_size, hidden_size)
    
    def forward(self, input, hidden):
        embedded = self.embedding(input).view(1,1,-1)
        output = embedded
        output, hidden = self.gru(output, hidden)
        return output, hidden
    
    def initHidden(self):
        return torch.zeros(1, 1, self.hidden_size, device=device)
```

---

# 4. 디코더(Decoder)

- 디코더는 인코더 출력 벡터를 받아 번역을 생성하기 위한 단어 시퀀스를 출력한다.

- 가장 간단한 seq2seq 디코더는 인코더의 마지막 출력만을 이용한다. 이 마지막 출력은 전체 시퀀스에서 문맥을 압축한 정보이므로 *컨텍스트 벡터(context vector)*로 불린다. 이 컨텍스트 벡터가 디코더의 초기 은닉 상태로 사용된다.

- 다시 말해, seq2seq 네트워크의 인코더는 마지막 셀에서 모든 입력 단어들을 한 은닉 상태 벡터, 컨텍스트 벡터로 문맥을 표현한다. 이를 디코더에서 첫 번째 셀의 은닉 상태로 입력 받아 사용한다.

![att_pro05](/assets/images/2020-09-14-att_pro05.png)  

- 이 때, 위 그림에서 보듯 디코더의 입력인 번역을 원하는 새로운 문장 또한 인코더에서와 같이 단어 임베딩 레이어를 통과해 임베딩 벡터로 표현되야 한다.

- 디코딩의 매 단계에서 디코더에게 입력 토큰과 은닉 상태가 주어진다. 

- 초기 입력 토큰은 문자열-시작(start-of-string), <SOS>토큰이고, 첫 은닉 상태는 컨텍스트 벡터이다.

![att_pro02](/assets/images/2020-09-14-att_pro02.png)  

  
**CODE :**  
  
```
class DecoderRNN(nn.Module):
    def __init__(self, hidden_size, output_size):
        super(DecoderRNN, self).__init__()
        self.hidden_size = hidden_size
        
        self.embedding = nn.Embedding(output_size, hidden_size)
        self.gru = nn.GRU(hidden_size, hidden_size)
        self.out = nn.Linear(hidden_size, output_size)
        self.softmax = nn.LogSoftmax(dim=1)
        
    def forward(self, input, hidden):
        output = self.embedding(input).view(1,1,-1)
        output = F.relu(output)
        output, hidden = self.gru(output, hidden)
        output = self.softmax(self.out(output[0]))
        return output, hidden

    def initHidden(self):
        return torch.zeros(1, 1, self.hidden_size, device=device)
```

---

# 5. 어텐션 디코더(Attention Decoder)

- 컨텍스트 벡터만 인코더와 디코더 사이로 전달 된다면, 단일 벡터가 전체 문장을 인코딩 해야하므로 정보 손실이 발생한다.

- 어텐션은 디코더 네트워크가 자기 출력의 모든 단계에서 인코더 출력의 현 출력과 가장 적합한 부분에 "집중"할 수 있게 한다.  
    - 어텐션 스코어 계산 : 스코어 조합을 만들기 위해 각 시점에서 인코더 출력 벡터와 어텐션 분포가 곱해진다. 따라서 어텐션은 입력 시퀀스의 특정 부분에 관한 정보를 포함하고, 디코더가 알맞은 출력 단어를 선택하는 것을 도와준다.  
    - 어텐션 스코어 계산은 디코더의 입력 및 은닉 상태를 입력으로 사용하는 다른 feed-forward 계층인 `attn`으로 수행된다.  
    - 이 때, 현 예제에서는 어텐션 스코어를 구하기 위해 인코더의 모든 은닉상태, 컨텍스트 벡터와 각 시점에서 디코더의 은닉상태 벡터 유사도를 feed-forward 계층으로 구했으나, dot, scaled dot, concat, location-base 등 다른 방법으로도 구할 수 있다. 이에 따라 어텐션 종류가 달라진다. [(참조: 5.다양한 종류의 어텐션)](https://woohee-yang.github.io/deep%20learning/2020/09/14/Attention/)

![att_pro03](/assets/images/2020-09-14-att_pro03.png)
  
- 학습데이터에는 모든 크기의 문장이 있기 때문에 이 계층을 실제로 만들고 학습시키려면 적용할 수 있는 최대 문장 길이(인코더 출력을 위한 입력 길이)를 선택해야 한다. 최대 길이의 문장은 모든 어텐션 스코어를 사용하지만 더 짧은 문장은 처음 몇개만 사용한다.  
  
![att_pro04](/assets/images/2020-09-14-att_pro04.png)  
  
**CODE :**  
  
```
class AttnDecoderRNN(nn.Module):
    def __init__(self, hidden_size, output_size, dropout_p=0.1, max_length=MAX_LENGTH):
        super(AttnDecoderRNN, self).__init__()
        self.hidden_size = hidden_size
        self.output_size = output_size
        self.dropout_p = dropout_p
        self.max_length = max_length
        
        self.embedding = nn.Embedding(self.output_size, self.hidden_size)
        self.attn = nn.Linear(self.hidden_size * 2, self.max_length)
        self.attn_combine = nn.Linear(self.hidden_size * 2, self.hidden_size)
        self.dropout = nn.Dropout(self.dropout_p)
        self.gru = nn.GRU(self.hidden_size, self.hidden_size)
        self.out = nn.Linear(self.hidden_size, self.output_size)
        
    def forward(self, input, hidden, encoder_outputs):
        # 1. 디코더 입력 문장 임베딩
        embedded = self.embedding(input).view(1,1,-1)
        embedded = self.dropout(embedded)
        
        # 2. 어텐션 가중치 분포 구하기
        ## 2.1 어텐션 스코어 = W * cat(인코더 모든 은닉(input), 디코더 은닉(hidden) + b
        ## 2.2 어텐션 가중치 분포 = softmax(어텐션 스코어)
        attn_weights = F.softmax(
            self.attn(torch.cat((embedded[0], hidden[0]), 1)), dim=1)
        
        # 3. 어텐션 가중치 분포와 인코더 출력들을 곱함 (torch.bmm : 텐서 결합)
        ## attn_applied : 어텐션 값
        attn_applied = torch.bmm(attn_weights.unsqueeze(0),
                                encoder_outputs.unsqueeze(0))
        
        # 4. 어텐션 값과 디코더 입력 결합
        output = torch.cat((embedded[0], attn_applied[0]), 1)
        
        # 5. 결합된 어텐션 값과 디코더 입력을 feed-forward 계층을 통해 어텐션 출력 구하기
        output = self.attn_combine(output).unsqueeze(0)
        
        # 6. ReLU
        output = F.relu(output)
        
        # 7. 어텐션 입력 GRU
        output, hidden = self.gru(output, hidden)
        
        # 8. 최종 출력 스코어 계산
        output = F.log_softmax(self.out(output[0]), dim=1)
        
        return output, hidden, attn_weights
    
    def initHidden(self):
        return torch.zeros(1, 1, self.hidden_size, device=device)
```
  
- `이전 Attention Mechanism 포스트에서 내적 어텐션과 가장 다른 점:`  
    **1\.** 과정 2: 어텐션 분포 구하기 -> 어텐션 내적과 달리, feed-forward 계층으로 어텐션 스코어 구함  
    **2\.** 과정 3: 어텐션 값 구하기 -> 어텐션 값을 인코더 은닉 상태와 곱하지 않고, 어텐션 값을 인코더 출력 벡터와 곱함(torch.bmm)  
    **3\.** 과정 4: 어텐션 값과 디코더 t시점 은닉 상태가 아니라, 어텐션 값과 현재 디코더 입력과 결합  
  
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

**1\.** [기초부터 시작하는 NLP: SEQUENCE TO SEQUENCE 네트워크와 ATTENTION을 이용한 번역](https://tutorials.pytorch.kr/intermediate/seq2seq_translation_tutorial.html#nlp-sequence-to-sequence-attention)  

**2\.** [Pytorch로 시작하는 딥러닝 입문, 14-01. 시퀀스투시퀀스(Sequence-to-Sequence, seq2seq)](https://wikidocs.net/65154)
    




