---
title:          "[네이버 부스트캠프 AI Tech] Maximum Likelihood Estimation"
categories:       
    - MLE
tags:           
    - 구현
    - MLE
comments: true
last_modified_at : 2023-11-10 13:00:00
toc: true
math: true
---

# 최대 가능도 추정(Maximum Likelihood Estimation, MLE)은 왜 할까?
> 관찰한 데이터를 가장 잘 설명하는 모델의 파라미터를 찾기 위해서
- 현재 내가 가지고 있는 데이터를 모집단이라고 했을때 이를 가장 잘 설명해줄 수 있는 모델을 찾아야 할때 사용할 수 있다.
- 기계학습에서 유한한 개수의 데이터를 관찰해서 모집단을 정확하게 추정하는 것은 불가능하다. 따라서 모집단을 **추정** 하게 되는 것이다.
- 추정 방법 중 하나가 **최대 가능도 추정(Maximum Likelihood Estimation, MLE)**이다.
- 최대가능도는 관찰된 데이터 X에 대한 모델 파라미터 $\theta$ 들이 얼마나 가능한가? 를 계산한다.
- 이때, 각 데이터 X는 독립추출(i.i.d)가정을 하여 각각 $ x_i $ 에 대해 모델 파라미터를 계산할 수 있게 된다.
- 그리고 만들어진 가능도(우도)를 가장 최대로 하기 위한 각 모델 파라미터를 구하기 위해 미분을 사용한다.

# 미분이 불가능한 우도함수의 경우

- 이때, 미분이 불가능한 모델을 선택한다면 최대값 추정이 아닌 다른 방법을 사용해야 할 수도 있다. 최대 가능도 추정에서는 미분이 가능한 모델을 사용한다고 가정하고 시작한다.

- 그래도 미분이 불가능한 모델을 선택해야한다면 다른 추정법을 고려하거나 미분이 가능하도록 만들 수 있다. 아래와 같은 대안들을 고려하면 된다.

1) 수치적 최적화 : 우도함수 최적화를 위해 수치적 최적화 알고리즘을 사용(ex. 경사하강법 또는 유전알고리즘)

2) 단순화된 모델 선택 : 미분이 어려운 모델 대신 미분이 가능한 간단한 모델로 대체하여 추정한다. 이때, 데이터들을 자세히 표현할 수 없는 모델이 선택될 수 있으니 주의해야 한다. 반대의 경우도 있을 수 있다. (over-fitting or under-fitting 문제)

3) 모델 수정 : 원래 모델을 수정하여 미분이 가능하도록 만든다. 이는 모델의 형태를 변경하거나 특수한 경우에 적용 가능한 변환을 고려해 본다.

4) 대체 추정 방법 사용 : 모멘트 방법, 베이즈 추정, EM알고리즘 등 다른 추정 방법 사용을 고려한다.

---

# 최대 가능도 추정에 왜 로그를 사용할까?
- 데이터 숫자 단위를 줄여준다.
- 가능도 함수는 0 ~ 1 사이 범위의 값을 가지는 확률을 독립추출 가정에 의해 모두 곱하기 때문에 값이 너무 작아진다.
- 이는 컴퓨터로 다룰 수 있는 부동소수점의 범위를 쉽게 넘길 수 있으므로 로그를 사용하여 계산 단위를 올려준다.
- 또한, 곱셈이 덧셈으로 치환되어 셈에 용이하다.

---

# 최대 가능도 추정 수식 전개
- 확률변수 : $ X (x_n, x_{n-1}, ... , x_1)$
- 모델은 **정규분포** 로 가정한다. 그러면 파라미터로 평균인 $ \mu $ 와 표준편차인 $ \sigma $ 를 갖게된다.
- 모델 파라미터(모수) : $ \theta = (\mu, \sigma^2) $
- 이를 $ x $ 에 대한 정규분포로 바꿔주면 아래와 같다.

$$ f_{X_i}(x_i;\theta) = \frac{1}{\sigma\sqrt{2\pi}} \exp\left(-\frac{(x_i-\mu)^2}{2\sigma^2}\right) , \,\,i=1, \cdots, n $$

- 이제 독립추출을 가정으로한 전체 데이터에 대한 가능도함수(likelihood)를 표현하면 아래와 같다.

$$ P(x|\theta) = \prod_{i=1}^{n}f_{X_i}(x_i;\theta)= \prod_{i=1}^{n}\frac{1}{\sigma\sqrt{2\pi}} \exp \left( -\frac{(x_i-\mu)^2}{2\sigma^2} \right) $$

- 양변에 자연로그를 취해 전체를 풀어준다.

$$L(\theta|x) = \ln P(x|\theta) = \sum_{i=1}^{n}\log\frac{1}{\sigma\sqrt{2\pi}}\exp\left(-\frac{(x_i-\mu)^2}{2\sigma^2}\right) $$

$$ L(\theta|x) = \ln P(x|\theta) $$

$$ = \sum_{i=1}^n (\ln \frac{1}{\sqrt{2\pi\sigma^2}} - \frac{(x_i - \mu)^2}{2\sigma^2})$$

$$ = -\frac{n}{2}\ln(2\pi\sigma^2) - \sum_{i=1}^n\frac{(x_i - \mu)^2}{2\sigma^2}$$

- 전개된 식에서 각 모델 파라미터에 대한 편미분을 하여 최대값을 추정한다. 먼저 평균에 대한 미분이다.

$$\frac{\partial L(\theta|x)}{\partial \mu}
= -\frac{1}{2\sigma^2}\sum_{i=1}^{n}\frac{\partial}{\partial \mu}\left(x_i^2-2x_i\mu+\mu^2\right) $$

$$ = -\frac{1}{2\sigma^2}\sum_{i=1}^{n}(-2x_i + 2\mu)$$

$$ = \frac{ - n\mu}{\sigma^2} + \frac{\sum_{i=1}^{n}x_i}{\sigma^2} $$

$$ = \frac{ - n\mu}{\sigma^2} + \frac{\sum_{i=1}^{n}x_i}{\sigma^2} = 0$$

$$ =  - n\mu  + \sum_{i=1}^nx_i= 0$$

$$ \therefore \hat{\mu} = \frac{\sum_{i=1}^n x_i}{n}$$


- 다음으로 표준편차에 대한 미분이다.

$$ \frac{\partial L(\theta|x)}{\partial \sigma} = \frac{\partial}{\partial \sigma} [-\frac{n}{2}ln(2\pi\sigma^2) - \sum_{i=1}^n\frac{(x_i - \mu)^2}{2\sigma^2}] = 0$$

$$ = - \frac{n}{\sigma} + \frac{\partial}{\partial\sigma}[\frac{1}{2\sigma^2} (\sum_{i=1}^n x_i - \mu)^2] $$

$$ = - \frac{n}{\sigma} - \frac{1}{\sigma^3} (\sum_{i=1}^n x_i - \mu)^2 $$

$$ = - n\sigma^2 - (\sum_{i=1}^n x_i - n\mu) $$

$$ \therefore \hat{\sigma}^2 = \frac{1}{n}(\sum_{i=1}^n x_i - \mu)^2 = \frac{(n-1)s^2}{n} $$

$$(\because s^2 = \frac{1}{n-1} (\sum_{i=1}^n x_i - \mu)^2 )$$

---

# 코드를 이용한 최대 가능도 추정

- 주요 코드를 살펴보면 다음과 같다.

```
def likelihood_mu(mu):
    return sp.stats.norm(loc=mu).pdf(0)
```

- 위 함수는 추정치 mu를 입력으로 받아 mu에 대한 가능도를 계산한다.

```
mus = np.linspace(-5, 5, 1000)
likelihood_mu = [likelihood_mu(m) for m in mus]

plt.plot(mus, likelihood_mu)
plt.title("Likelihood $L(\mu, \sigma^2=1; x=0)$")
plt.xlabel("$\mu$")
plt.show()
```

![mu에 대한 가능도함수](/assets/images/custom/MLE01.png)

- 표준편차도 동일한 과정으로 구할 수 있다.

---

# 베르누이 분포(Bernoulli Distribution)에 대한 최대 가능도 추정

- 위에서는 정규분포에 대한 최대 가능도 추정 식 전개와 코드를 살펴보았다.

- 모델은 어떤 것이든 사용될 수 있으므로, 이번엔 이산분포의 기본인 베르누이 분포를 이용한 최대 가능도 추정을 전개해 보겠다.

![MLE about Bernoulli Dist](/assets/images/custom/Bernoulli_MLE.png)


