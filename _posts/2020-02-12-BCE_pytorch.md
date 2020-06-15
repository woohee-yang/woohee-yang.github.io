---
title: "[Pytorch] Binary Cross Entropy" ## 포스트 제목
tags:
    - I3D
    - temporal reduction
    - research log
    - Pytorch
last_modified_at : 2020-02-12 00:30:00
use_math: true
toc: true
# published: false
---

```
label = np.zeros((num_classes,num_frames), np.float32)

        fps = num_frames/data[vid]['duration']
        for ann in data[vid]['actions']:
            for fr in range(0,num_frames,1):
                if fr/fps > ann[1] and fr/fps < ann[2]:
                    label[ann[0], fr] = 1 # binary classification
        dataset.append((vid, label, data[vid]['duration'], num_frames))
```

- 본 코드에서 BCE를 쓴 이유는 Charades 데이터셋에 있는 한 동영상이 multi action을 포함하고 있기 때문에 어떤 프레임에서 어떤 행동이 나타나고 있는지 표현하기 위해서 label을 binary로 표현했기 때문이다. 

- 따라서 위와 같이 label이 프레임별로 현재 보고 있는 action에 적합한지 아닌지를 표시한다. 아주 쉽게 말하면 n_classes x num_frames 크기를 가진 행렬 원소로 0이면 현재 클래스(ann[0])에 속하지 않음, 1이면 현재 클래스에 속함을 표시 

- 현재의 경우에는 한 동영상이 한 클래스를 나타내고 있다고 가정하므로 프레임별로 binary 형식 label을 사용할 것이 아니라 동영상별로 integer 형식 label을 사용하여 cross entropy를 사용하는 것이 맞음  