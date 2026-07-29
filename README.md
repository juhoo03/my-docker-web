# 🚀 개발 워크스테이션 구축 미션 보고서 (입학 연수)

## 1. 프로젝트 개요
터미널 CLI 조작, 파일 권한 설정, Docker 컨테이너 및 커스텀 이미지 제작, 포트 매핑, 바인드 마운트 및 볼륨 영속성 검증, Git/GitHub 연동을 수행하여 재현 가능한 개발 워크스테이션 환경을 성공적으로 구축하고 이를 기술 문서로 검증합니다.

---

## 2. 실행 환경 (Environment)
- **OS**: macOS (User: `hohojooho0306`)
    - **Shell / Terminal**: zsh / macOS Terminal
- **Docker Engine**: Docker Desktop / OrbStack (Docker v28.5.2)
- **Git Version**: git version 2.53.0

---

## 3. 수행 항목 체크리스트 (Checklist)
- [x] **터미널 기본 조작**: `pwd`, `ls -la`, `cd`, `mkdir`, `touch`, `cp`, `mv`, `rm`, `cat` 수행
- [x] **권한 변경 실습**: 파일 1개, 디렉토리 1개 대상 `chmod 755`, `chmod 644` 전/후 비교
- [x] **Docker 설치 및 데몬 점검**: `docker --version`, `docker info` 수행
- [x] **Docker 운영 명령 실습**: `docker images`, `docker ps -a`, `docker logs` 수행
- [x] **컨테이너 실행 및 차이 분석**: `hello-world` 실행, `ubuntu` 진입 실습, `attach` vs `exec` 및 종료/유지 동작 차이 관찰
- [x] **커스텀 Dockerfile 웹 서버**: NGINX 베이스 정적 HTML 작성, 이미지 빌드 및 실행
- [x] **포트 매핑 접속 검증**: 브라우저 접속 증거 첨부 (`http://localhost:8080`)
- [x] **바인드 마운트 반영**: 호스트-컨테이너 간 파일 실시간 동기화 검증
- [x] **Docker 볼륨 영속성 검증**: Named Volume 생성, 데이터 저장, 컨테이너 강제 삭제 후 복구 검증
- [x] **Git & GitHub 연동**: `git config` 사용자 설정, VSCode 저장소 연동 및 보안 마스킹 검증

---

## 4. 수행 로그 및 검증 결과

### 1) 터미널 조작 및 권한 실습 로그

#### [터미널 기본 조작]
```bash
# 1. 디렉토리 생성 및 현재 위치 확인
hohojooho0306@c4r6s3 ~ % mkdir -p ~/codyssey/practice
hohojooho0306@c4r6s3 ~ % cd ~/codyssey/practice
hohojooho0306@c4r6s3 practice % pwd
/Users/hohojooho0306/codyssey/practice

# 2. 파일 생성 및 내용 조작
hohojooho0306@c4r6s3 practice % touch test_file.txt
hohojooho0306@c4r6s3 practice % echo "Hello Codyssey!" > test_file.txt

# 3. 파일 복사, 이름 변경, 삭제 및 목록 확인
hohojooho0306@c4r6s3 practice % ls -la
total 0
drwxr-xr-x  3 hohojooho0306  hohojooho0306  96  7 29 15:51 .
drwxr-xr-x  3 hohojooho0306  hohojooho0306  96  7 29 15:41 ..
-rw-r--r--  1 hohojooho0306  hohojooho0306   0  7 29 15:51 test_file.txt



[권한 변경 전/후 비교 (파일 1개, 디렉토리 1개)]

# 디렉토리 생성 및 권한 변경 전 확인
hohojooho0306@c4r6s3 practice % mkdir test_dir
hohojooho0306@c4r6s3 practice % ls -ld test_file.txt test_dir
drwxr-xr-x  2 hohojooho0306  hohojooho0306  64  7 29 15:55 test_dir
-rw-r--r--  1 hohojooho0306  hohojooho0306   0  7 29 15:51 test_file.txt

# 권한 변경 실행 (파일: 755, 디렉토리: 644)
hohojooho0306@c4r6s3 practice % chmod 755 test_file.txt
hohojooho0306@c4r6s3 practice % chmod 644 test_dir

# 권한 변경 후 상태 확인
hohojooho0306@c4r6s3 practice % ls -ld test_file.txt test_dir
drw-r--r--  2 hohojooho0306  hohojooho0306  64  7 29 15:55 test_dir
-rwxr-xr-x  1 hohojooho0306  hohojooho0306   0  7 29 15:51 test_file.txt


2) Docker 설치 점검 및 운영 로그

[설치 및 데몬 동작 점검]
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



[Docker 운영 명령 실습 (images, ps, logs)]

# 다운로드된 이미지 목록 확인
hohojooho0306@c4r6s3 practice % docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
ubuntu        latest    de7345b16e94   2 weeks ago    100MB
hello-world   latest    e2ac70e7319a   4 months ago   10.1kB

# 컨테이너 실행 이력 목록 확인
hohojooho0306@c4r6s3 practice % docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED          STATUS                      PORTS   NAMES
ae98f507013d   ubuntu        "bash"     5 minutes ago    Exited (0) 3 minutes ago            my-ubuntu
bc52119204f6   hello-world   "/hello"   11 minutes ago   Exited (0) 11 minutes ago           gifted_meitner

# 컨테이너 로그 확인
hohojooho0306@c4r6s3 practice % docker logs my-ubuntu
root@ae98f507013d:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@ae98f507013d:/# echo "Hello Docker"
Hello Docker
root@ae98f507013d:/# exit
exit


3) 컨테이너 실행 실습 및 동작 관찰 (hello-world, ubuntu)

[hello-world 실행 결과]
hohojooho0306@c4r6s3 practice % docker run hello-world
Hello from Docker!
This message shows that your installation appears to be working correctly.


[ubuntu 컨테이너 내부 진입 후 명령어 수행]

hohojooho0306@c4r6s3 practice % docker run -it --name my-ubuntu ubuntu bash
root@ae98f507013d:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@ae98f507013d:/# echo "Hello Docker"
Hello Docker
root@ae98f507013d:/# exit
exit



[💡 관찰 정리: attach/exec 및 종료/유지 동작 차이]
exit vs Ctrl+P+Q: it 옵션으로 대화형 컨테이너 진입 후 exit를 누르면 메인 프로세스(bash)가 종료되어 컨테이너가 Exited 상태가 되지만, Ctrl+P+Q 단축키를 사용하면 컨테이너를 Up(Running) 상태로 유지하면서 빠져나올 수 있음.
docker attach: 실행 중인 컨테이너의 Main Process(표준 입출력)에 연결하므로, 연결 상태에서 exit 입력 시 컨테이너가 종료됨.
docker exec: 실행 중인 컨테이너 내부에서 새로운 프로세스를 추가로 실행하므로, 작업 완료 후 exit로 빠져나와도 기존 메인 프로세스 및 컨테이너는 계속 실행 유지됨.
4) 기존 Dockerfile 기반 커스텀 이미지 제작 및 실행

선택한 베이스 이미지: nginx:alpine (가볍고 안정적인 웹 서버 베이스)
커스텀 포인트 및 목적: index.html 파일을 생성하여 NGINX 기본 대문 페이지(/usr/share/nginx/html/index.html)를 사용자 지정 커스텀 문구로 변경함.
[소스 코드 및 Dockerfile 제작]

hohojooho0306@c4r6s3 practice % echo "<h1>My Custom Docker Web Server\!</h1>" > index.html
hohojooho0306@c4r6s3 practice % echo "FROM nginx:alpine" > Dockerfile
hohojooho0306@c4r6s3 practice % echo "COPY index.html /usr/share/nginx/html/index.html" >> Dockerfile

[빌드 및 포트 매핑 실행 로그]

hohojooho0306@c4r6s3 practice % docker build -t my-custom-web:1.0 .
[+] Building 6.3s (7/7) FINISHED                                              docker:orbstack
 => [1/2] FROM docker.io/library/nginx:alpine
 => [2/2] COPY index.html /usr/share/nginx/html/index.html
 => naming to docker.io/library/my-custom-web:1.0

hohojooho0306@c4r6s3 practice % docker run -d -p 8080:80 --name web-test my-custom-web:1.0
49b886e899c6d0026fc74564809cd71e4a71ea267ec00d6e8d2d3bf382ce1c20

5) 포트 매핑 브라우저 접속 성공 증거
접속 주소: http://localhost:8080
검증 내용: 브라우저 주소창(localhost:8080)과 웹페이지 메시지(My Custom Docker Web Server!)가 정상적으로 출력됨을 확인.

6) 바인드 마운트 반영 및 Docker 볼륨 영속성 검증

[A. 바인드 마운트 (Bind Mount) 호스트 변경 전/후 비교]
# 호스트 디렉터리($(pwd))를 컨테이너 내부(/app)로 바인드 마운트 연결
$docker run -d --name bind-test -v$(pwd):/app ubuntu sleep infinity

# 호스트에서 파일 변경 전 확인 (파일 없음)
$ ls ./app_test.txt
ls: ./app_test.txt: No such file or directory

# 컨테이너 내부에서 파일 생성
$ docker exec bind-test bash -c "echo 'Bind Mount Sync Test' > /app/app_test.txt"

# 호스트에서 실시간 반영 결과 확인 (동기화 증명)
$ cat ./app_test.txt
Bind Mount Sync Test

[B. Docker Named Volume 생성 및 데이터 영속성 검증]

# 1. Named Volume 생성
hohojooho0306@c4r6s3 practice % docker volume create mydata
mydata

# 2. 첫 번째 컨테이너 생성 및 볼륨 마운트 후 데이터 작성
hohojooho0306@c4r6s3 practice % docker run -d --name vol-test1 -v mydata:/data ubuntu sleep infinity
888df9810decfb9acf0e61f2d11d83c0c1dcd3bf9071066d46fb57f595b970fe

hohojooho0306@c4r6s3 practice % docker exec vol-test1 bash -c "echo 'Important Data' > /data/test.txt"

# 3. 컨테이너 강제 삭제 (파괴 테스트)
hohojooho0306@c4r6s3 practice % docker rm -f vol-test1
vol-test1

# 4. 동일 볼륨을 두 번째 새 컨테이너에 연동 후 데이터 보존 확인
hohojooho0306@c4r6s3 practice % docker run -d --name vol-test2 -v mydata:/data ubuntu sleep infinity
d8b5040cc41c59077f46f04861375e21419b11003dbe1a8d1d8d7c1c6ef42fc1

hohojooho0306@c4r6s3 practice % docker exec vol-test2 bash -c "cat /data/test.txt"
Important Data

검증 결과: vol-test1 컨테이너를 강제로 삭제하였음에도, 동일한 mydata 볼륨을 마운트한 vol-test2 컨테이너에서 /data/test.txt 파일의 Important Data 데이터가 손실 없이 유지됨을 확인함.

7) Git 설정 및 GitHub 연동 로그

# 1. Git 저장소 초기화 및 커밋
hohojooho0306@c4r6s3 practice % git init
hohojooho0306@c4r6s3 practice % git add .
hohojooho0306@c4r6s3 practice % git commit -m "feat: complete dev workstation mission"
[master (최상위-커밋) aea54d5] feat: complete dev workstation mission

# 2. 기본 브랜치 변경 (master -> main)
hohojooho0306@c4r6s3 practice % git branch -M main

# 3. 원격 저장소 URL 재설정 및 Push
hohojooho0306@c4r6s3 practice % git remote set-url origin [https://github.com/hohojooho-ship-it/my-docker-web.git](https://github.com/hohojooho-ship-it/my-docker-web.git)
hohojooho0306@c4r6s3 practice % git push -u origin main

5. 트러블슈팅 (Troubleshooting)

📌 이슈 1: zsh 쉘에서 echo 명령어 내 느낌표(!) 입력 시 zsh: event not found 오류

문제 (Problem): echo "<h1>My Custom Docker Web Server!</h1>" 실행 시 zsh: event not found: </h1> 에러 발생하며 파일 작성 실패.
원인 가설 (Hypothesis): zsh 쉘 특성상 쌍따옴표(" ") 안의 느낌표(!)를 이전 명령 히스토리(History Expansion) 검색 이벤트로 인식함.
해결 (Solution): 느낌표 앞에 이스케이프 문자를 추가하여 \!로 작성 (echo "<h1>My Custom Docker Web Server\!</h1>" > index.html)함으로써 정상 파일 생성 완료.
📌 이슈 2: GitHub URL 입력 시 줄바꿈(Enter)으로 인한 리모트 추가 에러

문제 (Problem): git remote add origin 명령어 실행 시 URL 주소 중간에 줄바꿈이 입력되어 zsh: no such file or directory 및 error: origin 리모트가 이미 있습니다 에러 발생.
원인 가설 (Hypothesis): 붙여넣기 과정에서 줄바꿈 문자가 들어갔으며, 이로 인해 첫 번째 시도에서 origin이 이미 부분 생성됨.
해결 (Solution): git remote set-url origin https://github.com/hohojooho-ship-it/my-docker-web.git 명령어를 통해 정확한 단일 라인 URL로 덮어씌워 리모트 주소 설정을 완료함.
6. 과제 목표 개념 자가 점검 (Self-Check)

절대 경로 vs 상대 경로:
절대 경로: 최상위 루트(/)부터 시작하는 전체 경로 (예: /Users/hohojooho0306/codyssey/practice).
상대 경로: 현재 위치(.)를 기준으로 상대적인 파일 위치 지정 (예: ./index.html).
파일 권한 (r/w/x) 및 755/644 해석:
755 (rwxr-xr-x): 소유자(rwx, 읽기/쓰기/실행), 그룹(r-x, 읽기/실행), 기타 사용자(r-x, 읽기/실행).
644 (rw-r--r--): 소유자(rw-, 읽기/쓰기), 그룹(r--, 읽기), 기타 사용자(r--, 읽기).
포트 매핑 필요성:
격리된 컨테이너 내부 포트(80)를 외부 호스트 컴퓨터 포트(8080)와 연결(-p 8080:80)해야 브라우저에서 localhost:8080으로 접근 가능함.
Docker 볼륨 (영속성):
컨테이너가 삭제되어도 데이터가 지워지지 않도록 호스트의 독립된 공간에 데이터를 지속 저장(Persistence)하는 기능.
Git vs GitHub 차이:
Git: 내 컴퓨터(로컬)에서 소스코드 버전 이력을 관리하는 CLI 도구.
GitHub: Git 이력을 원격에 저장하고 공유/협업할 수 있게 해주는 클라우드 플랫폼.
