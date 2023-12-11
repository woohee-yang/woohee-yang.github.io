---
title:          "Docker 임시 정리"
categories:       
    - Docker
tags:           
    - docker
    - command
comments: true
last_modified_at : 2023-12-11 17:30:24
toc: true
---

## Dockerhub 이용하기

- dockerhub : 도커에서 공식적으로 만든 도커이미지 배포 사이트

- 주소 : [https://hub.docker.com](https://hub.docker.com)

++ pytorch docker image : [Pytorch 도커 이미지](https://hub.docker.com/r/pytorch/pytorch)

---

## Docker 명령어 정리

### 1. docker image

- 도커 이미지 pull 받기

```
docker pull pytorch/pytorch
```

- 다운받은 도커 이미지 목록 보기

```
docker images
```

![docker images](/assets/images/custom/docker_images.png)


### 2. docker container run & info

#### 1) 도커 컨테이너 목록 확인하기 

```
docker container ls -a
```

- a옵션을 넣어야 정지된 컨테이너까지 포함하여 만들어 놓은 모든 컨테이너를 확인할 수 있다.

#### 2) 도커 컨테이너 만들기

```
docker run --name <컨테이너 이름> -itd --restart always --gpus all --shm-size=8G -v <로컬 폴더 경로>:<컨테이너 내 경로> <도커 이미지>
```


`--name <컨테이너 이름>`
- 만들 컨테이너의 이름을 붙여준다. 옵션을 안주면 도커가 임의의로 붙여준다. 
- 임의로 붙여주는 이름 예시 : peaceful_ardinghelli


`-it` 
- 컨테이너를 만들때 기본으로 많이 주는 옵션이다. 
- 컨테이너의 foreground 모드 옵션으로, t는 terminal 환경을, i는 standard input/output을 쓸 수 있는 환경을 만들어준다. 
- 다시말해, -it 옵션을 주면 표준 입출력이 활성화된, 상호작용 가능한 쉘 환경을 사용할 수 있다.
- 좀 더 쉽게 말하면, /bin/bash를 실행하여 CLI 환경을 쓸 수 있게 해준다.


`-d`
- d는 detached 모드를 뜻한다.
- 이는 컨테이너를 백그라운드에서 동작하는 어플리케이션으로써 실행하도록 설정한다.
- d는 입출력이 없는 상태로 컨테이너를 실행시켜, 컨테이너 내부에서 모니터가 있고 그 환경에서는 사용자 입출력 없이 프로그램이 실행되고 있다고 이해하면 편하다.
- detached 모드인 컨테이너는 반드시 컨테이너 내에서 프로그램이 실행되어야 하며, foreground 프로그램이 실행되지 않으면 컨테이너가 종료된다.
- 따라서 -it옵션과 함께 주어 컨테이너가 foreground/background 가리지 않고 잘 실행되게 해준다.


`--restart always`
- 컨테이너가 꺼지면 다시 시작해줄 옵션이다. 여기서는 always 옵션으로 컨테이너가 꺼지면 항상 재시작하게 해놓았다.


`--gpus all`
- 컴퓨터의 모든 gpu에 접근할 수 있게 해주는 권한을 준다.


`--shm-size=8G`
- 컨테이너가 쓸 수 있는 sheared memory 용량을 정해주는 옵션이다. 
- 도커에서는 IPC 통신으로 호스트와 컨테이너 간의 빠른 file read/write 등을 지원하기 때문이다. 본인의 컴퓨터 메모리 용량에 맞게 적절히 조절하면 된다. - 호스트환경에서도 메모리는 당연히 써야하므로 컨테이너에 더 많은 용량을 주면 안된다. 
- shared memory는 기본으로 64mb이고, 위에서 8G는 8GB를 뜻한다.
- Training을 위해 로컬내 데이터를 써야하므로 못해도 1G~8G를 추천한다.


`-v <로컬 폴더 경로>:<컨테이너 내 경로>`
- v는 volume의 약자로, 로컬과 아예 다른 가상환경을 만들어주는 컨테이너 내에서 로컬파일과 연결할 수 있게 해주는 옵션이다. 
- 예를 들어서 `-v ./code:/workspace` 라고 옵션을 주면, 현재 로컬에 있는 code 폴더를 도커 컨테이너에서는 /workspace 경로로 매칭시켜 사용할 수 있다. 
- 개념이 이해가 되지 않는다면 바로가기 폴더처럼 이름만 바뀐 경로라고 이해하는게 수월할 것이다.


#### 3) 도커 컨테이너 attach & detach

```
docker start <컨테이너 이름>
docker attach <컨테이너 이름>
```

- 이미 나와버린 컨테이너를 다시 접속할때 쓰이는 명령어다. 실행중이면 start를 안해도 되지만, 실행 중이지 않은 컨테이너면 start를 먼저하고 attach를 해줘야한다.

- 컨테이너 환경에서 나오려면 아래와 같이 컨테이너 내에 접속된 상태에서 `ctrl + p + q`를 누르자. 그러면 컨테이너를 끄지 않고 나올 수 있다.

![docker detach](/assets/images/custom/docker_detach.png)

- 컨테이너를 아예 끄고 싶다면 `ctrl + d` 나 컨테이너 내에서가 아닌 호스트 터미널에서 `docker stop <컨테이너 이름>` 을 하면 컨테이너가 정지된다.
