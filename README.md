# E1-1

**Git 버전 확인**
- `git --version`
    - git version 2.53.0
- `git config --list`는 Git 설정 및 GitHub 연동 단계에서 실행한다.

```
sevencvter4085@c6r5s6 ~ % git --version
git version 2.53.0
```

## 과제 목표
이 과제를 마친 후, 학습자는 아래를 스스로 설명할 수 있어야 한다.
- 절대 경로와 상대 경로의 차이를 예시를 들어 설명할 수 있다.
- 파일 권한의 의미(r/w/x)와 755, 644 같은 표기가 어떤 규칙으로 해석되는지 설명할 수 있다.
- 기존 Dockerfile을 기반으로 “커스텀 이미지”를 만들 수 있다.
- 포트 매핑이 필요한 이유를 설명할 수 있다.
- Docker 볼륨(영속 데이터)을 설명할 수 있다.
- Git과 GitHub의 역할 차이(로컬 버전관리 vs 원격 협업 플랫폼)를 설명할 수 있다.


---


## 1) 실행환경
- OS: macOS 15.7.4
- orb: Version: 2.0.5
- Docker: Docker version 28.5.2, build ecc6942
- Git: git version 2.53.0

**OS 확인**
- `sw_vers`
    - ProductName:		macOS   # OS이름
    - ProductVersion:	15.7.4  # OS버전
    - BuildVersion:		24G517  # 빌드 번호
- `uname -a`
    - Darwin c6r5s6.codyssey.kr 24.6.0 Darwin Kernel Version 24.6.0: Mon Jan 19 22:00:10 PST 2026; root:xnu-11417.140.69.708.3~1/RELEASE_X86_64 x86_64

```
sevencvter4085@c6r5s6 ~ % sw_vers
ProductName:		macOS
ProductVersion:		15.7.4
BuildVersion:		24G517
```

```
sevencvter4085@c6r5s6 ~ % uname -a
Darwin c6r5s6.codyssey.kr 24.6.0 Darwin Kernel Version 24.6.0: Mon Jan 19 22:00:10 PST 2026; root:xnu-11417.140.69.708.3~1/RELEASE_X86_64 x86_64
```

**셀 확인**
- `echo "$SHELL"`
    - /bin/zsh.  # 사용자 계정에 설정된 기본 셀
- `ps -p $$ -o command=`
    - -zsh  # 현재 터미널 세셩에서 실행 중인 셀

```
sevencvter4085@c6r5s6 ~ % echo "$SHELL"
/bin/zsh
sevencvter4085@c6r5s6 ~ % ps -p $$ -o command=
-zsh
```

**터미널 확인**
- `echo "$TERM_PROGRAM"`
    - Apple_Terminal  # 어떤 터미널 애플리케이션을 사용하는지
- `echo "$TERM"`
    - xterm-256color  # 프로그램들이 인식하는 터미널 호환 유형

```
sevencvter4085@c6r5s6 ~ % echo "$TERM_PROGRAM"
Apple_Terminal
sevencvter4085@c6r5s6 ~ % echo "$TERM"
xterm-256color
```


---


## 2) 수행 항목 체크리스트
- [x] 터미널 조작 로그 (위치/목록/생성/저장/읽기/복사·내용확인/이름변경·이동/삭제)
- [x] 권한 실습 (파일 1개 + 디렉토리 1개, 전/후 비교)
- [x] Docker 설치 점검 (version, info)
- [ ] Docker 기본 운영 (images/ps/logs/stats)
- [ ] 컨테이너 실행 (hello-world, ubuntu 진입)
- [ ] 커스텀 이미지 (Dockerfile 직접 작성 → 빌드·실행 성공)
- [ ] 포트 매핑 접속 증거 (주소창+응답화면 함께)
- [ ] 볼륨 영속성 검증 (삭제 전/후 데이터 유지)
- [ ] Git 설정 (config --list)
- [ ] GitHub 연동 증거
- [ ] 민감정보 마스킹 완료


---

## 3) 터미널 조작 로그
1. pwd                      # ① 현재 위치 확인
2. ls -al                   # ② 목록 확인 (숨김 파일 포함, -a)
3. mkdir test_dir           # ③ 디렉토리 생성
4. cd test_dir              # ④ 이동
5. touch empty.txt          # ⑤ 빈 파일 생성
6. echo "hello" > file.txt  # 파일 내용 작성
7. cat file.txt             # ⑥ 파일 내용 확인
8. cp file.txt copy.txt     # ⑦ 복사
9. mv copy.txt renamed.txt  # ⑧ 이동/이름변경
10. rm renamed.txt          # ⑨ 삭제

```
 # 현재위치 확인
sevencvter4085@c6r5s6 E1-1 % pwd
/Users/sevencvter4085/Documents/E1-1

 # 목록 확인(숨김 파일 포함)
sevencvter4085@c6r5s6 E1-1 % ls -a 
.		.DS_Store	README.md	file.txt	mission1.md
..		.git		empty.text	image		test_dir

 # 디렉토리 생성
sevencvter4085@c6r5s6 E1-1 % mkdir test_dir

 # 파일 생성
sevencvter4085@c6r5s6 E1-1 % touch empty.text

 # 파일 생성 및 텍스트 저장
sevencvter4085@c6r5s6 E1-1 % echo "hello" > file.txt

 # 파일 내용 읽기
sevencvter4085@c6r5s6 E1-1 % cat file.txt
hello

 # 파일 복사 및 확인
sevencvter4085@c6r5s6 E1-1 % cp file.txt copy.txt
sevencvter4085@c6r5s6 E1-1 % cat copy.txt
hello

 # 파일명 변경
sevencvter4085@c6r5s6 E1-1 % mv copy.txt renamed.txt
sevencvter4085@c6r5s6 E1-1 % ls
README.md	empty.text	file.txt	image		mission1.md	renamed.txt	test_dir

 # 파일 이동
sevencvter4085@c6r5s6 E1-1 % mv file.txt test_dir/
sevencvter4085@c6r5s6 E1-1 % ls
README.md	empty.text	image		mission1.md	test_dir
sevencvter4085@c6r5s6 E1-1 % cd test_dir 
sevencvter4085@c6r5s6 test_dir % ls
file.txt

 # 파일 삭제
sevencvter4085@c6r5s6 E1-1 % rm renamed.txt
sevencvter4085@c6r5s6 E1-1 % ls
README.md	empty.text	file.txt	image		mission1.md	test_dir
```


---


## 4) 권한 변경 실습
- 소유자/그룹/기타 3자리
    - `r`=4, `w`=2, `x`=1, `-`=0
    - 권한을 3글자씩 나눠서 숫자로 변환
        - 1번째	1글자 파일 종류
        - 2~4번째 3글자	소유자 권한
        - 5~7번째 3글자	그룹 권한
        - 8~10번째 3글자 기타 사용자 권한
    - `755` = `-rwxr-xr-x`, `644` = `-rw-r--r--`

```
 # 파일 권한 실험
sevencvter4085@c6r5s6 E1-1 % ls -l file.txt
-rw-r--r--  1 sevencvter4085  sevencvter4085  6 Jul 29 21:35 file.txt
sevencvter4085@c6r5s6 E1-1 % chmod 755 file.txt
sevencvter4085@c6r5s6 E1-1 % ls -l file.txt    
-rwxr-xr-x  1 sevencvter4085  sevencvter4085  6 Jul 29 21:35 file.txt
```

```
 # 디렉토리 권한 실험
sevencvter4085@c6r5s6 E1-1 % ls -ld test_dir   
drwxr-xr-x  2 sevencvter4085  sevencvter4085  64 Jul 29 21:34 test_dir
sevencvter4085@c6r5s6 E1-1 % chmod 644 test_dir
sevencvter4085@c6r5s6 E1-1 % ls -ld test_dir   
drw-r--r--  2 sevencvter4085  sevencvter4085  64 Jul 29 21:34 test_dir
```


---


## 5) Docker 설치 및 기본 점검
**실습 전 확인이 필요하 사항**
- OrbStack을 사용하는 경우에는 일반적으로 다음 상태여야 한다.
- OrbStack 실행 → Docker Engine 실행 → docker info 성공

**Docker 버전 확인**
- `docker --version`

```
 # 설치여부 + 버전 확인(재현성 정보로도 사용)
sevencvter4085@c6r5s6 ~ % docker --version
Docker version 28.5.2, build ecc6942
```

**상세 버전 (Client/Server 분리 확인) 및 데몬 동작 확인**
- `docker version`은 오직 `버전` 관련 정보만 보여준다.
- `docker info`와 동등한 점검이다. (Sever 항목이 나오면 데몬이 살아있는지 확인 가능)

```
 # docker version은 어떤 버전인지 확인하는 명령어이다.
 # docker --version 보다 상세 정보 확인 가능
 # Sever 부분이 나오면 데몬이 살아있다는 증거이다.

sevencvter4085@c6r9s8 ~ % docker version
Client:
 Version:           28.5.2                    # Client 정보
 API version:       1.51                      # API 정보
 Go version:        go1.25.3                  # Go 언어 버전
 Git commit:        ecc6942                   
 Built:             Wed Nov  5 14:42:30 2025  
 OS/Arch:           darwin/amd64              # 운영체제/아키텍처
 Context:           orbstack

Server: Docker Engine - Community  # Server(데몬) 버전
 Engine:
  Version:          28.5.2
  API version:      1.51 (minimum version 1.24)
  Go version:       go1.24.9
  Git commit:       89c5e8f
  Built:            Wed Nov  5 14:45:42 2025
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          v2.2.0
  GitCommit:        1c4457e00facac03ce1d75f7b6777a7a851e5c41
 runc:
  Version:          1.3.3
  GitCommit:        d842d7719497cc3b774fd71620278ac9e17710e0
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0

```

**도커 확인시 만날 수 있는 에러 상황(트러블 슈팅)**
```
 # 아래와 같은 출력이 나오면 데몬이 죽어있다는 의미이다.
Server:
Cannot connect to the Docker daemon at unix:///Users/sevencvter4085/.orbstack/run/docker.sock. Is the docker daemon running?
```

- 데몬이 꺼져 있는 상태로 확인
- OrbStack을 실행하여 다시 확인해보면 된다.

---


## 6) Docker 기본 운영 명령
- 이미지: 다운로드/목록 확인 (예: docker images)
- 컨테이너: 실행/중지/목록 확인 (예: docker ps, docker ps -a)
- 운영: 로그 확인 (예: docker logs), 리소스 확인 (예: docker stats)
- 수행 명령과 출력 결과를 기술 문서에 남긴다.

- 이미지(Image)
    - 실행 준비 된 틀/설계도
    - 붕어빵 틀
    - 상태고정(변하지 않음)

- 컨테이너(Container)
    - 이미지를 실행항 실체
    - 틀로 찍어낸 붕어빵
    - 실행/중지/삭제 가능

- 이미지 1개로 컨테이너 여러 개를 만들 수 있다.

1. 이미지 목록 확인
    - `docker images` → 내 PC에 있는 이미지 목록
2. 이미지 가져오기
    - `docker pull nginx` → Docker Hub에서 nginx 이미지를 다운로드
3. 컨테이너 실행 (run)
    - `docker run -d --name my-web nginx`
    - `run`
        - Docker run은 내무적으로 두 작업을 한 번에 수행한다.
            - docker create, docker start
            - 따라서 이미 존재하는 컨테이너를 다시 실행하는 명령어가 아니라, 새 컨테이너를 만들어 실행하는 명령어이다.
    - `-d`: 백그라운드 실행(detached)
        -  -d는 detached mode로 백그라운드 실행 모드를 의미한다.
        - -d를 사용하면 Nginx가 실행된 상태에서도 터미널을 계속 사용할 수 있다.
        - 실행 결과로는 긴 문자열이 출력 될수 있는데 이 문자열은 생성된 컨테이너의 컨테이너 ID 이다.
        - -d를 사용하지 않으면 컨테이너가 터미널의 현재 화면에 연결되어 실행된다. 터미널에 Nginx 로그가 출력되고, 해당 터미널을 바로 사용하기 불편해진다.
    - `--name my-web`: 컨테이너 이름 지정
        - 생성할 컨테이너의 이름을 my-web으로 지정한다.
        - 컨테이너 이름을 지정하지 않으면 Docker가 임의의 이름을 생성한다.
        - 이름을 지정하면 이후 명령어에서 컨테이너ID 대신 지정한 이름을 사용할 수 있다.
            - docker stop my-web
            - docker start my-web
            - docker logs my-web
            - docker rm my-web
    - `nginx`: 사용할 이미지
        - 컨테이너를 만들 때 사용할 Docker 이미지 이름이다.
        - 즉, Docker Hub에 있는 공식 Nginx 이미지를 사용하겠다는 의미이다.
        - 로컬 컴퓨터에 nginx 이미지가 없다면 Docker는 자동으로 다음 과정을 수행한다.
            1) 로컬에 nginx 이미지가 있는지 확인
            2) 없으면 Docker Hub에서 nginx 이미지 다운로드
            3) nginx 이미지로 컨테이너 생성
            4) 컨테이너 실행

4. Docker는 docker run -d --name my-web nginx 이 명령어를 받으면 다음 순서로 동작한다.
    1) 로컬에 nginx:latest 이미지가 있는지 확인한다.
    2) 이미지가 없으면 Docker Hub에서 내려받는다.
    3) nginx 이미지로 새로운 컨테이너를 만든다.
    4) 컨테이너 이름을 my-web으로 지정한다.
    5) Nginx를 백그라운드에서 실행한다.

5. 주의할 점(트러블슈팅)
    - 같은 이름의 컨테이너가 이미 존재한다면 다음과 같은 충돌 오류가 발생한다.
        - The container name "/my-web" is already in use
    - 이 경우 기존 컨테이너를 삭제하거나 다른 이름을 사용해야 한다.
        - docker rm -f my-web
    - 실행 중인 Docker는 `Ctrl+C`로 종료 가능

6. 실행 중인 컨테이너 확인
    - `docker ps` → 실행 중인 것만
    - `docker ps -a` → 중지된 것 포함 전체

7. 컨테이너 중지 / 삭제
    - `docker stop my-web` → 중지
    - `docker rm my-web` → 삭제

전체 흐름
[준비]  images(확인) → pull(가져오기) → images(재확인)
[실행]  run(실행) → ps(실행 확인)
[운영]  logs(로그 확인) → stats(리소스 확인)
[정리]  stop(중지) → rm(삭제)


**이미지 목록 확인**
```
 # 현재 이미지 목록 확인(작업전)
sevencvter4085@c6r9s8 E1-1 % docker images
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
```

```
 # 이미지 가져오기
sevencvter4085@c6r9s8 E1-1 % docker pull hello-world
Using default tag: latest
latest: Pulling from library/hello-world
4f55086f7dd0: Pull complete 
Digest: sha256:c3cbe1cc1aa588a64951ac6286e0df7b27fe2e6324b1001c619bb358770c0178
Status: Downloaded newer image for hello-world:latest
docker.io/library/hello-world:latest
```

```
 # 이미지 목록 재확인(작업 후)
sevencvter4085@c6r9s8 E1-1 % docker images          
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    e2ac70e7319a   4 months ago   10.1kB
```

```
 # 컨테이너 실행 및 확인
 # hello-world는 실행 후 바로 종료
 # docker ps는 실행 중인 컨테이너 확인이라 출력에 포함 되지 않음
 # docker ps -a로 종료된 컨테이너 확인시 hello-world 컨테이너 STATUS(Exited) 확인 가능

sevencvter4085@c6r9s8 ~ % docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

sevencvter4085@c6r9s8 ~ % docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

sevencvter4085@c6r9s8 ~ % docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED          STATUS                      PORTS     NAMES
9677d488f331   hello-world   "/hello"   21 seconds ago   Exited (0) 20 seconds ago             confident_brahmagupta
```

```
 # docker logs
 # 위에서 docker ps -a 로 확인 된 CONTINAER ID를 이용

sevencvter4085@c6r9s8 ~ % docker logs 9677d488f331

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

```

```
 # docker stats
 # stats의 기본 동작은 실시간 모니터링이다.
 # --no-stream 을 붙이면 스냅샷(현재 상태 한 번만 찍기) 확인이 가능하다
 # hello-world는 실행 후 바로 종료 되기 때문에 기록은 있지만 전부 0B/0으로 껍데기만 남은 상태이다.

sevencvter4085@c6r9s8 ~ % docker stats --no-stream 9677d488f331
CONTAINER ID   NAME                    CPU %     MEM USAGE / LIMIT   MEM %     NET I/O   BLOCK I/O   PIDS
9677d488f331   confident_brahmagupta   0.00%     0B / 0B             0.00%     0B / 0B   0B / 0B     0
``` 


## 7) 컨테이너 실행
