---
title: "[네이버 부스트캠프 AI Tech] 개발 환경셋팅 및 협업도구 정리"
categories:       
    - Environment Setting
tags:           
    - Anaconda
    - Docker
    - Linux
    - Jupyter
comments:       true 
last_modified_at : 2023-11-09 10:00:00
toc: true
---

> 팀 활동을 위해서 colab을 사용하지만, 개인적으로 사용하는 도구들 정리

## 1. Mac용 터미널
1. iTerm2 + zsh
    - [https://iterm2.com](https://iterm2.com)
    - 가장 널리 사용되는 앱
    - 기본 터미널과 유사한 UI
    - zsh을 이용한 여러 컬러테마, 글꼴, newline, highlight 등 커스텀 가능

2. Warp
    - [https://www.warp.dev/b](https://www.warp.dev/b)
    - iTerm2와 양대산맥같이 사용되는 앱
    - 기본으로 zsh를 이용한 커스텀 기능 제공
    - UI설정을 파일이 아닌 셋팅기능이 따로 있어서 편하게 가능
    - AI command 추천 기능 탑재
    - VS Code와 같이 command palette 제공
    - 히스토리 검색기능을 따로 제공
    - 워크플로우(어떤 업무를 하기 위한 명령어들의 모음)검색 기능 제공

3. Termius
    - [https://termius.com](https://termius.com)
    - ssh client
    - 데스크탑 뿐만아니라 모바일용(ios, android)도 제공 (아이패드도 가능!)
    - Basic / Premium / For Teams 버전별로 가격상이 (베이직은 무료!)
    - 호스트 정보를 라벨링하여 여러 호스트를 관리하기 용이
    - 포트 포워딩 정책도 쉽게 관리할 수 있게 UI 제공
    - 유료버전에서 제공되는 기능들도 있음
    - 테마 커스텀이 가능

---

## 2. 아나콘다 설치

- 아나콘다는 각 환경에 맞게 다운로드 받아서 설치
    
    [https://www.anaconda.com](https://www.anaconda.com/)
    
- docker를 이용해서 컨테이너를 만들어서 사용하기도 하나, 현재는 간단한 환경구축을 위해 아나콘다로만 이용
- docker는 실험환경을 통째로 배포하거나, 패키지 dependency 가 프로그램별로 다를때 사용한다. 그래서 주로 다른 사람의 옛날코드를 실험해 볼때 사용하였다.
    
    → 요새는 버전별로 프로그램이 만들어지니까 보안문제보다 개발자 입장에서 고정된 버전을 선호하기에 사용됨
    
    → 하지만, 업데이트되지 않은 구버전에서 보안문제가 언제 터질지 모르니 컨테이너를 사용한다.
    
    → 개념상 컨테이너는 커널은 공유하고 CGOUP(도커는 runc, libcontainer(구버전)) 따로 분리해서 사용하기 때문에 보안상 이점이있다.
    
    → but! 컨테이너도 완벽한 보안은 해결책은 아니다. 모든 컨테이너들이 커널을 공유하기 때문에 보안문제가 있는 코드가 커널에 침입할 수 있다거나 다른 컨테이너의 정보를 볼 수 있다거나, 호스트를 직접 공격할 수 있는 문제가 있다면 컨테이너도 완벽한건 아니다. (docker host attack 등)
    
    ➕ 리눅스 권한
    
    - uid0 == root
    - uid0 ≠ root == 일반사용자(sudo가 있든 아니든 상관 x)
    - sudo는 단지 privilege escalation용 프로그램
    - chmod
        - d : 디렉토리여부
        - d다음 3글자 == owner 권한 : rwx (읽기/쓰기/실행)
        - 2번째 3글자 == group 권한 : rwx
        - 3번째 3글자 == other 권한 : rwx
    - rwx말고 s :: Setuid / Setgid / Sticky Bit
        - setuid(set owner)
        - setgid(set group)
        - S(대문자) : x가 없음!
            
            → 이걸 주로 사용하는 경우 : —S——— root root
            
            ⇒ 사용자별로 ACL을 이용하여 권한있는 사람들만 관리하도록 함
            
            ⇒ `setfacl -m u:adm:rx privileged_command`
            
            ⇒ ACL >> 파일별 권한정보
            
            ⇒ ACL owner에 x권한이 있는 사용자가 root 권한으로 해당 파일을 실행시키고 싶을때
            
            ⇒ 그래서 대문자 S는 정말 거의 쓸 필요가 없음
            
        - s(소문자) : x가 있음!
            
            → ex) sudo :: -rs-s—x—x root wheel
            
            ⇒ 소문자 s가 sudo의 owner의 uid로 sudo를 실행 (현재는 root로 되어 있으니까 uid0으로 실행)
            
            ⇒ sudo 프로그램 자체는 uid를 변경시키지 않는다. 해당 사용자와 그룹이 sudoers에 포함이 되어 있는지 검사하고 비밀번호를 검증한 후 호출한 프로세스를 실행만 시켜줌
            
            ⇒ 예를 들어, 내가 root권한으로 실행시키고 싶은 파일을 실행 당시에만 sudo를 호출해서 s가 sudo파일의 owner인 root권한으로 해당 프로세스를 실행
            
        - sticky bit(마지막 s)
    - ACL(Access Control List) : 사용자별로 가진 쓸 수 있는 파일 또는 디렉토리 목록 >> 파일별 권한정보
        - ACL은 구현체 X, 개념! ⇒ 유명한 구현체 : SELinux (Security-Enhanced Linux) 커널 모듈

⇒ 따라서, 컨테이너 안에서 잘못 root 권한을 가지게 되면 커널 모듈이나 다른 컨테이너의 root권한을 가지는 파일 또는 디렉토리들을 볼 수 있게 된다.

➕ Unprivileged container : uid가 0이 아닌 사용자로 실행되는 컨테이너

- pid는 실행시킨 uid나 gid를 사용
- but, uid0으로 실행시키면 컨테이너가 호스트 환경을 와장창! 가능
- 호스토와 컨테이너가 가진 uid를 분리시키고, cgroup을 만들어서 서로를 다른 번호로 매칭시켜줌
- 여기서 가상머신이랑 가장 큰 차이점을 보인다!!!
    - 가상머신은 커널 + OS까지 모두 통째로 호스트 위에서 실행시키기 때문에 호스트랑 VM내의 pid가 완전히 따로임
        
        → 모든 자원을 프로세스 권한으로 호스트한테 얻어와야함
        
        → 맥위에서 프로세스(페러럴즈 같은)로 윈도우 사용가능
        
    - 컨테이너는 컨테이너와 컨테이너내의 pid가 다름!
        
        → 모든 자원을 호스트의 커널 권한으로 직접 공유
        
        → 리눅스 커널로 리눅스OS가 담긴 컨테이너 밖에 사용못함
        

➕ Cgroup

- Namespace isolation : uid를 분배
- 리눅스에서는 따로 cgroup에 의해서 할당된 uid들만 사용할 수 있다. ex) 컨테이너가 20만~21만 까지만 할당받는다.
    
    ⇒ 따라서 호스트와 컨테이너내에서 한정적인 uid를 잘 분배해서 써야됨
    

++ 클라우드 OS

→ 호스트 OS 위에서 클라우드내 OS를 불러와서 쓰거나 vmware horizon같은 VM을 돌려서 중앙에서 해당 시스템을 감시 또는 추적할 수 있게함(보안상 이점)

→ 인텔의 경우에는 vpro라는 기능을 넣은 칩을 판매하여 바이오스 위에서 돌아가는 원격프로그램으로 해당 시스템을 감시, 추적, 원격포맷 등의 일을 할 수 있게함

---

## 3. 필수 패키지 설치

- numpy
- scipy
- pandas
- matplotlib
- seaborn
- plotly
( - jupyter notebook )
( - jupyter_contirb_nbextensions )
( - jupyter lab )
- scikit-learn
- pytorch
- tensorflow

---

## 4. jpyter nbextensions

1. 설치 방법
    - https://jupyter-contrib-nbextensions.readthedocs.io/en/latest/install.html
    - 위의 링크 참고
    
    <aside>
    ✔️ CONDA
    
    `conda install -c conda-forge jupyter_contrib_nbextensions`
    
    </aside>
    
    <aside>
    ✔️ PIP
    
    `pip install jupyter_contrib_nbextensions`
    
    </aside>
    
    `📌 conda 환경에서는 conda로만 설치해야함! pip과 다른 연관패키지가 설치되는 것으로 보임`
    
    ---
    
    # 4. nbextensions 필요 모듈 활성화
    
    - Autopep8
    - Codefolding
    - Collapsible Headings
    - Live Markdown Preview
    - Navigation-Hotkeys
    - Python Markdown
    - table_beautifier
    - Variable Inspector
    - AutoSaveTime
    - Code Font Size
    - Codefolding in Editor
    - Excute Time
    - Table of Contents
    - Hide input all
    - Hinterland

    ---

## 6. Jupyter Lab
 - Jupyter Notebook의 대체제로 개발된 웹개발 환경

> As JupyterLab 4 development continues, and Notebook 7 development follows, Notebook 7 will eventually replace the classic Jupyter Notebook. (Jupyter Lab 공식문서 발췌)

 - 웹 IDE같은 모습으로 노트북의 단점들을 개선 시킴
 - 파일트리, 다중창 지원 등 더 편리한 UI 개선
 - 기존 노트북과 같이도 사용가능

---

## 7. 팀활동 및 지식관리를 위한 도구

1. Notion
    - 팀회고록, 알고리즘 공부자료, 과제 내 질문 등을 정리하는 용도로 사용하고 있음
    - 팀원을 해당 페이지 멤버로 초대하여 팀내에서만 공유하고 있음
    - 페이지 공유시 무료 버전의 경우 팀원 제한이 있으므로, 공유중지시에는 꼭 팀원들에게 해당 페이지를 복사하도록 알려줘야함

2. Slack
    - 부스트캠프에서 기본적으로 사용하는 채팅채널
    - 여러 채널을 개설해서 질문과 팁 등을 공유
    - 멘토님들과 조교님들과의 연락 채널
    - 앞으로 꿀팁 많이 올려봐야지!

3. Github
    - 초반은 개인 코드 보관용으로 사용
    - 터미널에서 사용이 익숙하여 따로 데스트탑용 앱은 사용하지 않음
    - git push 꼬여서 rebase로 개고생한 포스팅은 추후에...
    - 이제 pull은 꼭 rebase로 바꿔 선형적으로 깃 관리 중
    - git merge로 관리했을때 충돌이 일어날 문제도 있고, 깃그래프가 매우 복잡해지는 것을 방지하기 위해 rebase를 이용하여 main 브랜치 선형적 관리
    - 자신의 로그를 기능단위로 묶어서 배포하기도 편하여 히스토리 관리도 용이

4. gitk (mac에서 설치)
    ```
        brew update
        brew install git
        brew install git-gui

        // git 경로 확인
        type -a git

        // usr/local/bin/git 이 보여야함
    ```
    - 깃 그래프 UI 확인용
    - 간단하게 확인하기 편함
    
    ![git graph in gitk](/assets/images/custom/gitk.png)

5. Obsidian
    - 개인적인 공부내용이나 포스팅 예정용 내용을 관리
    - 노션과 다르게 마크다운으로 작성해야되서 수공예가 많이 들어감
    - 하지만, 백링크 연결과 그래프뷰 기능을 포기할 수 없었다
    - 유료버전에서는 옵시디언 블로그로 바로 퍼블리싱이 가능
    - icloud나 google drive, github 등을 연결하여 여러 플랫폼 사용 가능

6. VSCode
    - colab이나 주피터와 같은 노트북형식 외 모든 코드에서 사용
    - 블로그 포스팅 git 관리
    - 커스텀이 필요한 영역이 좀 있어서 포스팅 예정