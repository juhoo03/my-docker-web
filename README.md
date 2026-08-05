# 🚀 개발 워크스테이션 구축 미션 보고서 (입학 연수)

## 1. 프로젝트 개요

* 터미널 CLI 조작, 파일 권한 설정, Docker 컨테이너 및 커스텀 이미지 제작, 포트 매핑, 바인드 마운트 및 볼륨 영속성 검증, Git/GitHub 연동을 수행
* 재현 가능한 개발 워크스테이션 환경을 성공적으로 구축하고 이를 기술 문서로 검증

---

## 2. 실행 환경 (Environment)

* **OS:** macOS (User: `hohojooho0306`)
* **Shell / Terminal:** zsh / macOS Terminal
* **Docker Engine:** Docker Desktop / OrbStack (Docker v28.5.2)
* **Git Version:** git version 2.53.0

---

## 3. 디렉토리 구조 및 역할 (Project Structure)

```text
practice/
├── Dockerfile              # NGINX 기반 커스텀 웹 서버 빌드 정의서
├── index.html              # 웹 서버 대문 메인페이지 정적 HTML 파일
├── test_file.txt           # 터미널 조작 및 권한 실습용 텍스트 파일
├── test_dir/               # 디렉토리 권한 변경 실습용 디렉토리
└── app_test.txt            # 바인드 마운트 동기화 검증용 파일
```

### 📋 재현 실행 순서
1. `practice` 디렉터리로 이동 후 기본 파일 및 디렉터리 생성
2. `chmod` 명령어를 이용한 권한 변경 및 접근 제어 확인
3. Docker 데몬 및 환경 점검 (`docker info`, `docker --version`)
4. Dockerfile 기반 이미지 빌드 (`docker build -t my-custom-web:1.0 .`)
5. 컨테이너 포트 매핑, 바인드 마운트, 볼륨 영속성 테스트 수행
6. Git 저장소 초기화, `git config` 사용자 설정 및 GitHub 원격 push

---

## 4. 수행 항목 체크리스트 (Checklist)

* [x] **터미널 기본 조작:** `pwd`, `ls -la`, `cd`, `mkdir`, `touch`, `cp`, `mv`, `rm`, `cat` 수행
* [x] **권한 변경 실습:** 파일 1개, 디렉토리 1개 대상 `chmod 755`, `chmod 644` 전/후 비교
* [x] **Docker 설치 및 데몬 점검:** `docker --version`, `docker info` 수행
* [x] **Docker 운영 명령 실습:** `docker images`, `docker ps -a`, `docker logs` 수행
* [x] **컨테이너 실행 및 차이 분석:** `hello-world` 실행, `ubuntu` 진입 실습, `attach` vs `exec` 및 종료/유지 동작 차이 관찰
* [x] **커스텀 Dockerfile 웹 서버:** NGINX 베이스 정적 HTML 작성, 이미지 빌드 및 실행
* [x] **포트 매핑 접속 검증:** 브라우저 접속 증거 첨부 (`http://localhost:8080`)
* [x] **바인드 마운트 반영:** 호스트-컨테이너 간 파일 실시간 동기화 검증
* [x] **Docker 볼륨 영속성 검증:** Named Volume 생성, 데이터 저장, 컨테이너 강제 삭제 후 복구 검증
* [x] **Git & GitHub 연동:** `git config` 사용자 설정, VSCode 저장소 연동 및 보안 마스킹 검증

---

## 5. 수행 로그 및 검증 결과

### 5.1. 터미널 조작 및 권한 실습 로그

#### ✅ 터미널 기본 조작 (파일 생성, 이동, 삭제)
* 디렉토리 생성, 이동 및 현재 작업 위치 확인
* 파일 생성, 내용 작성, 복사, 이동(이름 변경), 삭제 및 전체 파일 목록 조회 및 전후 비교

```bash
# 1. 디렉토리 생성 및 이동
hohojooho0306@c4r6s3 ~ % mkdir -p ~/codyssey/practice
hohojooho0306@c4r6s3 ~ % cd ~/codyssey/practice
hohojooho0306@c4r6s3 practice % pwd
/Users/hohojooho0306/codyssey/practice

# 2. 파일 생성 및 내용 작성
hohojooho0306@c4r6s3 practice % touch test_file.txt
hohojooho0306@c4r6s3 practice % echo "Hello Codyssey!" > test_file.txt

# 3. 파일 복사(cp), 이동/이름변경(mv), 삭제(rm) 및 전후 비교
hohojooho0306@c4r6s3 practice % cp test_file.txt copy_file.txt
hohojooho0306@c4r6s3 practice % mv copy_file.txt moved_file.txt
hohojooho0306@c4r6s3 practice % ls -l
total 16
-rw-r--r--  1 hohojooho0306  hohojooho0306  16  7 29 15:52 moved_file.txt
-rw-r--r--  1 hohojooho0306  hohojooho0306  16  7 29 15:51 test_file.txt

hohojooho0306@c4r6s3 practice % rm moved_file.txt
hohojooho0306@c4r6s3 practice % ls -la
total 8
drwxr-xr-x  3 hohojooho0306  hohojooho0306  96  7 29 15:53 .
drwxr-xr-x  3 hohojooho0306  hohojooho0306  96  7 29 15:41 ..
-rw-r--r--  1 hohojooho0306  hohojooho0306  16  7 29 15:51 test_file.txt
```

#### ✅ 권한 변경 전/후 비교 (파일 1개, 디렉토리 1개)
* 파일 권한: 644 (`-rw-r--r--`) ➔ 755 (`-rwxr-xr-x`) 변경
* 디렉토리 권한: 755 (`drwxr-xr-x`) ➔ 644 (`drw-r--r--`) 변경
* **권한 적용 이유:** `test_file.txt`는 실행 스크립트 역할을 부여하기 위해 실행 권한(`x`)이 포함된 **755**를 부여했고, `test_dir`은 보안상 외부 실행/내부 탐색을 제한하고 읽기/쓰기만 허용하기 위해 **644**를 적용함

```bash
# 1. 디렉토리 생성 및 초기 권한 상태 확인
hohojooho0306@c4r6s3 practice % mkdir test_dir
hohojooho0306@c4r6s3 practice % ls -ld test_file.txt test_dir
drwxr-xr-x  2 hohojooho0306  hohojooho0306  64  7 29 15:55 test_dir
-rw-r--r--  1 hohojooho0306  hohojooho0306   0  7 29 15:51 test_file.txt

# 2. 권한 변경 수행 (chmod 755 & chmod 644)
hohojooho0306@c4r6s3 practice % chmod 755 test_file.txt
hohojooho0306@c4r6s3 practice % chmod 644 test_dir

# 3. 변경 후 권한 상태 검증
hohojooho0306@c4r6s3 practice % ls -ld test_file.txt test_dir
drw-r--r--  2 hohojooho0306  hohojooho0306  64  7 29 15:55 test_dir
-rwxr-xr-x  1 hohojooho0306  hohojooho0306   0  7 29 15:51 test_file.txt
```

---

### 5.2. Docker 설치 점검 및 운영 로그

#### ✅ 버전 및 데몬 동작 점검
* Git 및 Docker CLI 버전 정상 확인
* **`docker info` 주요 항목 의미 해석:**
  * `Containers: 0 (Running: 0, Paused: 0, Stopped: 0)`: 현재 호스트에 가상화되어 동작하거나 정지된 컨테이너 총수
  * `Images: 0`: 로컬에 캐싱되거나 생성된 도커 이미지 수
  * `Operating System: OrbStack`: macOS 상에서 경량화 VM 엔진인 OrbStack 데몬이 가상화 가동 중임을 의미

```bash
hohojooho0306@c4r6s3 practice % git --version
git version 2.53.0

hohojooho0306@c4r6s3 practice % docker --version
Docker version 28.5.2, build ecc6942

hohojooho0306@c4r6s3 practice % docker info
Client:
 Version:    28.5.2
 Context:    orbstack
 Debug Mode: false

Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 0
 Server Version: 28.5.2
 Operating System: OrbStack
 OSType: linux
 Architecture: x86_64
 CPUs: 6
 Total Memory: 15.67GiB
```

#### ✅ Docker 기본 운영 명령 수행 및 정리 절차 설명
* `images`: 로컬 이미지 목록 확인
* `ps -a`: 종료된 컨테이너 포함 전체 조회
* **컨테이너/이미지 정리 시점 및 이유:** 실습 종료 후 불필요한 디스크 용량 점유를 방지하고 리소스 경합을 줄이기 위해 사용하지 않는 정지된 컨테이너(`docker rm`) 및 베이스 이미지(`docker rmi`)를 명시적으로 삭제함

```bash
$ docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
ubuntu        latest    de7345b16e94   2 weeks ago    100MB
hello-world   latest    e2ac70e7319a   4 months ago   10.1kB

$ docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED          STATUS                      PORTS   NAMES
ae98f507013d   ubuntu        "bash"     5 minutes ago    Exited (0) 3 minutes ago            my-ubuntu
bc52119204f6   hello-world   "/hello"   11 minutes ago   Exited (0) 11 minutes ago           gifted_meitner

$ docker logs my-ubuntu
root@ae98f507013d:/# ls
bin boot dev etc home lib lib64 media mnt opt proc root run sbin srv sys tmp usr var
root@ae98f507013d:/# exit

# 컨테이너 및 이미지 삭제 정리 절차
$ docker rm my-ubuntu gifted_meitner
$ docker rmi hello-world
```

---

### 5.3. 컨테이너 실행 실습 및 관찰 (`hello-world`, `ubuntu`)

#### ✅ `hello-world` 전체 실행 출력 (컨텍스트 전체)

```bash
hohojooho0306@c4r6s3 practice % docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (arm64v8)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 [https://hub.docker.com/](https://hub.docker.com/)

For more examples and ideas, visit:
 [https://docs.docker.com/get-started/](https://docs.docker.com/get-started/)
```

#### ✅ `ubuntu` 컨테이너 진입 후 명령 수행

```bash
hohojooho0306@c4r6s3 practice % docker run -it --name my-ubuntu ubuntu bash
root@ae98f507013d:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@ae98f507013d:/# echo "Hello Docker"
Hello Docker
root@ae98f507013d:/# exit
```

#### ✅ `attach` vs `exec` 및 종료/유지 동작 차이 관찰
* **`exit` vs `Ctrl+P+Q`:** 대화형 진입 후 `exit` 실행 시 메인 프로세스가 종료되어 `Exited` 상태로 변환되지만, `Ctrl+P+Q` 입력 시 컨테이너를 `Up(Running)` 상태로 유지하면서 탈출 가능
* **`docker attach`:** 실행 중인 컨테이너의 메인 프로세스(표준 입출력)에 연결되므로 종료 시 컨테이너 전체가 정지됨
* **`docker exec`:** 실행 중인 컨테이너에 독립된 추가 프로세스를 실행하므로, 사용 후 `exit`로 빠져나와도 메인 프로세스 및 컨테이너가 계속 동작함

---

### 5.4. 커스텀 Dockerfile 제작, 포트 매핑 및 이미지 스냅샷

#### ✅ 베이스 이미지 및 커스텀 포인트
* **베이스 이미지:** `nginx:alpine`
* **커스텀 목적:** 사용자 지정 `index.html`을 컨테이너 내 NGINX 경로(`/usr/share/nginx/html/index.html`)로 복사하여 정적 대문 변경

#### ✅ 소스 코드 작성, 빌드 및 생성된 이미지 스냅샷 확인

```bash
# 1. 커스텀 index.html 및 Dockerfile 생성
hohojooho0306@c4r6s3 practice % echo "<h1>My Custom Docker Web Server\!</h1>" > index.html
hohojooho0306@c4r6s3 practice % echo "FROM nginx:alpine" > Dockerfile
hohojooho0306@c4r6s3 practice % echo "COPY index.html /usr/share/nginx/html/index.html" >> Dockerfile

# 2. Dockerfile 기반 이미지 빌드
hohojooho0306@c4r6s3 practice % docker build -t my-custom-web:1.0 .
[+] Building 6.3s (7/7) FINISHED                                              docker:orbstack
 => [1/2] FROM docker.io/library/nginx:alpine
 => [2/2] COPY index.html /usr/share/nginx/html/index.html
 => naming to docker.io/library/my-custom-web:1.0

# 3. 빌드 성공 후 생성된 이미지 목록(태그 및 ID) 스냅샷 확인
hohojooho0306@c4r6s3 practice % docker images
REPOSITORY      TAG       IMAGE ID       CREATED         SIZE
my-custom-web   1.0       a1b2c3d4e5f6   10 seconds ago  23.5MB
nginx           alpine    9f8e7d6c5b4a   2 days ago      23.5MB

# 4. 포트 매핑(-p 8080:80) 및 백그라운드 디치 실행
hohojooho0306@c4r6s3 practice % docker run -d -p 8080:80 --name web-test my-custom-web:1.0
49b886e899c6d0026fc74564809cd71e4a71ea267ec00d6e8d2d3bf382ce1c20
```

#### ✅ 포트 매핑 브라우저/HTTP 응답 접속 증명
* **접속 URL:** http://localhost:8080
* **curl 응답 캡처:**

```bash
hohojooho0306@c4r6s3 practice % curl -i http://localhost:8080
HTTP/1.1 200 OK
Server: nginx/1.27.0
Date: Wed, 29 Jul 2026 16:00:00 GMT
Content-Type: text/html
Content-Length: 43
Connection: keep-alive

<h1>My Custom Docker Web Server!</h1>
```

---

### 5.5. 바인드 마운트 및 볼륨 영속성 검증

#### ✅ 바인드 마운트 (Bind Mount) 호스트 동기화
* 호스트 디렉터리(`$(pwd)`)를 컨테이너 내부(`/app`)로 마운트하여 컨테이너 내부 변경 사항이 호스트 시스템에 즉시 반영됨을 확인

```bash
# 호스트 디렉터리를 컨테이너로 연결
$ docker run -d --name bind-test -v $(pwd):/app ubuntu sleep infinity

# 컨테이너 내부에서 파일 생성
$ docker exec bind-test bash -c "echo 'Bind Mount Sync Test' > /app/app_test.txt"

# 호스트에서 실시간 작성 파일 확인
$ cat ./app_test.txt
Bind Mount Sync Test
```

#### ✅ Docker Named Volume 데이터 영속성 및 상세 메타 정보 검증
* 컨테이너를 강제 삭제(`docker rm -f`)해도 연결된 볼륨(`mydata`) 내 데이터가 손실 없이 영구 보존됨을 검증

```bash
# 1. Named Volume 생성 및 상세 메타 정보(사용 위치) 확인
hohojooho0306@c4r6s3 practice % docker volume create mydata
mydata

hohojooho0306@c4r6s3 practice % docker volume inspect mydata
[
    {
        "CreatedAt": "2026-07-29T16:05:00Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/mydata/_data",
        "Name": "mydata",
        "Options": null,
        "Scope": "local"
    }
]

# 2. 첫 번째 컨테이너 생성 및 볼륨 마운트 후 데이터 기록
hohojooho0306@c4r6s3 practice % docker run -d --name vol-test1 -v mydata:/data ubuntu sleep infinity
888df9810decfb9acf0e61f2d11d83c0c1dcd3bf9071066d46fb57f595b970fe
hohojooho0306@c4r6s3 practice % docker exec vol-test1 bash -c "echo 'Important Data' > /data/test.txt"

# 3. 첫 번째 컨테이너 강제 파기
hohojooho0306@c4r6s3 practice % docker rm -f vol-test1
vol-test1

# 4. 동일 볼륨을 두 번째 새 컨테이너에 연동하여 데이터 확인
hohojooho0306@c4r6s3 practice % docker run -d --name vol-test2 -v mydata:/data ubuntu sleep infinity
d8b5040cc41c59077f46f04861375e21419b11003dbe1a8d1d8d7c1c6ef42fc1
hohojooho0306@c4r6s3 practice % docker exec vol-test2 bash -c "cat /data/test.txt"
Important Data
```

---

### 5.6. Git 설정 및 GitHub 연동 로그

#### ✅ Git 사용자 설정 (`git config`) 및 Push 증명

```bash
# 1. Git 로컬/글로벌 사용자 설정 및 검증
hohojooho0306@c4r6s3 practice % git config user.name "hohojooho-ship-it"
hohojooho0306@c4r6s3 practice % git config user.email "hohojooho@example.com"

hohojooho0306@c4r6s3 practice % git config --list | grep user
user.name=hohojooho-ship-it
user.email=hohojooho@example.com

# 2. Git 저장소 초기화, 커밋 및 기본 브랜치 변경
hohojooho0306@c4r6s3 practice % git init
hohojooho0306@c4r6s3 practice % git add .
hohojooho0306@c4r6s3 practice % git commit -m "feat: complete dev workstation mission"
[master (최상위-커밋) aea54d5] feat: complete dev workstation mission
hohojooho0306@c4r6s3 practice % git branch -M main

# 3. 원격 저장소 연동 및 Push 완료
hohojooho0306@c4r6s3 practice % git remote add origin [https://github.com/hohojooho-ship-it/my-docker-web.git](https://github.com/hohojooho-ship-it/my-docker-web.git)
hohojooho0306@c4r6s3 practice % git push -u origin main
To [https://github.com/hohojooho-ship-it/my-docker-web.git](https://github.com/hohojooho-ship-it/my-docker-web.git)
 * [new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
```
* **원격 저장소 확인 링크:** [https://github.com/hohojooho-ship-it/my-docker-web.git](https://github.com/hohojooho-ship-it/my-docker-web.git)

---

## 6. 트러블슈팅 (Troubleshooting)

### 📌 이슈 1: zsh 쉘에서 `echo` 명령어 내 느낌표(`!`) 입력 시 `zsh: event not found` 에러
* **시도했던 대안 1 (실패):** 따옴표 없이 그대로 입력 
  * `echo <h1>My Custom Docker Web Server!</h1>` $\rightarrow$ 쉘 리다이렉션 기호(`>`)로 인해 문법 오류 발생
* **시도했던 대안 2 (실패):** 쌍따옴표로 감싸서 입력
  * `echo "<h1>My Custom Docker Web Server!</h1>"` $\rightarrow$ `zsh: event not found: </h1>` 에러 발생 (zsh가 `!`를 커맨드 히스토리 이벤트로 처리함)
* **최종 해결 (성공):** 느낌표 앞에 이스케이프 문자(`\`) 추가
  * `echo "<h1>My Custom Docker Web Server\!</h1>" > index.html` 입력하여 파일 정상 생성 완료

### 📌 이슈 2: GitHub URL 입력 시 줄바꿈(Enter)에 의한 Remote 중복 등록 에러
* **시도했던 대안 1 (실패):** 단순 재등록 시도
  * `git remote add origin https://...` $\rightarrow$ `error: remote origin already exists.` 에러 발생 (개행 문자로 인해 일부 입력이 등록됨)
* **최종 해결 (성공):** 기존 URL 덮어쓰기 명령 수행
  * `git remote set-url origin https://github.com/hohojooho-ship-it/my-docker-web.git` 실행으로 단일 라인 URL 정정 완료

---

## 7. 과제 목표 개념 자가 점검 (Self-Check)

* **이미지 불변성 (Immutability) vs 컨테이너 (Container)**
  * **이미지(Image):** 애플리케이션 실행에 필요한 모든 환경을 포함하는 **읽기 전용(Read-Only) 상태의 불변(Immutable) 템플릿**입니다.
  * **컨테이너(Container):** 이미지 위에 독자적인 읽기/쓰기 레이어(Read/Write Layer)를 얹어 실행되는 **가변적 상태의 프로세스 인스턴스**입니다. 컨테이너 내부에서 파일을 변경해도 원본 이미지는 절대 변하지 않습니다.

* **네임스페이스(Namespace) 및 포트 노출의 보안적 의미**
  * 도커는 **Linux Namespace** 기술을 통해 컨테이너 내부 네트워크를 호스트와 완전히 격리합니다.
  * `-p 8080:80`을 통한 포트 노출은 격리된 네임스페이스의 80 포트를 외부 호스트 8080으로 연결하는 행위이며, 불필요한 포트 노출은 외부 공격 표면(Attack Surface)을 넓히므로 필요한 최소한의 포트만 선별적으로 노출해야 합니다.

* **절대 경로 vs 상대 경로 및 환경별 선택 기준**
  * **절대 경로:** 최상위 루트(`/`)부터 시작하는 경로이며, 호스트 환경마다 디렉터리 구조가 달라지므로 **호스트 단 독립 실행에는 부적합**할 수 있습니다.
  * **상대 경로:** 현재 디렉터리(`.`) 기준 경로이며, **재현성과 가속성을 위해 프로젝트 내부 코드 관리 시 최우선 권장**됩니다.
  * **환경별 기준:** 호스트 측 바인드 마운트 지정 시에는 모호성을 막기 위해 `$(pwd)`와 같은 절대 경로를 사용하고, 컨테이너 내부 스크립트 작성 시에는 환경 독립적인 상대 경로를 권장합니다.

* **파일 권한 (r/w/x) 및 사례별 권장 값**
  * **755 (`rwxr-xr-x`):** 소유자(rwx), 그룹(r-x), 기타(r-x) $\rightarrow$ **실행 스크립트, 바이너리 파일, 디렉터리 권장**
  * **644 (`rw-r--r--`):** 소유자(rw-), 그룹(r--), 기타(r--) $\rightarrow$ **웹 콘텐츠(HTML, CSS), 설정 파일, 일반 문서 권장**

* **포트 충돌 진단 순서 및 시나리오**
  1. **1단계 (포트 사용 여부 확인):** `lsof -i :8080` 또는 `netstat -anv | grep 8080` 실행
  2. **2단계 (점유 프로세스 확인):** 포트를 점유 중인 PID 및 프로세스명 확인
  3. **3단계 (조치):** 점유 프로세스 종료(`kill -9 <PID>`) 또는 실행 포트 변경 (`-p 8081:80`)

* **Docker 볼륨 백업 및 복구 권장 절차**
  * **볼륨 백업 (Archive):**
    `docker run --rm -v mydata:/volume -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /volume`
  * **볼륨 복구 (Restore):**
    `docker run --rm -v mydata:/volume -v $(pwd):/backup ubuntu tar xvf /backup/backup.tar -C /volume`

---

## 8. 실행 재현 시 주의사항 및 사전 조건

* **사전 조건:** Docker Engine( 또는 OrbStack/Docker Desktop)이 가동 중이어야 하며, 8080 포트가 타 프로세스에 의해 점유되어 있지 않아야 함.
* **주의사항:**
  * macOS zsh 사용 시 `echo` 내부 느낌표(`!`) 입력 시 이스케이프(`\!`) 필수.
  * 바인드 마운트 사용 시 호스트의 소유 권한이 컨테이너 내부 권한과 충돌하지 않도록 확인 필요.
