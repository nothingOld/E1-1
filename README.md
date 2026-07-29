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
    - 