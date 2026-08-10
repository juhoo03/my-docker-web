# 🚀 개발 워크스테이션 구축 미션 보고서 (입학 연수)

## 1. 프로젝트 개요

* 터미널 CLI 조작, 파일 권한 설정, Docker 컨테이너 및 커스텀 이미지 제작, 포트 매핑, 바인드 마운트 및 볼륨 영속성 검증, Git/GitHub 연동을 수행
* 재현 가능한 개발 워크스테이션 환경을 성공적으로 구축하고 이를 기술 문서로 검증

---

## 2 개발 환경 안내

| 구분 | 요소 | 버전 및 상세 정보 |
| :--- | :--- | :--- |
| **OS** | macOS | 15.7.4 (24G517) |
| **Shell** | zsh | 455.1 |
| **Runtime** | OrbStack | - |
| **Docker** | Docker Engine | v28.5.2 (build ecc6942) |
| **Git** | Git | v2.53.0 |

<br>

* **OS (운영체제):** 컴퓨터 기본 운영 시스템 (macOS 15.7.4)
* **Shell (쉘):** 터미널 명령어 입력 대화창 (zsh)
* **Runtime (런타임):** Docker를 가볍고 빠르게 구동하는 실행 환경 (OrbStack)
  --> 맥은 기본적으로 Linux가 아니기 때문에 Docker 컨테이너를 실행하려면 Linux 환경이 필요
* **Docker (도커):** 앱 개발 환경을 컨테이너로 격리·실행하는 도구 (v28.5.2)
  --> 어디서든 똑같이 실행
* **Git (깃):** 소스코드 변경 이력을 관리하는 버전 관리 시스템 (v2.53.0)

---

## 파일 구조

### 프로젝트 구조

```bash
my-docker-web/
├── Dockerfile
├── index.html
├── README.md
└── test_file.txt
```

### 구조 설명

현재 프로젝트는 별도의 하위 디렉터리 없이 모든 파일이 루트 디렉터리에 위치한 구조입니다.

* `Dockerfile` : Docker 이미지 빌드 설정 파일
* `index.html` : Nginx에서 제공할 정적 웹 페이지
* `README.md` : 프로젝트 실습 내용 및 문서
* `test_file.txt` : 파일/권한/Git 실습용 파일

## 3. 수행 항목 체크리스트 (Checklist)

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

## 4. 수행 로그 및 검증 결과

git clone https://github.com/juhoo03/my-docker-web

### 4.1. 터미널 조작 및 권한 실습 로그

#### 1) 디렉토리 생성 및 작업 위치 확인

`mkdir -p ~/codyssey/practice`는 새로운 디렉토리를 생성하는 명령어입니다.  
`-p` 옵션은 중간 경로가 없을 경우 함께 생성해 줍니다.
```bash
hohojooho0306@c4r6s3 practice % mkdir -p ~/codyssey/practice
```

---
### 명령어가 궁금하다면?

명령어의 사용 방법과 옵션을 확인할 수 있는 명령어입니다.
```bash
man 명령어
```
---


### 현재 폴더에서 하위 폴더로 이동

`cd ~/codyssey/practice`는 현재 작업 위치를 실습 디렉토리인 `practice`로 이동하는 명령어입니다.
```bash
hohojooho0306@c4r6s3 practice % cd ~/codyssey/practice
```

---

### 현재 위치 확인

`pwd`는 현재 작업 중인 디렉토리의 전체 경로를 출력합니다.
```bash
hohojooho0306@c4r6s3 practice % pwd
/Users/hohojooho0306/codyssey/practice
```



---

### 목록 확인

현재 디렉토리에 있는 파일과 폴더 목록을 확인합니다.
```bash
ls

README.md    test.txt
```


---

### 숨김 파일 포함 목록 확인

`ls -al`은 숨김 파일을 포함하여 자세한 파일 목록을 출력합니다.
```bash
ls -la

total 16
drwxr-xr-x   5 hohojooho0306  staff   160  8월  2 14:33 .
drwxr-xr-x   3 hohojooho0306  staff    96  8월  2 13:39 ..
drwxr-xr-x  14 hohojooho0306  staff   448  8월  2 14:57 .git
-rw-r--r--   1 hohojooho0306  staff   379  8월  2 14:54 README.md
-rw-r--r--   1 hohojooho0306  staff     7  8월  2 13:57 test.txt
```



---
### 파일 생성 및 내용 작성

`touch`는 빈 파일를 새로 만드는 명령어입니다.

```bash
hohojooho0306@c4r6s3 practice % touch test_file.txt
```

`echo`는 문자열을 파일에 저장하는 명령어입니다.  
`>` 기호는 기존 내용이 있다면 덮어씁니다.

```bash
hohojooho0306@c4r6s3 practice % echo 'Hello Codyssey!' > test_file.txt
```

`cat`는 파일 안에 내용이 제대로 입력되었는지 확인하는 명령어입니다.

```bash
hohojooho0306@c4r6s3 practice % cat test_file.txt
Hello Codyssey!
```


### 파일 복사, 이름 변경 및 상세 조회
[파일] [디렉토리] --> 파일 이동
[파일] [파일] --> 파일 이름 변경

`cp test_file.txt copy_file.txt`는 `test_file.txt` 파일을 복사하여 `copy_file.txt`를 생성하는 명령어입니다.

```bash
cp test_file.txt copy_file.txt
```

`mv copy_file.txt moved_file.txt`는 `copy_file.txt`의 이름을 `moved_file.txt`로 변경하는 명령어입니다.

```bash
mv copy_file.txt moved_file.txt
```

`ls -l`은 현재 디렉토리의 파일 목록을 권한, 소유자, 크기, 수정 시간과 함께 자세히 보여주는 명령어입니다.

```bash
hohojooho0306@c3r7s7 practice % ls -l
total 8
-rw-r--r--  1 hohojooho0306  hohojooho0306  16 Aug 10 14:45 test_file.txt
```

---

## 4) 파일 삭제 및 최종 검증

`rm moved_file.txt`는 복사 후 이름을 변경했던 `moved_file.txt` 파일을 삭제하는 명령어입니다.

```bash
rm moved_file.txt
```

`ls -la`는 숨김 파일을 포함한 전체 파일 목록을 자세히 출력하는 명령어입니다.  
`.`은 현재 디렉토리, `..`은 상위 디렉토리를 의미합니다.

```bash
hohojooho0306@c3r7s7 practice % ls -la
total 8
drwxr-xr-x  3 hohojooho0306  hohojooho0306  96 Aug 10 14:58 .
drwxr-xr-x  3 hohojooho0306  hohojooho0306  96 Aug 10 14:41 ..
-rw-r--r--  1 hohojooho0306  hohojooho0306  16 Aug 10 14:45 test_file.txt
```

---

## 5) 권한 변경 실습 및 전/후 비교

`chmod`는 파일이나 디렉토리의 읽기(`r`), 쓰기(`w`), 실행(`x`) 권한을 변경하는 명령어입니다.

권한은 순서대로 **사용자 / 그룹 / 기타 사용자**를 의미합니다.


- 맨 앞자리 `-` : 일반 파일
- 맨 앞자리 `d` : 디렉토리
- `r` : 읽기 권한
- `w` : 쓰기 권한
- `x` : 실행 권한
--> 이진법을 통해 순서대로 4, 2, 1로 나타낼 수 있음
---

###  디렉토리 생성

`mkdir test_dir`는 권한 변경 실습에 사용할 디렉토리 `test_dir`를 생성하는 명령어입니다.

```bash
hohojooho0306@c4r6s3 practice % mkdir test_dir
```
---

###  초기 권한 상태 확인

`ls -ld`는 파일 또는 디렉토리의 권한 정보를 자세히 확인하는 명령어입니다.

```bash
hohojooho0306@c4r6s3 practice % ls -ld test_file.txt test_dir
drwxr-xr-x  2 hohojooho0306  hohojooho0306  64  7 29 15:55 test_dir
-rw-r--r--  1 hohojooho0306  hohojooho0306   0  7 29 15:51 test_file.txt
```


- `test_dir`의 초기 권한은 `drwxr-xr-x`입니다.
  - 디렉토리 권한: `755`
- `test_file.txt`의 초기 권한은 `-rw-r--r--`입니다.
  - 파일 권한: `644`

---

###  권한 변경 수행

`chmod 000 파일명`는 파일에 권한을 변경합니다.

```bash
hohojooho0306@c4r6s3 practice % chmod 755 test_file.txt
hohojooho0306@c4r6s3 practice % chmod 644 test_dir
```


- `test_file.txt` 권한을 `644`에서 `755`로 변경
- `test_dir` 권한을 `755`에서 `644`로 변경

---

###  변경 후 권한 상태 검증

권한이 정상적으로 변경되었는지 `ls -ld` 명령어로 다시 확인합니다.

```bash
hohojooho0306@c3r7s7 practice % ls -ld test_file.txt test_dir
drw-r--r--  2 hohojooho0306  hohojooho0306  64 Aug 10 15:02 test_dir
-rwxr-xr-x  1 hohojooho0306  hohojooho0306  16 Aug 10 14:45 test_file.txt
```


- 파일 권한 변경 확인

```bash
-rw-r--r--  →  -rwxr-xr-x
644         →  755
```


- 디렉토리 권한 변경 확인

```bash
drwxr-xr-x  →  drw-r--r--
755         →  644
```

- `test_dir`의 실행 권한 `x`가 제거되었습니다.
- 디렉토리에서 실행 권한 `x`가 없으면 해당 디렉토리로 진입하거나 내부 파일에 접근하는 데 제한이 생길 수 있습니다.

---

### 최종 정리

- `test_file.txt`

```bash
644 (-rw-r--r--) → 755 (-rwxr-xr-x)
```

- `test_dir`

```bash
755 (drwxr-xr-x) → 644 (drw-r--r--)
```

파일과 디렉토리의 권한이 `chmod` 명령어를 통해 정상적으로 변경되었음을 확인
---

## 4.2. Docker 설치 점검 및 운영 로그

Docker 설치 여부와 정상 동작 상태를 확인하기 위해 Git 버전, Docker 버전, Docker 시스템 정보를 점검했습니다.

---

## 1) Docker 기본 개념 정의

### Docker란?

Docker는 프로그램을 **컨테이너(Container)** 라는 독립된 실행 공간에서 실행할 수 있게 해주는 도구입니다.

프로그램을 실행하려면 보통 다음과 같은 환경이 필요합니다.

- Python 버전
- Node.js 버전
- 라이브러리
- 환경 설정
- 운영체제 관련 설정

하지만 사람마다 컴퓨터 환경이 다르면 같은 프로그램도 정상적으로 실행되지 않을 수 있습니다.

Docker는 이런 실행 환경을 하나로 묶어 **어디서든 동일하게 실행할 수 있도록** 도와줍니다.

---

### Container란?

컨테이너는 프로그램과 실행에 필요한 환경을 함께 담은 **독립된 실행 공간**입니다.

즉, 내 컴퓨터 환경에 직접 영향을 많이 받지 않고 프로그램을 실행할 수 있습니다.

---

### Image란?

이미지는 컨테이너를 만들기 위한 **설계도 또는 실행 템플릿**입니다.

컨테이너는 이미지를 기반으로 생성됩니다.

---

### Docker Daemon이란?

Docker Daemon은 사용자가 직접 조작하지 않아도 백그라운드에서 계속 실행되는 프로그램입니다.

Docker에서 컨테이너를 실제로 만들고, 실행하고, 관리하는 핵심 역할을 합니다.

---

### Docker Client란?

Docker Client는 사용자가 터미널에서 입력한 명령어를 Docker Daemon에게 전달하는 역할을 합니다.

예를 들어 사용자가 아래 명령어를 입력하면, Docker Client가 이 명령을 Docker Daemon에게 전달하고, 
Docker Daemon이 실제 컨테이너를 실행합니다.

---

### 주방 비유로 이해하기

Docker의 동작 방식은 식당에 비유할 수 있습니다.

| Docker 개념 | 식당 비유 |
|---|---|
| 사용자 | 손님 |
| Docker Client | 주문 받는 직원 |
| Docker Daemon | 주방 |
| Image | 음식 레시피 |
| Container | 완성된 음식 |

손님이 주문하면 직원이 주방에 전달하고, 주방은 레시피를 보고 음식을 만듭니다.

Docker도 마찬가지로 사용자가 명령어를 입력하면 Docker Client가 Docker Daemon에게 전달하고, Docker Daemon이 이미지를 이용해 컨테이너를 실행합니다.

```text
[ 사용자 / 터미널 ]
        │
        ▼ 명령어 전달: docker run ...
┌──────────────────────────────────────────────┐
│ Docker Client                                │
└──────────────────────┬───────────────────────┘
                       │ REST API / Socket
                       ▼
┌──────────────────────────────────────────────┐
│ Docker Host                                  │
│                                              │
│  ┌────────────────────────────────────────┐  │
│  │ Docker Daemon                          │  │
│  │ dockerd                                │  │
│  └──────────────────┬─────────────────────┘  │
│                     │                        │
│          ┌──────────┴──────────┐             │
│          ▼                     ▼             │
│      Images              Containers          │
└──────────────────────────────────────────────┘
```

---

## 2) Git 버전 확인

`git --version`은 현재 설치된 Git의 버전을 확인하는 명령어입니다.

```bash
hohojooho0306@c4r6s3 practice % git --version
git version 2.53.0
```
---

## 3) Docker 버전 확인

`docker --version`은 현재 설치된 Docker의 버전을 확인하는 명령어입니다.

```bash
hohojooho0306@c4r6s3 practice % docker --version
Docker version 28.5.2, build ecc6942
```
---

## 4) Docker 시스템 전체 상태 및 상세 정보 확인

`docker info`는 Docker Client와 Docker Server의 상세 정보를 확인하는 명령어입니다.

이 명령어를 통해 Docker Daemon이 정상적으로 동작하는지, 컨테이너와 이미지가 몇 개 있는지, Docker가 어떤 환경에서 실행 중인지 확인할 수 있습니다.

```bash
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

### 주요 항목 설명

- `Client`
  - 사용자가 터미널에서 입력하는 Docker 명령어를 처리하는 부분입니다.
  - Docker Daemon에게 명령을 전달합니다.

- `Server`
  - Docker Daemon이 동작하는 서버 영역입니다.
  - 컨테이너와 이미지를 실제로 관리합니다.

- `Containers`
  - 현재 Docker에 존재하는 컨테이너의 총 개수입니다.
  - 실행 중, 일시 정지, 정지 상태를 포함합니다.

- `Running`
  - 현재 실행 중인 컨테이너 개수입니다.

- `Paused`
  - 일시 정지된 컨테이너 개수입니다.

- `Stopped`
  - 정지된 컨테이너 개수입니다.

- `Images`
  - 로컬 환경에 저장된 Docker 이미지 개수입니다.

- `Server Version`
  - Docker Daemon의 버전입니다.

- `Operating System`
  - Docker가 실행 중인 운영 환경입니다.
  - 현재는 `OrbStack` 환경에서 실행 중입니다.

- `OSType`
  - Docker가 사용하는 운영체제 타입입니다.
  - 현재는 `linux`입니다.

- `Architecture`
  - 시스템 아키텍처 정보입니다.
  - 현재는 `x86_64`입니다.

- `CPUs`
  - Docker가 사용할 수 있는 CPU 개수입니다.

- `Total Memory`
  - Docker 환경에서 사용할 수 있는 전체 메모리 크기입니다.


---


## 4.2. Docker 설치 점검 및 운영 로그

Docker가 정상적으로 설치되어 있고, Docker Daemon이 제대로 동작하는지 확인합니다.

---

# Docker 설치 및 기본 점검

## Docker 버전 확인

`docker --version`은 현재 설치된 Docker 버전을 확인하는 명령어입니다.

```bash
docker --version
```

### 실행 결과

```bash
hohojooho0306@c4r6s3 practice % docker --version
Docker version 29.4.0, build 9d7ad9f
```

### 확인 내용

- Docker가 정상적으로 설치되어 있음을 확인했습니다.
- 현재 Docker 버전은 `29.4.0`입니다.
- `build 9d7ad9f`는 해당 Docker 버전의 빌드 식별자입니다.

---

## Docker 시스템 전체 상태 & 상세 정보 확인

`docker info`는 Docker Client와 Docker Server의 상세 정보를 확인하는 명령어입니다.

Docker Daemon이 정상적으로 실행 중이면 `Server` 정보가 출력됩니다.

```bash
docker info
```

### 실행 결과

```bash
hohojooho0306@c4r6s3 practice % docker info
Client:
 Version:    29.4.0
 Context:    orbstack
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.33.0
    Path:     /Users/hanjiwon/.docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v5.1.2
    Path:     /Users/hanjiwon/.docker/cli-plugins/docker-compose

Server: # 정상 출력 확인
 Containers: 1
  Running: 0
  Paused: 0
  Stopped: 1
 Images: 3
 Server Version: 29.4.0
 Storage Driver: overlayfs
  driver-type: io.containerd.snapshotter.v1
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog

... 이하 생략
```

### 주요 항목 설명

- `Client`
  - 사용자가 터미널에서 입력하는 Docker 명령어를 처리하는 부분입니다.
  - Docker Daemon에게 명령을 전달합니다.

- `Server`
  - Docker Daemon이 동작하는 영역입니다.
  - 컨테이너와 이미지를 실제로 생성, 실행, 관리합니다.

- `Containers`
  - 현재 Docker에 존재하는 컨테이너의 총 개수입니다.

- `Running`
  - 현재 실행 중인 컨테이너 개수입니다.

- `Paused`
  - 일시 정지된 컨테이너 개수입니다.

- `Stopped`
  - 정지된 컨테이너 개수입니다.

- `Images`
  - 로컬 환경에 저장된 Docker 이미지 개수입니다.

- `Server Version`
  - Docker Daemon의 버전입니다.

- `Context`
  - Docker가 현재 어떤 실행 환경을 사용하는지 나타냅니다.
  - 현재는 `orbstack` 환경을 사용하고 있습니다.

### 확인 내용

- `docker info` 명령어가 정상적으로 실행되었습니다.
- `Server` 정보가 출력되었으므로 Docker Daemon이 정상적으로 동작 중임을 확인했습니다.
- 현재 컨테이너는 총 `1`개이며, 정지 상태 컨테이너가 `1`개 있음을 확인했습니다.
- 현재 로컬 이미지가 `3`개 존재함을 확인했습니다.

---

# Docker 기본 개념 정리

## Docker란?

Docker는 프로그램을 **컨테이너(Container)** 라는 독립된 공간에서 실행할 수 있게 해주는 도구입니다.

프로그램 실행에는 보통 다음과 같은 환경이 필요합니다.

- Python 버전
- Node.js 버전
- 라이브러리
- 환경 설정
- 운영체제 관련 설정

하지만 사람마다 컴퓨터 환경이 다르면 같은 프로그램도 정상적으로 실행되지 않을 수 있습니다.

Docker는 이런 실행 환경을 이미지로 묶고, 컨테이너로 실행하여 **어디서든 동일한 환경에서 프로그램을 실행할 수 있도록** 도와줍니다.

---

## Container란?

컨테이너는 프로그램과 실행에 필요한 환경을 함께 담은 **독립된 실행 공간**입니다.

즉, 내 컴퓨터 환경에 직접 영향을 적게 받으면서 프로그램을 실행할 수 있습니다.

---

## Image란?

이미지는 컨테이너를 만들기 위한 **설계도 또는 실행 템플릿**입니다.

컨테이너는 이미지를 기반으로 생성됩니다.

---

## Docker Daemon이란?

Docker Daemon은 백그라운드에서 계속 실행되며 컨테이너를 실제로 만들고 실행하고 관리하는 프로그램입니다.

사용자가 터미널에 Docker 명령어를 입력하면 Docker Client가 Docker Daemon에게 요청을 전달하고, Docker Daemon이 실제 작업을 수행합니다.

---

## Docker 동작 구조

```text
[ 사용자 / 터미널 ]
        │
        ▼ 명령어 전달: docker run ...
┌──────────────────────────────────────────────┐
│ Docker Client                                │
└──────────────────────┬───────────────────────┘
                       │ REST API / Socket
                       ▼
┌──────────────────────────────────────────────┐
│ Docker Host                                  │
│                                              │
│  ┌────────────────────────────────────────┐  │
│  │ Docker Daemon                          │  │
│  │ dockerd                                │  │
│  └──────────────────┬─────────────────────┘  │
│                     │                        │
│          ┌──────────┴──────────┐             │
│          ▼                     ▼             │
│      Images              Containers          │
└──────────────────────────────────────────────┘
```

---

## 식당 비유로 이해하기

| Docker 개념 | 식당 비유 |
|---|---|
| 사용자 | 손님 |
| Docker Client | 주문 받는 직원 |
| Docker Daemon | 주방 |
| Image | 음식 레시피 |
| Container | 완성된 음식 |

손님이 주문하면 직원이 주방에 전달하고, 주방은 레시피를 보고 음식을 만듭니다.

Docker도 마찬가지로 사용자가 명령어를 입력하면 Docker Client가 Docker Daemon에게 전달하고, Docker Daemon이 이미지를 이용해 컨테이너를 실행합니다.

---

# Docker 기본 운영 명령 수행

## Docker 이미지 다운로드

`docker pull`은 Docker Hub에서 이미지를 다운로드하는 명령어입니다.

```bash
hohojooho0306@c3r7s7 practice % docker pull nginx
Using default tag: latest
latest: Pulling from library/nginx
26c307b5e35a: Pull complete 
3c55dc422a81: Pull complete 
d84ae7b21412: Pull complete 
c0df8d325117: Pull complete 
b8b80b9bc028: Pull complete 
f5de6e85ac74: Pull complete 
5a4222b844e8: Pull complete 
Digest: sha256:8541484afbc9c8a5a8a99b379568ebbc957f658583ec9448fc43104229c03cf8
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
```


- `nginx` 이미지를 로컬 환경으로 다운로드했습니다.
- 이미지가 이미 존재하는 경우 최신 상태인지 확인합니다.

---

## Docker 이미지 확인

`docker images`는 로컬에 저장된 Docker 이미지 목록을 확인하는 명령어입니다.

```bash
hohojooho0306@c4r6s3 practice % docker images
IMAGE           ID             DISK USAGE   CONTENT SIZE   EXTRA
mysql:latest    66aec17cd21a   1.31GB       286MB
nginx:latest    5a88c9c45479   258MB        64.3MB
ubuntu:latest   3131b4cc82a7   137MB        3.71MB         U
```

- `mysql`, `nginx`, `ubuntu` 이미지가 로컬에 저장되어 있음을 확인했습니다.
- 이미지는 컨테이너를 생성하기 위한 기반이 됩니다.

---

## 현재 실행 중인 Container 목록 확인

`docker ps`는 현재 실행 중인 컨테이너 목록을 확인하는 명령어입니다.

```bash
hohojooho0306@c4r6s3 practice % docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

- 현재 실행 중인 컨테이너가 없음을 확인했습니다.
- `docker ps`와 `docker container ls`는 실행 중인 컨테이너만 보여줍니다.

---

## 모든 Container 목록 확인

`docker ps -a`는 실행 중인 컨테이너뿐만 아니라 종료된 컨테이너까지 모두 보여주는 명령어입니다.

```bash
hohojooho0306@c4r6s3 practice % docker ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED        STATUS                    PORTS     NAMES
5968a1446969   ubuntu    "/bin/bash"   21 hours ago   Exited (0) 21 hours ago             volume-first

```

- 종료된 컨테이너 `volume-first`가 존재함을 확인했습니다.
- 상태가 `Exited`이므로 현재 실행 중은 아니지만, 컨테이너 이력은 남아 있습니다.

---

## Docker 로그 확인

`docker logs`는 특정 컨테이너에서 출력된 로그를 확인하는 명령어입니다.

```bash
hohojooho0306@c4r6s3 practice % docker logs 5968a1446969
root@5968a1446969:/# touch test.txt
root@5968a1446969:/# ls
bin  boot  data  dev  etc  home  lib  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@5968a1446969:/# exit
exit
```

- 컨테이너 내부에서 실행했던 명령어 로그를 확인했습니다.
- `touch test.txt`, `ls`, `exit` 명령 실행 기록이 남아 있음을 확인했습니다.

---

## 컨테이너 및 이미지 삭제 정리

실습이 끝난 후 사용하지 않는 컨테이너와 이미지를 삭제할 수 있습니다.

`docker rm`은 컨테이너를 삭제하는 명령어입니다.

```bash
docker rm 5968a1446969
```

`docker rmi`는 이미지를 삭제하는 명령어입니다.

```bash
docker rmi hello-world
```

- 사용하지 않는 정지 상태 컨테이너를 삭제하여 불필요한 리소스 사용을 줄일 수 있습니다.
- 사용하지 않는 이미지를 삭제하여 디스크 용량을 확보할 수 있습니다.

---

## 4.3. 컨테이너 실행 실습 및 관찰


---

# 1) hello-world 이미지 실행

`docker run`은 이미지를 기반으로 컨테이너를 생성하고 실행하는 명령어입니다.

이미지가 로컬에 없으면 Docker Hub에서 자동으로 다운로드한 뒤 컨테이너를 생성하고 실행합니다.

즉, `docker run`은 다음 과정을 자동으로 수행합니다.

1. `pull`: 이미지가 없으면 다운로드
2. `create`: 컨테이너 생성
3. `start`: 컨테이너 실행

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
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

- Docker Client가 Docker Daemon과 정상적으로 통신했습니다.
- `hello-world` 이미지가 없을 경우 자동으로 다운로드됩니다.
- Docker Daemon이 이미지를 기반으로 컨테이너를 생성하고 실행했습니다.
- 컨테이너의 출력 결과가 터미널에 정상적으로 표시되었습니다.

---

# 2) ubuntu 대화형 컨테이너 실행

`ubuntu`라는 이미지를 기반으로 대화형 컨테이너를 실행합니다.

`-it` 옵션은 컨테이너 내부 터미널에 직접 접속하기 위해 사용합니다.

```bash
hohojooho0306@c4r6s3 practice % docker run -it --name my-ubuntu ubuntu bash
root@ae98f507013d:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@ae98f507013d:/# echo "Hello Docker"
Hello Docker
root@ae98f507013d:/# exit
exit
```

### 옵션 설명

- `-i`
  - interactive의 약자입니다.
  - 표준 입력을 유지하여 사용자가 명령어를 입력할 수 있게 합니다.

- `-t`
  - tty의 약자입니다.
  - 터미널 환경을 할당합니다.

- `--name my-ubuntu`
  - 컨테이너 이름을 `my-ubuntu`로 지정합니다.

- `ubuntu`
  - 사용할 Docker 이미지 이름입니다.

- `bash`
  - 컨테이너 내부에서 실행할 쉘입니다.


- Ubuntu 컨테이너 내부 bash 쉘에 정상적으로 접속했습니다.
- 컨테이너 내부에서 `ls`, `echo` 명령어가 정상 실행되었습니다.
- `exit`를 입력하면 bash 프로세스가 종료됩니다.
- 이 경우 bash가 컨테이너의 메인 프로세스이므로 컨테이너도 함께 종료됩니다.

---

# 3) attach와 exec 차이 관찰

실행 중인 컨테이너에 다시 접속하는 방법에는 `attach`와 `exec`가 있습니다.

두 명령어는 비슷해 보이지만 동작 방식이 다릅니다.

---

## attach란?

`docker attach`는 이미 실행 중인 컨테이너의 메인 프로세스에 직접 연결합니다.

이 컨테이너의 메인 프로세스는 bash
다른 터미널에서 docker attach my-ubuntu
하면 기존에 실행 중이던 bash 화면에 다시 붙는 것

새 bash를 만드는 게 아니라 원래 있던 bash에 연결하는 것

- `exit`를 입력하면 메인 프로세스가 종료됩니다.
- 대화형 bash 컨테이너에 attach한 뒤 `exit`하면 컨테이너가 `Exited` 상태가 될 수 있습니다.
  
```bash
docker attach my-ubuntu
```

---

## exec란?

`docker exec`는 실행 중인 컨테이너 안에 exec는 컨테이너 내부의 자체적 프로그램을 통해 새로운 명령어를 실행합니다.
이건 기존 bash에 붙는 게 아니라 컨테이너 안에서 새 bash를 하나 더 실행하는 것이다.
```bash
docker exec -it my-ubuntu bash
```

# Docker 커스텀 NGINX 웹 서버 실습

## Dockerfile로 커스텀 NGINX 이미지 만들기

이번 실습에서는 `nginx:alpine` 이미지를 기반으로 사용자 지정 `index.html`을 포함한 커스텀 웹 서버 이미지를 생성했습니다.

- **베이스 이미지**: `nginx:alpine`
- **커스텀 목적**: 사용자 지정 `index.html`을 컨테이너 내부 NGINX 기본 경로에 복사하여 정적 웹 페이지 변경
- **복사 경로**: `/usr/share/nginx/html/index.html`

---

## 소스 코드 작성

먼저 커스텀 메인 페이지로 사용할 `index.html` 파일과 Docker 이미지를 만들기 위한 `Dockerfile`을 생성합니다.

```bash
hohojooho0306@c4r6s3 practice % echo "My Custom Docker Web Server!" > index.html

hohojooho0306@c4r6s3 practice % echo "FROM nginx:alpine" > Dockerfile

hohojooho0306@c4r6s3 practice % echo "COPY index.html /usr/share/nginx/html/index.html" >> Dockerfile
```

- `index.html` 파일에 표시할 문구를 작성합니다.
- `FROM nginx:alpine`은 `nginx:alpine` 이미지를 베이스 이미지로 사용한다는 의미입니다.
- `COPY` 명령어는 로컬의 `index.html` 파일을 컨테이너 내부 NGINX 웹 루트 경로로 복사합니다.

---

## Dockerfile 기반 이미지 빌드

작성한 `Dockerfile`을 기반으로 Docker 이미지를 빌드합니다.

```bash
hohojooho0306@c4r6s3 practice % docker build -t my-custom-web:1.0 .
[+] Building 6.3s (7/7) FINISHED docker:orbstack

=> [1/2] FROM docker.io/library/nginx:alpine
=> [2/2] COPY index.html /usr/share/nginx/html/index.html
=> naming to docker.io/library/my-custom-web:1.0
```

- `docker build`는 Dockerfile을 기반으로 이미지를 생성하는 명령어입니다.
- `-t my-custom-web:1.0` 옵션은 이미지 이름과 태그를 지정합니다.
  - 이미지 이름: `my-custom-web`
  - 태그: `1.0`
- 마지막의 `.`은 현재 디렉터리를 빌드 컨텍스트로 사용한다는 의미입니다.
- 빌드 결과 `my-custom-web:1.0` 이미지가 생성되었습니다.

---

## 컨테이너 실행

빌드한 이미지를 기반으로 컨테이너를 실행합니다.

```bash
hohojooho0306@c4r6s3 practice % docker run -d -p 8080:80 --name web-test my-custom-web:1.0

49b886e899c6d0026fc74564809cd71e4a71ea267ec00d6e8d2d3bf382ce1c20
```

- `docker run`은 이미지를 기반으로 컨테이너를 생성하고 실행하는 명령어입니다.
- `-d` 옵션은 컨테이너를 백그라운드에서 실행합니다.
- `-p 8080:80` 옵션은 호스트의 `8080` 포트를 컨테이너의 `80` 포트와 연결합니다.
  - 호스트 포트: `8080`
  - 컨테이너 포트: `80`
- `--name web-test`는 컨테이너 이름을 `web-test`로 지정합니다.
- `my-custom-web:1.0` 이미지를 사용해 컨테이너를 실행했습니다.

---

## 포트 매핑 브라우저 접속 확인

컨테이너 실행 후 브라우저에서 아래 주소로 접속합니다.

```text
http://localhost:8080
```

- 호스트의 `8080` 포트로 접속하면 컨테이너 내부의 NGINX `80` 포트로 요청이 전달됩니다.
- 브라우저 화면에 아래 문구가 표시되면 정상적으로 동작한 것입니다.

```text
My Custom Docker Web Server!
```

---

### 4.5. 바인드 마운트 및 볼륨 영속성 검증

--> 둘 다 컨테이너가 삭제되어도 데이터를 남길 수 있음

#### 1) 바인드 마운트 (Bind Mount) 실시간 동기화
* **바인드 마운트 개념:** 호스트 컴퓨터의 특정 폴더를 컨테이너 내부 폴더와 직접 연결하는 방식
* `-v $(pwd):/app`: 현재 작업 위치(`$(pwd)`)를 컨테이너의 `/app`에 연결합니다.

호스트에서 파일을 수정하면 컨테이너에도 바로 반영됩니다.
호스트 경로에 의존하기 때문에 다른 환경에서는 경로 문제가 생길 수 있습니다

```bash
# 호스트 디렉터리를 컨테이너로 연결
$ docker run -d --name bind-test -v $(pwd):/app ubuntu sleep infinity

# 컨테이너 내부에서 파일 생성
$ docker exec bind-test bash -c "echo 'Bind Mount Sync Test' > /app/app_test.txt"

# 호스트에서 실시간 작성 파일 확인
$ cat ./app_test.txt
Bind Mount Sync Test
```
* 컨테이너 내부에서 생성한 파일이 호스트 파일시스템 상에 즉시 동기화되어 반영, 반대도 가능

---

#### 2) Named Volume 데이터 영속성 검증
* **Named Volume 개념:** 볼륨은 Docker가 직접 관리하는 저장 공간
사용자가 직접 호스트 경로를 지정하지 않고, Docker가 내부적으로 안전한 위치에 데이터를 저장합니다.

* `영속성이란?`: 컨테이너는 기본적으로 삭제하면 내부 데이터도 같이 사라지지만 볼륨이나 바인드 마운트에 저장한 데이터는 남아있음
이를 데이터 영속성이라 부름

```bash
# 1. 첫 번째 컨테이너 생성 및 볼륨 마운트 후 데이터 기록
hohojooho0306@c4r6s3 practice % docker run -d --name vol-test1 -v mydata:/data ubuntu sleep infinity
888df9810decfb9acf0e61f2d11d83c0c1dcd3bf9071066d46fb57f595b970fe
hohojooho0306@c4r6s3 practice % docker exec vol-test1 bash -c "echo 'Important Data' > /data/test.txt"

# 2. 첫 번째 컨테이너 강제 파기 (rm -f)
hohojooho0306@c4r6s3 practice % docker rm -f vol-test1
vol-test1

# 3. 동일 볼륨을 두 번째 새 컨테이너에 연동하여 데이터 복구 확인
hohojooho0306@c4r6s3 practice % docker run -d --name vol-test2 -v mydata:/data ubuntu sleep infinity
d8b5040cc41c59077f46f04861375e21419b11003dbe1a8d1d8d7c1c6ef42fc1
hohojooho0306@c4r6s3 practice % docker exec vol-test2 bash -c "cat /data/test.txt"
Important Data
```
* **확인 내용:** 첫 번째 컨테이너를 강제 삭제(`docker rm -f`)했음에도 불구하고, 동일 볼륨을 마운트한 두 번째 컨테이너에서 `Important Data` 텍스트가 유실 없이 완벽하게 복구되었습니다.

Docker가 저장 위치를 관리합니다.
컨테이너를 삭제해도 볼륨은 삭제되지 않습니다.
데이터베이스 데이터 저장에 많이 사용합니다.
호스트 경로에 직접 의존하지 않아 관리가 편합니다.
---

### 4.6. Git 설정 및 GitHub 연동 로그

#### 1) Git 사용자 환경 설정 (`git config`)
* `git config user.name` / `user.email`: 커밋 작성자의 소유자 정보를 설정합니다.
* `git config --list`: 설정된 사용자명과 이메일 항목을 조회 및 검증합니다.

```bash
# 1. Git 로컬/글로벌 사용자 설정 및 검증
hohojooho0306@c4r6s3 practice % git config --global user.name "juhoo03"
hohojooho0306@c4r6s3 practice % git config --global user.email "hohojooho@*****.com"
보안으로 인한 *처리

hohojooho0306@c4r6s3 practice % git config --list | grep user
user.name=juhoo03
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
hohojooho0306@c4r6s3 practice % git commit -m "feat: complete dev workstation mission nnn"
[master (최상위-커밋) aea54d5] feat: complete dev workstation mission
hohojooho0306@c4r6s3 practice % git branch -M main

# 3. 원격 저장소 연동 및 Push 완료
hohojooho0306@c4r6s3 practice % git remote add origin [https://github.com/hohojooho-ship-it/my-docker-web.git](https://github.com/hohojooho-ship-it/my-docker-web.git)
hohojooho0306@c4r6s3 practice % git push -u origin main
To [https://github.com/hohojooho-ship-it/my-docker-web.git](https://github.com/hohojooho-ship-it/my-docker-web.git)
 * [new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.

이미 생성돼있다면
rm -rf my-docker-web/.git
git add my-docker-web
git status
```
* **원격 저장소 확인 링크:** [https://github.com/hohojooho-ship-it/my-docker-web.git](https://github.com/juhoo03/my-docker-web.git)

touch hello.txt
git status
git add hello.txt
git commit -m "feat: add hello.txt file"
git push

---

## 5. 트러블슈팅 (Troubleshooting)

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

## 6. 과제 목표 개념 자가 점검 (Self-Check)

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

