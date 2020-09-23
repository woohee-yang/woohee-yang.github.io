---
title: "[Practice] PCA(Principle Component Analysis)" ## 포스트 제목
category:       
    - Machine Learning
tags:           
    - PCA
    - coding
comments:  true
use_math : true
last_modified_at : 2020-09-23
toc: true
# published: false
---

# 1. Scikit-Learn 라이브러리를 이용한 PCA

- Scikit-Learn 라이브러리 내 `sklearn.decomposition.PCA` 가 있다.

- 공식 설명에 따르면 Scikit-Learn은 SVD를 사용하여 PCA를 하고 있고, 앞선 포스팅에서와 같이 먼저 입력데이터들을 입력데이터들의 평균으로 빼 원점으로 정렬시키고 SVD를 수행한다.

- 입력데이터 모양과 추출할 성분 개수에 따라, **full SVD**는 [LAPACK](http://netlib.org/lapack/) 선형대수 패키지 구현을 따르고, truncated SVD는 Halko et al. 2009 논문 또는 scipy.aprse.linalg ARPACK 패키지 구현을 사용하기도 한다.

- 참조 : [sklearn.decomposition.Truncated SVD](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html#sklearn.decomposition.TruncatedSVD)

---

# 2. sklearn.decomposition.PCA

## 2.1 Parameters :
- `n_components` : int, float, None or str :: 주성분 개수  
- 이외 : copy(bool) / whiten(bool) / svd_solver(str) / tol(float) / iterate_power(int) / random_state(int)


## 2.2 Methods :
- `fit_transform(X[,y])` 
    + 특징행렬을 낮은 차원의 근사행렬로 변환된 행렬 반환
    + parameters :  
        * training data : X :: array-like, shape(n_samples, n_features)  
        + ignored variable : y :: None  
    + returns : 
        * transformed data : X_new :: array-like, shape(n_samples, n_components)  

- `inverse_trainsform(X)`
    + 변환된 근사행렬 역변환된 행렬 반환  
    + parameters :  
        + new data : X :: array-like, shape(n_samples, n_features)
    + returns :  
        * X_original :: array-like, shape (n_samples, n_features)  

- `score()`
    + 모든 샘플들의 평균 로그 우도 반환 / 참조 : Pattern Recognition and Machine Learning, Bishop, 12.2.1 p.574 or [기타문서](http://www.miketipping.com/papers/met-mppca.pdf)  
    + parameters :   
        * data : X :: array  
        * ignored variable : y :: None
    + returns : 
        * Average log-likelihood of the samples under the current model : ll :: float  

- 이외 :  
    + fit() / get_covariance() / get_params() / get_precision() / set_params() / transform()  


## 2.3 Attributes :
+ `mean` :  입력 데이터의 평균 벡터, 입력 데이터 정규화(중앙에 맞춤, 원점 정렬)에 사용  
+ `components_` (array, shape(n_components, n_features)) : 계산된 주성분 행벡터, 전치시켜서 봐야 주성분들 순서대로 확인  
+ `explained_variance_ratio` (array, shape(n_components)) : 설명된 분산의 비율, 각 성분들의 고유치를 비율로 표현  
+ `explained_variance_`  (array, shape(n_components)) : 설명된 분산의 양, 주성분들의 고유치, 행렬 X의 공분산의 가장 고유치 크기 순으로  
+ `singluar_values` (n_components) : 주성분들과 순서대로 대응하는 특이치  
+ 이외 : n_components / n_features / n_samples / noise_variance

---

# 3. Examples

## 3.1 PCA 뜯어보기 : 분꽃 데이터
### 1) 주성분

- 코드 :  
  
![pca02](/assets/images/2020-09-23-pca02.png)

- 결과 :
  
![pca01](/assets/images/2020-09-23-pca01.png)

### 2) d차원으로 투영

- 코드 :  

![pca03](/assets/images/2020-09-23-pca03.png)

- 결과 :  
    + 3차원 : 원본 입력  
    ![pca04](/assets/images/2020-09-23-pca04.png)  
      
    + 2차원 : PCA  
    ![pca05](/assets/images/2020-09-23-pca05.png)   
  
    + 1차원 : PCA, 주성분 1개씩 도식화
        * 1번째 주성분 :  
        ![pca06](/assets/images/2020-09-23-pca06.png)  

        * 2번째 주성분 :  
        ![pca07](/assets/images/2020-09-23-pca07.png)  

        * 3번째 주성분 :
        ![pca08](/assets/images/2020-09-23-pca08.png)  
  

- 해석 :  
    + 입력 데이터의 3차원상 분포를 보면 클래스들이 잘 구별되어 있음을 볼 수 있다.
    + 이를 2차원으로 PCA를 통해 차원축소한 2번째 결과를 보면, 클래스들을 구별하던 분산들이 잘 보존되어 있음을 확인할 수 있다.
    + 추출된 주성분 3개를 기저로 각각 데이터 분포를 살펴본 결과, 1번째 주성분에서 클래스들을 잘 설명하는 분포를 만들고 있음을 확인할 수 있다.

---

# 참고

**1\.** [Sklearn Document : PCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html)