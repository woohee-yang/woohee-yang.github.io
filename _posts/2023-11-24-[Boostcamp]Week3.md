---
title:          "[네이버 부스트캠프 AI Tech 6기] 3주차 회고"
categories:       
    - Naver_BoostCamp_AI
tags:           
    - 회고
    - 부캠
comments: true
last_modified_at : 2023-11-24 20:48:10
toc: true
---

## 3주차 멘토링 정리

1. 논문을 어떻게 찾는가???
	-  넓은 주제
		* 최신 Review 논문 (옛날은 안돼!!!!!, 최소 2-3년)
		* 완전히 뭐를 찾아야할지 모를때 : CVPR2023 논문들중 가장 많이 발표된 논문 (CVPR2023 by the Numbers)[https://public.tableau.com/views/CVPR2023SubjectAreasbyTeamSize/Dashboard2a?:showVizHome=no]
	-  특정 주제 (좁은 주제)
		* 키워드 “awesome” 붙여서 github 내용 검색
	- 저자를 찾기! -> 대학인 경우에는 랩실 단위로 보거나 관심있는 교수님 논문을 본다.
2. 논문을 어떻게 읽는가??<br>
	1)  arXiv 버전을 먼저 찾는다.<br>
	2) 컨퍼런스 제출하느라 업데이트가 되었거나 내용을 빠뜨렸을 수도 있으니 찾아본다<br>
	3) 읽기<br>
		- 잘 알때(전문가) : experiments 위주로 봄<br>
			(2) 요약을 읽음 -> 잘 아는 내용이면 넘어가기도<br>
			(3) 요약을 잘 모를때 -> Related Work<br>
			(4) Introduction ~= Abstarct<br>
			=> 논문에 흥미도에 따라 읽을지 말지 판가름<br>
			(5) 쓰고 싶으면 : Experiments<br>
		- 잘 모를때 : 순서대로<br>
			(1) 컨셉 설명 사진 봄(abstract위에)<br>
			(2) Related Work<br>
			(3) Introduction ~= Abstract<br>
			(4) Proposed Method 다 읽어야함<br>
			(5) 실험에서 쓴 평가지표 읽기<br>
			(6) Ablation Study는 다르게 실험해본 내용이니 공부할때는 필요X<br>

---

## 피어세션 정리 
- 3주차 피어세션에서는 알고리즘 5문제 / 프로젝트 환경 셋팅 및 결과 공유 / 각자 과제 질문을 진행하였다.
    - 미로탈출
    - 호텔 대실 
    - 무인도 여행
    - 뒤에 있는 큰 수 찾기
    - 숫자 변환하기
- 이번주는 강의 난이도 급상승?으로 피어세션 시간이 비교적 널널해졌다. 그리고 다음주 월요일에 yolo v8를 각자 돌려보고 발표하기 위해 이번주에는 더 부가적으로 할 일을 만들지 않았다.
- 강의에서 ViT와 AAE를 배우며 서로 질문을 하면서 나는 당연하지라고 넘겼던 내용을 한 번 더 생각해보게 되었다. 새삼스레 같이 공부하는 힘을 느꼈던 것 같다.
- 추가로 논문 내용이 수업에 나오니 기초가 되는 논문들인 Attention All You Need, Seq2Seq, ResNet 논문들을 같이 주말에 리뷰하자고 제안하여 주말에 모이기로 했다!
- 끝으로 오늘 스폐셜 피어세션에서 꿈에 대해 질문해주셨던 분이 계셔서 우리 팀원들에게도 꿈에 대해 말해보았다. 생각보다 말하면서 내가 어떤 방향을 원하는지 정리할 수 있게 되서 기회가 되다면 블로그에도 올려보겠다ㅎㅎ
- 주말에 팀원들과 만나서 Variational Inference에 대해 내가 설명해보기로 했기 때문에 동기부여가 팍팍되서 더 열심히 공부할 수 있을 것 같다!!!

---

## 스폐셜 피어세션
- 이번에 또 다른 캠퍼분들을 뵈며 새삼 부스트캠프에 I가 많다는 것을 다시 느꼈다... 은근히 중간중간에 침묵이 있었는데 다행이 나 말고도 E이신 분이 계셔서 즐거운 시간을 보냈던것 같다.
- 다른 분들의 다양한 경험담을 들을 수 있어 즐거운 시간이었다. 그런데 저번주에 듣자라고 생각했더니 진짜 내가 듣는걸 더 많이해서 질문을 더 많이 해볼 걸 그랬다ㅠㅠ
- 다음주에는 질문 몇 가지만 생각해보고 들어가자!
- 어떤 팀에서는 강의마다 있는 Further Question 서로 토론한다고 하였다. 정신없이 강의듣느라 놓쳤던 부분인데 나도 나름 답을 정리해보고 팀원들과 이야기 나눠보아야겠다!

---

## 회고

### 잘했던 것, 좋았던 것, 계속할 것
- 피어세션에서 팀원분이 Receptive Field에 대해 질문하셔서 미리 질문이 나올걸로 예상했기에 준비했던 자료를 드렸다! 그리고 상당히 반응이 좋아서 생생정보팁에도 같이 공유했다!
- 오랜만에 다시 medeley켜서 읽은 논문들을 정리해나가고 있다.
- 지난주와 마찬가지로 질의응답 게시판을 틈날때마다 보면서 대답할 수 있는 것들은 대답하고, 아닌 것들은  나름 생각을 정리해놨다가 조교님들이 달아주시는 답변과 비교해 보았다.
- 최성준 마스터님의 마스터 세션에서 논문의 의미를 파악하는 방법에 대해 질문했다. 마스터님께서 비판적 사고를 가지고 논문이 왜 쓰여졌을까? 이 논문을 쓰게된 동기가 무엇일까? 를 생각해보라고 하셔서 나도 똑같이 해보아야 겠다!!

### 잘못했던 것, 아쉬운 것, 부족한 것 -> 개선방향
- 블로깅을 하려고 자료를 모으고 생각을 정리해서 옵시디언에 짤막하게 쓰고 있는데 블로그에 올릴 정도로 정리되지 않아 아쉬운 것 같다. -> 조금만 길게 정리해놓자!
- 강의를 듣는거에 몰두되어 논문이라던지, 더 있었던 질문이라던지 놓쳤던 것들이 있는 것 같다. -> 강의를 씹어먹겠다!! 라는 마음으로 꼼꼼하게 노트정리를 해 봐야겠다.
- 주말로 넘겨놓은 일들이 많아졌다... -> 시간관리법이 전혀 통하지 않고 있어서 앞으로는 시계를 차고 타이머로 집중관리를 해야할 것 같다!

### 도전할 것, 시도할 것
- 타이머와 알람을 이용한 시간 통제!
- 강의 정리 적게라도 해보기!
- Further Question 꼭꼭 읽어보기!
- 논문을 읽으면서 왜? 작성했을지 찾아보거나 생각해보기

### 공부한 것, 알게된 것, 느낀점 + 추가로 알아본 것
- SGD는 왜 Stochastic 인가?
- receptive field 설명 : [https://distill.pub/2019/computing-receptive-fields/]
- nvidia pytorch tensorRT
[https://docs.nvidia.com/deeplearning/frameworks/pytorch-release-notes/running.html]
- 우리집 컴퓨터(a.k.a 춘식이)에 프로젝트 환경셋팅을 다시 하면서 docker 새로운 tensorRT라는 새로운 라이브러리를 알았다! nvidia에서 직접 제작한 텐서 연산에 최적화된 라이브러리라고 한다. 
- vscode powerline font 적용: [https://github.com/romkatv/powerlevel10k/issues/671]
- CVPR 2023 트랜드 : [https://public.tableau.com/views/CVPR2023SubjectAreasbyTeamSize/Dashboard2a?:showVizHome=no]
- on / offline data aggumentation : [https://stackoverflow.com/questions/72388663/online-vs-offline-data-augmentation]
- powerlevel10k : [https://github.com/romkatv/powerlevel10k#how-do-i-change-prompt-colors]
- powerlevel10k configure reset command : p10k configure
