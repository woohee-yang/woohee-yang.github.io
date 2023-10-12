---
title: "[Pytorch] Tensor Operation 정리" ## 포스트 제목
category:       
    - Pytorch
last_modified_at : 2020-06-01
use_math : true
toc: true
---

# Pytorch Tensor 연산 정리 모음

1. torch.mm(mat1, mat2)
- 이 함수는 브로드캐스팅 없이 입력 텐서들의 곱을 반환한다. == `일반적인 행렬곱`
ex) mat1.shape=(n x m), mat2.shape=(m x p) => result = (n x p)

- 이때 반드시 `두 텐서의 dtype이 일치`해야 한다.

![op01](/assets/images/custom/2020-06-01-torch_op01.PNG)
