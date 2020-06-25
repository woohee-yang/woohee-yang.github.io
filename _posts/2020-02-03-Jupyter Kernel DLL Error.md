---
title: "Error Report : Jupyter Notebook Kernel DLL Error" ## 포스트 제목
category:
    - Development Environment
tags:
    - jupyter notebook
    - kernel
    - dll
last_modified_at : 2020-02-03 12:40:00
---

## Jupyter Notebook에서 만난 커널 에러 리포트

- $path$/Andaconda3/env/$env_name$/Library/bin/pythoncom37.dll을 찾지 못하여 kernel을 실행할 수 없는 에러 발생

- python interpreter는 vscode에서 실행했을시 문제 없이 동작했으나 jupyter notebook 환경에서만 작동불가하여 jupyter문제라고 판단

- pytorch 설치한 커널을 못 찾음


## 해결책

**1\.** 처음에는 jupyter kernel중 pytorch 커널이 같은 이름으로 중복 등록되어 kernel을 잘못 선택하는 에러를 발견하여 kernel list 정리

관련 명령어 :

```
jupyter kernelspec list : 주피터에 등록된 커널 리스트 출력
jupyter kernelspce uninstall <kernel_name> : 해당 커널 삭제
python -m ipykernel install --user --name <kernel> --display-name <kernel_name> : 해당 커널 등록 및 지정된 이름 사용
```

**2\.** dll이 없어진것도 아니므로 jupyter가 경로를 못 찾는다 판단하여 jupyter를 업데이트 

[jupyter kernel dll error](https://stackoverflow.com/questions/46353215/import-error-dll-load-failed-in-jupyter-notebook-but-working-in-py-file)

- 위의 답변 중 전체 패키지 업데이트를 진행하라는 답변을 채택

- opencv-python-contrib 패키지가 numpy를 특정 버전에서만 


**3\.** ipykernel 개념 모르쥬? kernel이 뭘 의미하는지 대략적으로 이제 파악 -> 실행할 수 있는 환경 자체를 말하는듯

이게 새로 만든 환경을 기반으로 만들어져야하는데 그걸 무시하고 계속 base로 만들었으니.......^^

