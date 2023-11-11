---
title: "[Pytorch] Permute vs View" ## 포스트 제목
category:       
    - Pytorch
last_modified_at : 2020-05-29
use_math : true
toc: true
---

tensor.view() : tensor 형태를 원하는 모양으로 바꿔준다.
numpy reshpae와 동일

tensor.permute() : tensor 차원 순서를 원하는 순서로 변경해준다.
numpy transpose와 동일

```
import torch
import numpy as np

ten = torch.Tensor([[1, 2], [3, 4], [5, 6]])
num = np.array([[1, 2], [3, 4], [5, 6]])

print('-'*30)
print('Origin')
print('-'*30)

print('torch.tensor : \n')
print(ten)
print(ten.shape)

print()

print('numpy array : \n')
print(num)
print(num.shape)
print()

print('-'*30)
print('Permute')
print('-'*30)

print('torch.tensor.permute : \n')
pten = ten.permute(1, 0)
print(pten)
print(pten.shape)

print()

print('np.array.transpose : \n')
pnum = num.transpose(1, 0)
print(pnum)
print(pnum.shape)
print()

print('-'*30)
print('View')
print('-'*30)

print('torch.tensor.view : \n')
vten = ten.view(-1, 1)
print(vten)
print(vten.shape)

print()

print('np.array.reshape : \n')
vnum = num.reshape(-1, 1)
print(vnum)
print(vnum.shape)
print()
```

Results :
[results](/assets/images/2020-05-29-view_permute.PNG)

