---
title: "Github Blog Trouble Report 1 : Can't Display Post" ## 포스트 제목 
category:       
    - Jekyll Blog
tags:           
    - trouble report
    - post
    - display
comments:  true ## 댓글 기능
last_modified_at : 2020-02-01 16:08:00
---

***오류 리포트는 개인적인 기록이므로 공식적인 문체가 아닙니다.***

<hr>

## 첫 번째 오류 리포트

&nbsp;&nbsp;&nbsp;&nbsp; 갑자기 어제부터인가 블로그에 포스트가 안 올라가기 시작했다. 어제 막 커스터마이징에 지쳐 티스토리로 이전하려고 포스트를 옮기려고 준비중이었다가 마크다운 문제로 마음을 접었었다. 그리고 다시 지킬 블로그에 포스팅을 하니 먼저 배신하려해서 그런가 포스팅이 안되기 시작했다.....

## 최종 해결책

답답하니 최종 해결책부터 말하고 아래에서 해결 단계를 기록하기로 한다. 먼저 내가 만난 에러는 다음과 같다.

```
Your site is having problems building: Your SCSS file assets/css/main.scss has an error on line 342: 
Incompatible units: 'px' and 'x'.
```

이는 현재 블로그 repository에 setting에 아래 이미지에 있는 Github Pages 섹션에서 발견할 수 있었다.
![Github Pages](/assets/images/trouble_report.PNG)

(초록 박스가 노란색으로 변해있었고 위의 문구가 표시되고 있었다.)

그래서 경고문 경로 그대로 따라가서 보았으나 main.scss에서 include로 코드를 포함하고 있어 그 파일을 찾아가도 또 include하고 바람에 줄번호 따위 아무 쓸모짝이 없게 되버렸다...

하지만 css 파일 중 px를 x로 잘못 쓰고 있어 문제를 일으키고 있다는 것을 파악하여 commit 기록을 보고 문제를 찾을 수 있었다. 왜 진작 기록을 볼 생각을 안했는지 모르겠다...

![commit log](/assets/images/2020-02-01-commit_기록.PNG)

기록에서 보면 2020년 01월 29일 내가 블로그 글 영역 폭을 넓히겠다고 만졌다가 이런 사달이 난 것이었다... 이러니 이후 아무리 커밋을 해도 받아주지 않았던 것이었다...ㅠㅠ 제발 커밋 기록부터 먼저보고 삽질하지 말자...

### 참고 링크
<hr>

**1\.** 이 대삽질 와중에도 수확이 있다. 아래 링크에서 포스팅이 안될 때 거의 모든 해결책이 답변으로 기록되어있다. 

    [Stack overflow : Problem is my GitHub page do not update its content](https://stackoverflow.com/questions/24713112/problem-is-my-github-page-do-not-update-its-content)

**2\.** 아래 링크들은 지킬 블로그를 생성하는 방법에 대해 내가 본 포스트 중 가장 깔끔하다고 생각한다.

    [Jekyll Blog 만들기](https://xho95.github.io/blog/github/jekyll/git/2016/01/11/Make-a-blog-with-Jekyll.html)
    [하우투 : 같이 따라하기 시리즈](https://devinlife.com/howto/)

덕분에 깃과 지킬에 대해 더 공부하고 유용했으리라 생각하자...