---
title: "Github Blog Trouble Report 2 : Markdown Internal Link and New Category" ## 포스트 제목 
category:       
    - Jekyll Blog
tags:           
    - trouble report
    - markdown
    - link
comments:  true ## 댓글 기능
last_modified_at : 2020-02-02 05:01:00
---

***오류 리포트는 개인적인 기록이므로 공식적인 문체가 아닙니다.***

<hr>

## 두 번째 오류 리포트

1. 내부 링크 첨부하느라 2시간은 넘게 걸린듯 하다... 이미지 내부 링크와 다르게 문법을 작성해야 한다고 한다. 그렇지 않으면 지킬이 정확한 url을 생성할 수 없다고 한다. 아마 markdown문서를 지킬이 html로 변환하여 보여주기 때문에 github에 저장된 url과 변환된 url은 달라서 그런듯 하다. 해결책은 의외로 아주아주 가까운 지킬 공식 문서에 있었지만 난 이걸 또 스택오버플로우까지 가서 찾을 수 있었다지ㅠㅠ.....

2. 갑자기 왜 새로운 카테고리를 만들고 싶다는 생각이 들었을까...(잠 좀 자자ㅠㅠ) 새로운 카테고리는 단순히 원래 카테고리 모음에서 추가만 하면 된다. 즉, _pages에 원하는 카테고리 페이지를 추가하고 각 포스트들에는 원하는 카테고리명을 적으면 된다. 사실 이게 다다...

3. 이 포스트를 게시하다 발생해 버린 오류.... "%" 절대 그냥 사용해서는 안될 퍼센트 기호...

4. liquid tag 오류


## 최종 해결책

1. Markdown Internal Link

    ```
    [원하는 문구]({\\% link 포스트위치/포스트이름.md \\%})

    OR

    [원하는 문구]({\\% post_url 포스트위치/포스트이름.md \\%})
    ```

    라고 적으면 문제 없이 다른 포스트로 이동이 가능하다. 이는 아래의 이미지 첨부 방법과 다르게 별도의 태그가 필요하니 유념하자.

    (liquid tag문법인 "%"는 "\\"와 같이 사용하면 안됨!, 오류때문에 할 수 없이....)

    ```
    [원하는 대체 문구](이미지경로/(정확한!)이미지 이름)

    예제)
    [원하는 대체 문구] (assets/images/trouble_report.PNG)
    ```

    그리고 이미지 입력시에도 반드시 주의해야 할 것이 **확장자 대문자**도 신경써야한다. 이걸로도 1시간....ㅠㅠ

2. New Category

    이는 [하우투 : 같이 따라하기 시리즈](https://devinlife.com/howto/)에 아주 매우 상세하게 잘! 나와 있었다ㅠㅠ(9번 탭에 가서 보면 된다.)

    그래도 해야할 일을 간단하게 정리하자면 아래와 같다.

    1) 원하는 카테고리 페이지 만들기

    ![새 카테고리 페이지](/assets/images/custom/2020-02-02-pages.PNG)

    _pages/<원하는 카테고리 이름>.md 만들기

    코드 :

    ![새 카테고리 페이지 코드](/assets/images/custom/2020-02-02-pages2.PNG)

    2) 포스트에 새로운 카테고리 이름 적기

    ![새 카테고리 추가](/assets/images/custom/2020-02-02-pages3.PNG)

    새로운 카테고리 이름을 카테고리에 적어주면 된다. 카테고리 형식의 하위 목록으로 작성할 것 이므로!

    3) 네비게이터에 등록해 주기

    ![네비게이터 등록](/assets/images/custom/2020-02-02-pages4.PNG)

    새로운 카테고리를 표시하기 위해 네비게이터에 넣어주자

    끝!

3. "%" liquid tag 안에서 그냥 사용하다가 오류만 팡팡 터져서 몇 분 동안 수정했는지 모르겠다.. 함부러 사용하지 말 것!!!

4. 코드 하이라이트 기능 사용 단순 오탈자 오류지만 liquid tag 사용 되새기자는 의미
    아래 첨고 링크 2에 가면 유용한 liquid tag 종류를 볼 수 있다.

### 참고 링크
<hr>


1. 우리 모두의 스택오버플로우... 

[jekyll markdown internal links - Stack Overflow](https://stackoverflow.com/questions/4629675/jekyll-markdown-internal-links)

2. 무시말자 공식... 

[Jekyll Tag Filters](https://jekyllrb.com/docs/liquid/tags/)



