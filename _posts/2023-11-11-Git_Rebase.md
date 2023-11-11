---
title: "Git Rebase"
categories:       
    - Git
tags:           
    - rebase
    - git
comments: true
date : 2023-11-11 02:26:10
toc: true
---

1. 자신만 쓰는 개발브랜치(ex. dev)를 만든다.
	git checkout -b dev <commitID or branch name> (원하는 커밋이나 브랜치에서 시작 가능)
	OR
	git branch dev (현재브랜치의 마지막 커밋을 따라감)

2. 자신만 쓰는 브랜치에는 모든 커밋을 원격에 push한다.
   (이 과정은 로컬 브랜치에서 커밋을 잃어버리지 않게 백업하는 용도이다.)
	git add <source file or .(현재 폴더내 수정본 전체)>
	git commit -m <commit message>
	git push origin dev

3. 자신만 쓰는 브랜치에서 기능 or 의미 단위로 묶어서 origin/main에 올리고 싶은 
   커밋을 가려내기 위해 로컬에 임시 브랜치(ex. dev-temp)를 만든다.
 	git checkout -b dev-temp <commitID or branch name>
	OR
	git switch -c <new branch name>

4. rebase 명령으로 커밋들을 묶어서 새로운 커밋을 만든다.
	git rebase -i <commitID>
	=> 이때 들어가는 commitID는 내가 묶기를 원하는 커밋의 시작점 바로 앞 commitID여야 한다.

5. rebase -i 옵션의 경우, 원하는 커밋들을 편집할 수 있게 에디터가 나오는데 
   아래처럼 pick이나 s(squash), #(주석)로 커밋을 골라서 편집할 수 있다.
      
      ** 이 과정에서 pick한 커밋에 합칠 커밋들을 고를 수 있는데 이때 커밋 순서가 중요하므로
         중간에 필요없는 커밋은 주석치고 합칠 커밋을 고르면 된다.
      
      ** 중간에 주석친 커밋을 다시 넣고 싶으면 이 전체 과정을 똑같이 다시 하면 된다.
         커밋 하나만 넣거나, 묶을 필요가 없다면 cherry-pick을 이용해 그 커밋들만 origin/main에 push하면 된다.

      ** 이때, 주의할 점은 반드시 임시 브랜치에서 커밋을 편집해야 한다.
         원격 브랜치에서 이 작업을 할 경우, 주석으로 빼버린 커밋을 다시 살리지 못할 수도 있기 때문이다.
         그리고 만약 다른 사람이 내 개발 브랜치(origin/dev)를 같이 사용할 때 갑자기 커밋이 없어지거나 rebase로 인해
         새로운 커밋이 생겨서 다른 사람의 로컬이 달라질 수 있기 때문에 반드시 내 로컬에서 rebase 작업을 수행하고 마쳐야 한다.
 
6. -i를 종료한 후에는 아래와 같이 새로운 커밋 메세지를 쓸건지 이전 커밋 메세지를 쓸건지 써줄 수 있다.

7. 편집이 끝난 후, 새로운 rebase 커밋이 생성되었으면 그 커밋을 cherry-pick으로 origin/main에 push한다.
	git checkout main
	git cherry-pick <생성된 commitID>
	git push

   ** 이 과정에서 origin/main에 있던 내용과 push할 내용이 충돌날 수 있다. merge conflict를 해결한 것과 동일하게
       해결하고 다시 commit 후 push 해주면 된다.

8. 모든 과정이 끝난 임시 브랜치는 "반드시" 삭제해준다. 삭제하지 않고 남을 경우에 추후 이 git을 관리하는 사람이 헷갈릴 수 있기 때문에 이를 방지한다.
	git branch -d <branch name> 
	(-d 옵션은 soft delete 삭제할 브랜치에 모든 커밋이 다른 브랜치들에 push 안되어 있으면 이 브랜치를 지우지 않는다.)
	OR
	git branch -D <branch name>
	(-D 옵션은 강제 삭제)