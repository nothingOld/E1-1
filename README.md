# E1-1
codyssey E1-1

**OS 확인**
- `sw_vers`
    - ProductName:		macOS   # OS이름
    - ProductVersion:	15.7.4  # OS버전
    - BuildVersion:		24G517  # 빌드 번호

---

**셀 확인**
- `echo "$SHELL"`
    - /bin/zsh.  # 사용자 계정에 설정된 기본 셀
- `ps -p $$ -o command=`
    - -zsh  # 현재 터미널 세셩에서 실행 중인 셀

---

**터미널 확인**
- `echo "$TERM_PROGRAM"`
    - Apple_Terminal  # 어떤 터미널 애플리케이션을 사용하는지
- `echo "$TERM"`
    - xterm-256color  # 프로그램들이 인식하는 터미널 호환 유형

---

**Docker 버전 확인**
- `docker --version`
    - Docker version 28.5.2, build ecc6942

---

**Docker 데몬 동작 확인**
- `docker info`  # Docker Client와 Server 관련 정보 확인
    - Client:
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

**OrbStack을 사용하는 경우에는 일반적으로 다음 상태여야 한다.**
- OrbStack 실행 → Docker Engine 실행 → docker info 성공

---

**Git 버전 확인**
- `git --version`
    - git version 2.53.0
- `git config --list`는 Git 설정 및 GitHub 연동 단계에서 실행한다.


---


1) 과제 목표
이 과제를 마친 후, 학습자는 아래를 스스로 설명할 수 있어야 한다.
- 절대 경로와 상대 경로의 차이를 예시를 들어 설명할 수 있다.
- 파일 권한의 의미(r/w/x)와 755, 644 같은 표기가 어떤 규칙으로 해석되는지 설명할 수 있다.
- 기존 Dockerfile을 기반으로 “커스텀 이미지”를 만들 수 있다.
- 포트 매핑이 필요한 이유를 설명할 수 있다.
- Docker 볼륨(영속 데이터)을 설명할 수 있다.
- Git과 GitHub의 역할 차이(로컬 버전관리 vs 원격 협업 플랫폼)를 설명할 수 있다.


2) 기능 요구 사항  
다음 요구사항을 모두 만족해야 한다.  

1. 제출 저장소 및 기술 문서
- GitHub Repository 링크로 제출한다.
- 기술 문서(README.md 등)는 아래 내용을 반드시 포함한다.
    - 모든 수행 결과는 “기술 문서(README.md 등)”에서 확인 가능해야 한다.
    - 프로젝트 개요(미션 목표 요약)
    - 실행 환경(OS/쉘/터미널, Docker 버전, Git 버전)
    - 수행 항목 체크리스트(터미널/권한/Docker/Dockerfile/포트/마운트/볼륨/Git/GitHub)
    - 검증 방법(어떤 명령으로 무엇을 확인했는지) + 결과 위치/증거 링크
- 기술 문서 내 명령/출력은 코드블록으로 정리한다.


2. 터미널 조작 로그 기록
- 다음 작업을 터미널로 수행하고, 명령어 + 출력 결과를 기술 문서에 기록한다.
    - 현재 위치 확인, 목록 확인(숨김 파일 포함), 이동, 생성, 복사, 이동/이름변경, 삭제
    - 파일 내용 확인, 빈 파일 생성

3. 권한 실습 및 증거 기록
- 권한을 확인/변경하는 명령을 수행하고, 변경 전/후 비교를 기술 문서에 남긴다.
- 최소 요구: 파일 1개, 디렉토리 1개에 대해 권한 변경 실험을 수행한다.

4. Docker 설치 및 기본 점검
- Docker 버전 확인 결과를 기록한다. (docker --version)
- Docker 데몬 동작 여부 확인 결과를 기록한다. (docker info 또는 동등 점검)

5. Docker 기본 운영 명령 수행
- 이미지: 다운로드/목록 확인 (예: docker images)
- 컨테이너: 실행/중지/목록 확인 (예: docker ps, docker ps -a)
- 운영: 로그 확인 (예: docker logs), 리소스 확인 (예: docker stats)
- 수행 명령과 출력 결과를 기술 문서에 남긴다.

6. 컨테이너 실행 실습
- hello-world 실행 성공을 기록한다.
- ubuntu 컨테이너를 실행하고 내부 진입 후 간단 명령(예: ls, echo) 수행 결과를 기록한다.
- 컨테이너 종료/유지(attach/exec 등)의 차이를 스스로 관찰하고 간단히 정리한다.

7. 기존 Dockerfile 기반 커스텀 이미지 제작
    - 아래 방식 중 하나를 선택하여 기존 Dockerfile/이미지 기반의 커스텀 이미지를 만든다.
        - (A) 웹 서버 베이스 이미지 활용(예: NGINX/Apache 등) + 정적 콘텐츠/설정만 교체
        - (B) Linux 베이스 이미지(예: ubuntu/alpine 등) + 기본 기능(패키지/사용자/환경변수/헬스체크 등) 추가
- 제작 결과는 아래 조건을 만족해야 한다.
    - 커스텀 이미지 빌드 성공 및 컨테이너 실행 성공
    - 기술 문서에 다음을 포함한다.
        - 어떤 “기존 베이스(이미지/예시 Dockerfile)”를 선택했는지
        - 내가 적용한 커스텀 포인트 각각의 목적(간단 요약)
        - 빌드/실행 명령 + 핵심 결과(출력/스크린샷)

8. 포트 매핑 및 접속 증거
- 브라우저 접속 화면(또는 curl 응답)을 기술 문서에 첨부한다.

9. Docker 볼륨 영속성 검증
- Docker 볼륨을 생성하고 컨테이너에 연결한다.
- 컨테이너 삭제 전/후로 데이터를 확인하여 데이터가 유지됨을 증명한다.
- 기술 문서에 생성/연결/검증 절차(명령+출력)를 포함한다.

10. Git 설정 및 GitHub 연동
- Git 사용자 정보/기본 브랜치 설정을 완료하고 git config --list 결과를 기록한다.
- GitHub 로그인 및 저장소 연동을 완료하고, 연동 증거(스크린샷 등)를 기술 문서에 첨부한다.

11. 보안 및 개인정보 보호
- 기술 문서/로그/스크린샷에 토큰, 비밀번호, 개인키, 인증 코드 등이 포함되지 않도록 마스킹한다.
- 의심되는 민감정보가 노출된 경우, 즉시 히스토리/문서에서 제거하고 재발급 절차를 수행한다 (가능한 범위에서).

---


2) 수행 항목 체크리스트
- [x] 터미널 기본 조작 및 실행환경 확인
- [] 권한 변경 실습
- [] Docker 설치/점검
- [] hello-world 실행
- [] Dockerfile 빌드/실행
- [] 포트 매핑 접속(2회)
- [] 바인드 마운트 반영
- [] 볼륨 영속성
- [] Git 설정 + VSCode GitHub 연동


---


3) 실행환경
- OS: macOS 15.7.4
- orb: Version: 2.0.5
- Docker: Docker version 28.5.2, build ecc6942
- Git: git version 2.53.0