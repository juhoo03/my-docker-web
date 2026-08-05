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

1. 디렉토리 생성 및 이동hohojooho0306@c4r6s3 ~ % mkdir -p ~/codyssey/practicehohojooho0306@c4r6s3 ~ % cd ~/codyssey/practicehohojooho0306@c4r6s3 practice % pwd/Users/hohojooho0306/codyssey/practice2. 파일 생성 및 내용 작성을 위한 touch & echohohojooho0306@c4r6s3 practice % touch test_file.txthohojooho0306@c4r6s3 practice % echo "Hello Codyssey!" > test_file.txt3. 파일 목록 조회 (권한 및 크기 확인)hohojooho0306@c4r6s3 practice % ls -latotal 0drwxr-xr-x  3 hohojooho0306  hohojooho0306  96  7 29 15:51 .drwxr-xr-x  3 hohojooho0306  hohojooho0306  96  7 29 15:41 ..-rw-r--r--  1 hohojooho0306  hohojooho0306   0  7 29 15:51 test_file.txt
✅ **권한 변경 전/후 비교 (파일 1개, 디렉토리 1개)**

* 파일 권한: `644` (`-rw-r--r--`) ➔ `755` (`-rwxr-xr-x`) 변경
* 디렉토리 권한: `755` (`drwxr-xr-x`) ➔ `644` (`drw-r--r--`) 변경

1. 디렉토리 생성 및 초기 권한 상태 확인hohojooho0306@c4r6s3 practice % mkdir test_dirhohojooho0306@c4r6s3 practice % ls -ld test_file.txt test_dirdrwxr-xr-x  2 hohojooho0306  hohojooho0306  64  7 29 15:55 test_dir-rw-r--r--  1 hohojooho0306  hohojooho0306   0  7 29 15:51 test_file.txt2. 권한 변경 수행 (chmod 755 & chmod 644)hohojooho0306@c4r6s3 practice % chmod 755 test_file.txthohojooho0306@c4r6s3 practice % chmod 644 test_dir3. 변경 후 권한 상태 검증hohojooho0306@c4r6s3 practice % ls -ld test_file.txt test_dirdrw-r--r--  2 hohojooho0306  hohojooho0306  64  7 29 15:55 test_dir-rwxr-xr-x  1 hohojooho0306  hohojooho0306   0  7 29 15:51 test_file.txt
---

### 4.2. Docker 설치 점검 및 운영 로그

✅ **버전 및 데몬 동작 점검**

* Git 및 Docker CLI 버전 정상 확인
* Docker Server Engine(OrbStack) 데몬 정상 실행 검증

hohojooho0306@c4r6s3 practice % git --versiongit version 2.53.0hohojooho0306@c4r6s3 practice % docker --versionDocker version 28.5.2, build ecc6942hohojooho0306@c4r6s3 practice % docker infoClient:Version:    28.5.2Context:    orbstackDebug Mode: falseServer:Containers: 0Running: 0Paused: 0Stopped: 0Images: 0Server Version: 28.5.2Operating System: OrbStackOSType: linuxArchitecture: x86_64CPUs: 6Total Memory: 15.67GiB
✅ **Docker 기본 운영 명령 수행 (`images`, `ps -a`, `logs`)**

* **images**: 로컬 이미지 목록 확인 (`ubuntu`, `hello-world`)
* **ps -a**: 종료된 컨테이너를 포함한 전체 컨테이너 이력 확인
* **logs**: 컨테이너 표준 출력 로그 점검

$ docker imagesREPOSITORY    TAG       IMAGE ID       CREATED        SIZEubuntu        latest    de7345b16e94   2 weeks ago    100MBhello-world   latest    e2ac70e7319a   4 months ago   10.1kB$ docker ps -aCONTAINER ID   IMAGE         COMMAND    CREATED          STATUS                      PORTS   NAMESae98f507013d   ubuntu        "bash"     5 minutes ago    Exited (0) 3 minutes ago            my-ubuntubc52119204f6   hello-world   "/hello"   11 minutes ago   Exited (0) 11 minutes ago           gifted_meitner$ docker logs my-ubunturoot@ae98f507013d:/# lsbin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  varroot@ae98f507013d:/# echo "Hello Docker"Hello Dockerroot@ae98f507013d:/# exit
---

### 4.3. 컨테이너 실행 실습 및 관찰 (`hello-world`, `ubuntu`)

✅ **hello-world 실행 결과**

* 이미지 다운로드 후 기본 메시지 정상 출력 검증

hohojooho0306@c4r6s3 practice % docker run hello-worldHello from Docker!This message shows that your installation appears to be working correctly.
✅ **ubuntu 컨테이너 진입 후 명령 수행**

* 대화형 터미널(`-it`) 진입 후 내부 CLI 명령 실행

hohojooho0306@c4r6s3 practice % docker run -it --name my-ubuntu ubuntu bashroot@ae98f507013d:/# lsbin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  varroot@ae98f507013d:/# echo "Hello Docker"Hello Dockerroot@ae98f507013d:/# exit
✅ **attach vs exec 및 종료/유지 동작 차이 관찰**

* **`exit` vs `Ctrl+P+Q`**: 대화형 진입 후 `exit` 실행 시 메인 프로세스가 종료되어 `Exited` 상태로 변환되지만, `Ctrl+P+Q` 입력 시 컨테이너를 `Up(Running)` 상태로 유지하면서 탈출 가능
* **`docker attach`**: 실행 중인 컨테이너의 메인 프로세스(표준 입출력)에 연결되므로 종료 시 컨테이너 전체가 정지됨
* **`docker exec`**: 실행 중인 컨테이너에 **독립된 추가 프로세스**를 실행하므로, 사용 후 `exit`로 빠져나와도 메인 프로세스 및 컨테이너가 계속 동작함

---

### 4.4. 커스텀 Dockerfile 제작 및 포트 매핑

✅ **베이스 이미지 및 커스텀 포인트**

* **베이스 이미지**: `nginx:alpine`
* **커스텀 목적**: 사용자 지정 `index.html`을 컨테이너 내 NGINX 경로(`/usr/share/nginx/html/index.html`)로 복사하여 정적 대문 변경

✅ **소스 코드 작성 및 빌드/실행**

1. 커스텀 index.html 및 Dockerfile 생성hohojooho0306@c4r6s3 practice % echo "My Custom Docker Web Server!" > index.htmlhohojooho0306@c4r6s3 practice % echo "FROM nginx:alpine" > Dockerfilehohojooho0306@c4r6s3 practice % echo "COPY index.html /usr/share/nginx/html/index.html" >> Dockerfile2. Dockerfile 기반 이미지 빌드hohojooho0306@c4r6s3 practice % docker build -t my-custom-web:1.0 .[+] Building 6.3s (7/7) FINISHED                                              docker:orbstack=> [1/2] FROM docker.io/library/nginx:alpine=> [2/2] COPY index.html /usr/share/nginx/html/index.html=> naming to docker.io/library/my-custom-web:1.03. 포트 매핑(-p 8080:80) 및 백그라운드 디치 실행hohojooho0306@c4r6s3 practice % docker run -d -p 8080:80 --name web-test my-custom-web:1.049b886e899c6d0026fc74564809cd71e4a71ea267ec00d6e8d2d3bf382ce1c20
✅ **포트 매핑 브라우저 접속 증거**

* **접속 URL**: `http://localhost:8080`
* **접속 성공 화면**:

![포트 매핑 브라우저 접속 화면](./port_mapping.png)

---

### 4.5. 바인드 마운트 및 볼륨 영속성 검증

✅ **바인드 마운트 (Bind Mount) 호스트 동기화**

* 호스트 디렉터리(`$(pwd)`)를 컨테이너 내부(`/app`)로 마운트하여 컨테이너 내부 변경 사항이 호스트 시스템에 즉시 반영됨을 확인

호스트 디렉터리를 컨테이너로 연결$docker run -d --name bind-test -v$(pwd):/app ubuntu sleep infinity컨테이너 내부에서 파일 생성$ docker exec bind-test bash -c "echo 'Bind Mount Sync Test' > /app/app_test.txt"호스트에서 실시간 작성 파일 확인$ cat ./app_test.txtBind Mount Sync Test
✅ **Docker Named Volume 데이터 영속성 검증**

* 컨테이너를 강제 삭제(`docker rm -f`)해도 연결된 볼륨(`mydata`) 내 데이터가 손실 없이 영구 보존됨을 검증

1. Named Volume 생성hohojooho0306@c4r6s3 practice % docker volume create mydatamydata2. 첫 번째 컨테이너 생성 및 볼륨 마운트 후 데이터 기록hohojooho0306@c4r6s3 practice % docker run -d --name vol-test1 -v mydata:/data ubuntu sleep infinity888df9810decfb9acf0e61f2d11d83c0c1dcd3bf9071066d46fb57f595b970fehohojooho0306@c4r6s3 practice % docker exec vol-test1 bash -c "echo 'Important Data' > /data/test.txt"3. 첫 번째 컨테이너 강제 파기hohojooho0306@c4r6s3 practice % docker rm -f vol-test1vol-test14. 동일 볼륨을 두 번째 새 컨테이너에 연동하여 데이터 확인hohojooho0306@c4r6s3 practice % docker run -d --name vol-test2 -v mydata:/data ubuntu sleep infinityd8b5040cc41c59077f46f04861375e21419b11003dbe1a8d1d8d7c1c6ef42fc1hohojooho0306@c4r6s3 practice % docker exec vol-test2 bash -c "cat /data/test.txt"Important Data
---

### 4.6. Git 설정 및 GitHub 연동 로그

✅ **Git 저장소 초기화 및 커밋 / Remote 설정**

* 기본 브랜치를 `main`으로 변경 후 원격 저장소(`origin`) 연동 완료

1. Git 저장소 초기화 및 전체 파일 커밋hohojooho0306@c4r6s3 practice % git inithohojooho0306@c4r6s3 practice % git add .hohojooho0306@c4r6s3 practice % git commit -m "feat: complete dev workstation mission"[master (최상위-커밋) aea54d5] feat: complete dev workstation mission2. 기본 브랜치 이름 변경 (master -> main)hohojooho0306@c4r6s3 practice % git branch -M main3. Remote URL 설정 및 Pushhohojooho0306@c4r6s3 practice % git remote set-url origin https://github.com/hohojooho-ship-it/my-docker-web.githohojooho0306@c4r6s3 practice % git push -u origin main
---

## 5. 트러블슈팅 (Troubleshooting)

📌 **이슈 1: zsh 쉘에서 `echo` 명령어 내 느낌표(`!`) 입력 시 `zsh: event not found` 에러**

* **문제 (Problem)**: `echo "<h1>My Custom Docker Web Server!</h1>"` 입력 시 `zsh: event not found: </h1>` 오류가 발생하며 파일 저장 실패
* **원인 가설 (Hypothesis)**: zsh 쉘이 쌍따옴표 내부의 `!`를 히스토리 검색 이벤트 키워드로 인식함
* **해결 (Solution)**: 느낌표 앞에 이스케이프 문자를 붙여 `\!` 형태로 작성 (`echo "<h1>My Custom Docker Web Server\!</h1>" > index.html`)하여 정상 파일 생성 완료

📌 **이슈 2: GitHub URL 입력 시 줄바꿈(Enter)에 의한 Remote 중복 등록 에러**

* **문제 (Problem)**: `git remote add origin` 명령어 실행 중 URL에 개행 문자가 들어가면서 `zsh: no such file or directory` 및 `error: origin 리모트가 이미 있습니다` 에러 발생
* **원인 가설 (Hypothesis)**: 불완전하게 추가된 `origin` 이름이 이미 저장소 목록에 등록된 상태임
* **해결 (Solution)**: `git remote set-url origin https://github.com/hohojooho-ship-it/my-docker-web.git` 명령어를 통해 단일 라인 URL로 정확히 갱신 및 연동 완료

---

## 6. 과제 목표 개념 자가 점검 (Self-Check)

✅ **절대 경로 vs 상대 경로**

* **절대 경로**: 최상위 루트(`/`)부터 파일 전체 주소를 지정 (예: `/Users/hohojooho0306/codyssey/practice`)
* **상대 경로**: 현재 작업 디렉토리(`.`) 기준으로 파일 위치를 지정 (예: `./index.html`)

✅ **파일 권한 (r/w/x) 및 755 / 644 해석**

* **755 (`rwxr-xr-x`)**: 소유자(rwx: 읽기/쓰기/실행), 그룹(r-x: 읽기/실행), 기타 사용자(r-x: 읽기/실행)
* **644 (`rw-r--r--`)**: 소유자(rw-: 읽기/쓰기), 그룹(r--: 읽기), 기타 사용자(r--: 읽기)

✅ **포트 매핑의 필요성**

* 격리된 컨테이너 내부 네트워크 포트(80)를 외부 호스트 포트(8080)에 마운트(`-p 8080:80`)해야 브라우저에서 `localhost:8080`으로 서비스 접근이 가능함

✅ **Docker 볼륨 영속성 (Data Persistence)**

* 컨테이너 라이프사이클과 독립된 호스트 저장 공간을 마련하여, 컨테이너가 파기되더라도 내부 데이터를 보존하는 기술

✅ **Git vs GitHub의 역할 차이**

* **Git**: 로컬 컴퓨터에서 변경 이력을 분산 관리하는 CLI 버전 관리 프로그램
* **GitHub**: Git 이력을 원격에 저장하고 공유 및 협업할 수 있게 해주는 클라우드 플랫폼
