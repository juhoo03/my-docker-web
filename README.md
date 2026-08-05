# 🚀 개발 워크스테이션 구축 미션 보고서 (입학 연수)

---

## 1. 프로젝트 개요

* 터미널 CLI 조작, 파일 권한 설정, Docker 컨테이너 및 커스텀 이미지 제작, 포트 매핑, 바인드 마운트 및 볼륨 영속성 검증, Git/GitHub 연동을 수행
* 재현 가능한 개발 워크스테이션 환경을 성공적으로 구축하고 이를 기술 문서로 검증

---

## 2. 실행 환경 (Environment)

* **OS**: macOS (User: `hohojooho0306`)
* **Shell / Terminal**: zsh / macOS Terminal
* **Docker Engine**: Docker Desktop / OrbStack (Docker v28.5.2)
* **Git Version**: git version 2.53.0

---

## 3. 수행 항목 체크리스트 (Checklist)

✅ **터미널 기본 조작**
* `pwd`, `ls -la`, `cd`, `mkdir`, `touch`, `cp`, `mv`, `rm`, `cat` 수행

✅ **권한 변경 실습**
* 파일 1개, 디렉토리 1개 대상 `chmod 755`, `chmod 644` 전/후 비교

✅ **Docker 설치 및 데몬 점검**
* `docker --version`, `docker info` 수행

✅ **Docker 운영 명령 실습**
* `docker images`, `docker ps -a`, `docker logs` 수행

✅ **컨테이너 실행 및 차이 분석**
* `hello-world` 실행, `ubuntu` 진입 실습, `attach` vs `exec` 및 종료/유지 동작 차이 관찰

✅ **커스텀 Dockerfile 웹 서버**
* NGINX 베이스 정적 HTML 작성, 이미지 빌드 및 실행

✅ **포트 매핑 접속 검증**
* 브라우저 접속 증거 첨부 (`http://localhost:8080`)

✅ **바인드 마운트 반영**
* 호스트-컨테이너 간 파일 실시간 동기화 검증

✅ **Docker 볼륨 영속성 검증**
* Named Volume 생성, 데이터 저장, 컨테이너 강제 삭제 후 복구 검증

✅ **Git & GitHub 연동**
* `git config` 사용자 설정, VSCode 저장소 연동 및 보안 마스킹 검증

---

## 4. 수행 로그 및 검증 결과

### 4.1. 터미널 조작 및 권한 실습 로그

✅ **터미널 기본 조작**

* 디렉토리 생성, 이동 및 현재 작업 위치 확인
* 파일 생성, 내용 작성, 복사, 이동(이름 변경), 삭제 및 전체 파일 목록 조회
