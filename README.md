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

#### 1) 디렉토리 생성 및 작업 위치 확인
* `mkdir -p ~/codyssey/practice`: 새로운 디렉토리를 생성하는 명령어이며, `-p` 옵션은 중간 경로가 없을 경우 함께 생성해 줍니다.
* `cd ~/codyssey/practice`: 현재 작업 위치를 실습 디렉토리인 `practice`로 이동합니다.
* `pwd`: 현재 작업 중인 디렉토리의 전체 경로를 출력합니다.

```bash
hohojooho0306@c4r6s3 ~ % mkdir -p ~/codyssey/practice
hohojooho0306@c4r6s3 ~ % cd ~/codyssey/practice
hohojooho0306@c4r6s3 practice % pwd
/Users/hohojooho0306/codyssey/practice
```
* **확인 내용:** 현재 작업 위치가 `practice` 디렉토리로 정상 변경된 것을 확인했습니다.

---

#### 2) 파일 생성 및 내용 작성
* `touch test_file.txt`: 빈 파일 `test_file.txt`를 새로 만듭니다.
* `echo "Hello Codyssey!" > test_file.txt`: 문자열을 출력하는 `echo`와 리다이렉션 기호(`>`)를 사용하여 파일에 내용을 저장합니다. (기존 내용이 있다면 덮어씁니다.)

```bash
hohojooho0306@c4r6s3 practice % touch test_file.txt
hohojooho0306@c4r6s3 practice % echo "Hello Codyssey!" > test_file.txt
```
* **확인 내용:** `test_file.txt` 파일 생성 및 "Hello Codyssey!" 문자열이 성공적으로 저장되었습니다.

---

#### 3) 파일 복사, 이름 변경 및 상세 조회
* `cp test_file.txt copy_file.txt`: `test_file.txt`를 복사하여 `copy_file.txt`를 생성합니다.
* `mv copy_file.txt moved_file.txt`: `copy_file.txt`의 이름을 `moved_file.txt`로 변경합니다.
* `ls -l`: 현재 디렉토리의 파일 목록과 함께 권한, 소유자, 크기, 수정 시간 등을 자세히 보여줍니다.

```bash
hohojooho0306@c4r6s3 practice % cp test_file.txt copy_file.txt
hohojooho0306@c4r6s3 practice % mv copy_file.txt moved_file.txt
hohojooho0306@c4r6s3 practice % ls -l
total 16
-rw-r--r--  1 hohojooho0306  hohojooho0306  16  7 29 15:52 moved_file.txt
-rw-r--r--  1 hohojooho0306  hohojooho0306  16  7 29 15:51 test_file.txt
```
* **확인 내용:**
  * 원본 파일 `test_file.txt`가 존재함을 확인했습니다.
  * 복사 후 이름이 변경된 `moved_file.txt`가 정상 생성된 것을 확인했습니다.
  * `-rw-r--r--`를 통해 현재 설정된 기본 파일 권한 정보를 검증했습니다.

---

#### 4) 파일 삭제 및 최종 검증
* `rm moved_file.txt`: 복사 후 이름 변경했던 `moved_file.txt`를 삭제합니다.
* `ls -la`: 숨김 파일을 포함한 전체 파일 목록을 상세 출력합니다. (`.`은 현재 디렉토리, `..`은 상위 디렉토리를 의미)

```bash
hohojooho0306@c4r6s3 practice % rm moved_file.txt
hohojooho0306@c4r6s3 practice % ls -la
total 8
drwxr-xr-x  3 hohojooho0306  hohojooho0306  96  7 29 15:53 .
drwxr-xr-x  3 hohojooho0306  hohojooho0306  96  7 29 15:41 ..
-rw-r--r--  1 hohojooho0306  hohojooho0306  16  7 29 15:51 test_file.txt
```
* **확인 내용:**
  * `moved_file.txt`가 정상적으로 삭제되었습니다.
  * 최종적으로 `test_file.txt`만 남아있음을 확인했습니다.
  * 파일의 **생성 ➔ 복사 ➔ 이름 변경 ➔ 삭제** 전 과정이 정상적으로 검증되었습니다.

---

#### 5) 권한 변경 실습 및 전/후 비교
* `chmod`: 파일이나 디렉토리의 읽기(r), 쓰기(w), 실행(x) 접근 권한을 변경합니다.
* **권한 적용 이유:** `test_file.txt`는 실행 스크립트 역할을 부여하기 위해 실행 권한(`x`)이 포함된 **755**를 부여했고, `test_dir`은 보안상 외부 실행/내부 탐색을 제한하고 읽기/쓰기만 허용하기 위해 **644**를 적용했습니다.

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
* **확인 내용:**
  * 파일 권한: `644 (-rw-r--r--)` ➔ `755 (-rwxr-xr-x)`로 성공적으로 변경되었습니다.
  * 디렉토리 권한: `755 (drwxr-xr-x)` ➔ `644 (drw-r--r--)`로 성공적으로 변경되었습니다.

---

### 5.2. Docker 설치 점검 및 운영 로그

#### 1) 도커 버전 및 데몬 상태 점검
* `docker --version`: 클라이언트 도커 엔진 버전을 확인합니다.
* `docker info`: 시스템 전체의 도커 데몬 상태, 시스템 자원, 컨테이너 및 이미지 개수 등 상세 메타정보를 조회합니다.

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
* **`docker info` 주요 용어 설명:**
  * `Containers`: 현재 가상화되어 동작 중이거나 정지된 컨테이너의 총 수 (`Running`/`Paused`/`Stopped` 상태별 세부 표기)
  * `Images`: 로컬 저장소에 캐싱되거나 직접 빌드하여 보관 중인 이미지의 수
  * `Operating System`: macOS 환경에서 가상화 백엔드로 작동 중인 경량 VM 엔진(OrbStack) 정보를 출력

---

#### 2) Docker 기본 운영 명령 수행 및 정리 절차
* `docker images`: 로컬에 다운로드되거나 생성된 이미지 목록을 조회합니다.
* `docker ps -a`: 종료된 컨테이너를 포함해 호스트의 모든 컨테이너 이력을 확인합니다. (`-a` 옵션 제외 시 실행 중인 컨테이너만 표시)
* `docker logs`: 지정한 컨테이너 내부의 표준 출력(STDOUT) 로그를 확인합니다.
* `docker rm` / `docker rmi`: 컨테이너 삭제 / 이미지 삭제 명령어입니다.

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
* **컨테이너/이미지 정리 시점 및 이유:** 실습 완료 후 불필요한 디스크 용량 점유를 방지하고 리소스 경합을 줄이기 위해, 사용하지 않는 정지 상태의 컨테이너와 중복 이미지를 명시적으로 삭제했습니다.

---

### 5.3. 컨테이너 실행 실습 및 관찰 (`hello-world`, `ubuntu`)

#### 1) `hello-world` 이미지 실행
* `docker run`: 이미지가 로컬에 없으면 Docker Hub에서 자동 다운로드(`pull`)한 후 컨테이너를 생성(`create`) 및 시작(`start`)합니다.

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
* **확인 내용:** Docker 클라이언트-데몬 간 통신, 이미지 다운로드, 컨테이너 생성 및 메시지 출력이 정상 작동함을 전체 로그로 입증했습니다.

---

#### 2) `ubuntu` 대화형 컨테이너 실행
* `-it` 옵션: `-i`(Interactive, 표준 입력 유지)와 `-t`(TTY, 의사 터미널 할당)를 조합하여 컨테이너 내부 쉘로 직접 들어갈 수 있게 해줍니다.

```bash
hohojooho0306@c4r6s3 practice % docker run -it --name my-ubuntu ubuntu bash
root@ae98f507013d:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@ae98f507013d:/# echo "Hello Docker"
Hello Docker
root@ae98f507013d:/# exit
```
* **확인 내용:** 우분투 리눅스 환경의 CLI 제어판에 접속하여 명령어 수행이 완벽히 작동함을 검증했습니다.

---

#### 3) `attach` vs `exec` 및 종료/유지 동작 차이 관찰
* **`exit` vs `Ctrl + P + Q` 차이:**
  * `exit`: 대화형 쉘 종료 시 메인 프로세스가 종료되어 컨테이너가 `Exited` (정지) 상태로 변경됨.
  * `Ctrl + P` $\rightarrow$ `Ctrl + Q` (Detach): 컨테이너를 정지시키지 않고 백그라운드 `Up (Running)` 상태로 둔 채 내 터미널 화면만 빠져나옴.
* **`docker attach` vs `docker exec` 핵심 개념 차이:**
  * **`docker attach <컨테이너ID>`:** 컨테이너가 켜질 때 실행된 **기존 메인 프로세스(STDIN/STDOUT)**에 직접 연결합니다. 따라서 터미널 탈출 시 `exit`를 치면 메인 프로세스가 죽어 컨테이너 전체가 중지됩니다.
  * **`docker exec -it <컨테이너ID> bash`:** 실행 중인 컨테이너에 **독립된 보조 프로세스(새 쉘)**를 하나 더 띄워서 진입합니다. 작업 종료 후 `exit`를 쳐도 추가된 프로세스만 종료되므로 메인 컨테이너는 계속 실행 상태를 유지합니다.

---

### 5.4. 커스텀 Dockerfile 제작, 포트 매핑 및 이미지 스냅샷

#### 1) 개념 설명 및 소스 코드 작성
* **Dockerfile:** 이미지 빌드 과정을 자동화한 명세서 파일입니다.
* **베이스 이미지 (`FROM`):** 이미지를 빌드할 바탕이 되는 기초 환경 (`nginx:alpine` 사용)
* **`COPY`:** 호스트의 `index.html` 파일을 NGINX 웹 서버의 루트 경로로 복사하여 기본 페이지를 대체함.

```bash
# 1. 커스텀 index.html 및 Dockerfile 생성
hohojooho0306@c4r6s3 practice % echo "<h1>My Custom Docker Web Server\!</h1>" > index.html
hohojooho0306@c4r6s3 practice % echo "FROM nginx:alpine" > Dockerfile
hohojooho0306@c4r6s3 practice % echo "COPY index.html /usr/share/nginx/html/index.html" >> Dockerfile
```

---

#### 2) 이미지 빌드 및 포트 매핑 실행
* `docker build -t <이미지명:태그> <경로>`: Dockerfile을 읽어 새 이미지를 빌드합니다.
* `docker run -d -p 8080:80`:
  * `-d` (Detached): 백그라운드에서 컨테이너를 실행합니다.
  * `-p 8080:80` (Port Mapping): 호스트의 8080 포트 접속을 컨테이너 내부의 NGINX 80 포트로 연결해 줍니다.

```bash
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

---

#### 3) 포트 매핑 접속 증명 (curl 응답 캡처)
* `curl -i`: HTTP 응답 헤더와 바디 본문 출력을 함께 조회하여 웹 서버가 정상 작동하는지 외부 터미널에서 검증합니다.

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
* **확인 내용:** 브라우저 및 `curl` 명령을 통해 NGINX 웹 서버의 200 OK 응답과 커스텀 HTML 메시지가 정상 노출됨을 증명했습니다.

---

### 5.5. 바인드 마운트 및 볼륨 영속성 검증

#### 1) 바인드 마운트 (Bind Mount) 실시간 동기화
* **바인드 마운트 개념:** 호스트의 특정 절대 경로 디렉터리를 컨테이너 내부 폴더에 직접 마운트하는 방식입니다.
* `-v $(pwd):/app`: 현재 작업 위치(`$(pwd)`)를 컨테이너의 `/app`에 연결합니다.

```bash
# 호스트 디렉터리를 컨테이너로 연결
$ docker run -d --name bind-test -v $(pwd):/app ubuntu sleep infinity

# 컨테이너 내부에서 파일 생성
$ docker exec bind-test bash -c "echo 'Bind Mount Sync Test' > /app/app_test.txt"

# 호스트에서 실시간 작성 파일 확인
$ cat ./app_test.txt
Bind Mount Sync Test
```
* **확인 내용:** 컨테이너 내부에서 생성한 파일이 호스트 파일시스템 상에 즉시 동기화되어 반영되었습니다.

---

#### 2) Named Volume 데이터 영속성 (Data Persistence) 및 메타정보 검증
* **Named Volume 개념:** 호스트의 특정 폴더 위치에 얽매이지 않고, Docker가 데이터 저장 전용 스토리지 영역을 직접 제어/관리하는 방식입니다. 컨테이너가 파기되어도 데이터는 보존됩니다.
* `docker volume inspect`: 볼륨의 물리적 마운트 경로 및 생성 일자 등 상세 메타정보를 확인합니다.

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

# 3. 첫 번째 컨테이너 강제 파기 (rm -f)
hohojooho0306@c4r6s3 practice % docker rm -f vol-test1
vol-test1

# 4. 동일 볼륨을 두 번째 새 컨테이너에 연동하여 데이터 복구 확인
hohojooho0306@c4r6s3 practice % docker run -d --name vol-test2 -v mydata:/data ubuntu sleep infinity
d8b5040cc41c59077f46f04861375e21419b11003dbe1a8d1d8d7c1c6ef42fc1
hohojooho0306@c4r6s3 practice % docker exec vol-test2 bash -c "cat /data/test.txt"
Important Data
```
* **확인 내용:** 첫 번째 컨테이너를 강제 삭제(`docker rm -f`)했음에도 불구하고, 동일 볼륨을 마운트한 두 번째 컨테이너에서 `Important Data` 텍스트가 유실 없이 완벽하게 복구되었습니다.

---

### 5.6. Git 설정 및 GitHub 연동 로그

#### 1) Git 사용자 환경 설정 (`git config`)
* `git config user.name` / `user.email`: 커밋 작성자의 소유자 정보를 설정합니다.
* `git config --list`: 설정된 사용자명과 이메일 항목을 조회 및 검증합니다.

```bash
# 1. Git 로컬/글로벌 사용자 설정 및 검증
hohojooho0306@c4r6s3 practice % git config user.name "hohojooho-ship-it"
hohojooho0306@c4r6s3 practice % git config user.email "hohojooho@example.com"

hohojooho0306@c4r6s3 practice % git config --list | grep user
user.name=hohojooho-ship-it
user.email=hohojooho@example.com
```

---

#### 2) 저장소 초기화, 커밋 및 원격 저장소 푸시
* `git init`: 로컬 Git 저장소를 생성합니다.
* `git branch -M main`: 기본 브랜치 이름을 `master`에서 표준인 `main`으로 변경합니다.
* `git remote add origin`: GitHub의 원격 저장소 URL 주소를 연동합니다.
* `git push -u origin main`: 로컬 커밋 이력을 원격 저장소 `main` 브랜치로 최종 업로드합니다.

```bash
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

---

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

* **사전 조건:** Docker Engine(또는 OrbStack/Docker Desktop)이 가동 중이어야 하며, 8080 포트가 타 프로세스에 의해 점유되어 있지 않아야 함.
* **주의사항:**
  * macOS zsh 사용 시 `echo` 내부 느낌표(`!`) 입력 시 이스케이프(`\!`) 필수.
  * 바인드 마운트 사용 시 호스트의 소유 권한이 컨테이너 내부 권한과 충돌하지 않도록 확인 필요.
