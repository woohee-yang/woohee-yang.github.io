---
title: "개발 환경 총 정리" ## 포스트 제목
category:       
    - Development Environment
tag:
    - anaconda
    - vscode
    - jupyter notebook
last_modified_at : 2020-04-26
use_math : true
toc: true
comments:       true ## 댓글 기능
---
# 1. 현재 사용하는 개발환경 정리
- 다시 환경 구축시 순서대로 하면 됨

## 2. Anaconda
- 파이썬 패키지 의존성 관리 및 가상환경 생성 편리를 위해서 사용
- Anaconda prompt에서 첨부된 명령어로 가상환경 관리

1. conda update
```
conda update -n <env> conda
```
    - 지정된 환경에서 conda 업데이트
    - 새로운 환경 생성시나 오래된 환경 다시 사용시 먼저 해주면 됨

2. 모든 패키지 업데이트 (패키지 의존성 주의!)
```
conda update --all
```
    - 현재 가상환경에서 모든 패키지 업데이트
    - 의존성 관리는 아나콘다가 알아서 해줌
    - update 명령어로 특정 패키지만 업데이트시, 다른 패키지와 의존성 고려할 것
    - 환경별로 구축한 환경 특성상 특정버전 패키지만 사용해야 한다면 업데이트는 조심해서 사용할 것

3. 새로운 가상환경 만들기
```
conda create --name <name> <option>
```
    - `<option>` : 설치할 언어 및 패키지 특정 버전 명시 가능

4. jupyter notebook 설치
```
pip install ipykernel
```

5. jupyter notebook nbextension
```
conda install -c conda-forge jupyter_contrib_nbextensions
jupyter contrib nbextension install --user
```
    - jupyter notebook 확장기능인 nbextension 사용
    - 브라우저에서 jupyter notebook을 실행시 사용가능

6. jupyter notebook kernel 추가
```
python -m ipykernel install --user --name <VirtualEnv> --display-name "KernelName"
```

7. jupyter notebook에서 kernel 추가됐는지 확인
```
jupyter notebook --ip=[ipAddress]
```

## 3. VSCode
1. VSCode 다운로드 : <https://code.visualstudio.com/>

2. 사용 패키지 설치
    - 

## 4. Jupyter Notebook



  {% if site.disqus-shortname %}{% include disqus.html %}{% endif %}