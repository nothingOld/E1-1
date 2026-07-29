# E1-1

**과제 목표**
이 과제를 마친 후, 학습자는 아래를 스스로 설명할 수 있어야 한다.
- 절대 경로와 상대 경로의 차이를 예시를 들어 설명할 수 있다.
- 파일 권한의 의미(r/w/x)와 755, 644 같은 표기가 어떤 규칙으로 해석되는지 설명할 수 있다.
- 기존 Dockerfile을 기반으로 “커스텀 이미지”를 만들 수 있다.
- 포트 매핑이 필요한 이유를 설명할 수 있다.
- Docker 볼륨(영속 데이터)을 설명할 수 있다.
- Git과 GitHub의 역할 차이(로컬 버전관리 vs 원격 협업 플랫폼)를 설명할 수 있다.


---


1) 실행환경
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

**터미널 조작 로그**
1. pwd                      # ① 현재 위치 확인
2. ls -al                   # ② 목록 확인 (숨김 파일 포함, -a)
3. mkdir test_dir           # ③ 디렉토리 생성
4. cd test_dir              # ④ 이동
5. touch empty.txt          # ⑤ 빈 파일 생성
6. echo "hello" > file.txt  # 파일 내용 작성
7. cat file.txt             # ⑥ 파일 내용 확인
8. cp file.txt copy.txt     # ⑦ 복사
9. mv copy.txt renamed.txt  # ⑧ 이동/이름변경
10. rm renamed.txt           # ⑨ 삭제

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


2) 수행 항목 체크리스트
- [x] 터미널 기본 조작 및 실행환경 확인
- [x] 권한 변경 실습
- [] Docker 설치/점검
- [] hello-world 실행
- [] Dockerfile 빌드/실행
- [] 포트 매핑 접속(2회)
- [] 바인드 마운트 반영
- [] 볼륨 영속성
- [] Git 설정 + VSCode GitHub 연동


---


3) 권한 변경 실습
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


4) Docker 설치 및 기본 점검
**Docker 버전 확인**
- `docker --version`

```
sevencvter4085@c6r5s6 ~ % docker --version
Docker version 28.5.2, build ecc6942
```

**Docker 데몬 동작 확인**
- `docker info`  # Docker Client와 Server 관련 정보 확인

```
 # OrbStack을 사용하는 경우에는 일반적으로 다음 상태여야 한다.
 # OrbStack 실행 → Docker Engine 실행 → docker info 성공

sevencvter4085@c6r5s6 ~ % docker info
Client:
 Version:    28.5.2
 Context:    orbstack
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.29.1
    Path:     /Users/sevencvter4085/.docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v2.40.3
    Path:     /Users/sevencvter4085/.docker/cli-plugins/docker-compose

Server:
Cannot connect to the Docker daemon at unix:///Users/sevencvter4085/.orbstack/run/docker.sock. Is the docker daemon running?
```

**Git 버전 확인**
- `git --version`
    - git version 2.53.0
- `git config --list`는 Git 설정 및 GitHub 연동 단계에서 실행한다.

```
sevencvter4085@c6r5s6 ~ % git --version
git version 2.53.0
```

5) Docker 기본 운영 명령

