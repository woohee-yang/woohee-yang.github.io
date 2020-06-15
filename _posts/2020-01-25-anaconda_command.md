---
title:          "자주 사용하는 Anaconda 명령어" ## 포스트 제목
categories:       
    - Anaconda
tags:           
    - basic
    - command
comments:       true ## 댓글 기능
last_modified_at : 2020-01-24 22:47:00
published: false
---

가상환경 관리를 쉽게 하려고 아나콘다를 매일 사용하지만 명령어는 항상 까먹는 나를 위한 기록

1. conda update
```
conda update -n <env> conda
```
2. 모든 패키지 업데이트 (패키지 의존성 주의!)
```
conda update --all
``` 
3. 새로운 가상환경 만들기
```
conda create --name <name> <option>
```
`<option>` : 설치할 언어 및 패키지 특정 버전 명시 가능
4. jupyter notebook nbextension
```
conda install -c conda-forge jupyter_contrib_nbextensions
jupyter contrib nbextension install --user
```

  {% if site.disqus-shortname %}{% include disqus.html %}{% endif %}
