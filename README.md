### 프로젝트 개요
이 프로젝트는 내 컴퓨터에 개발자용 ‘작업실’을 직접 꾸미는 과정입니다. 단순히 프로그램을 설치하는 것을 넘어, 터미널로 작업 공간과 권한을 정리하고, Docker로 언제든 같은 조건으로 다시 실행할 수 있는 개발 환경을 만들며, Git/GitHub로 작업 기록과 협업 기반을 갖추는 것을 목표로 합니다.

개발은 코드를 작성하기 전에 시작됩니다. 터미널은 파일과 디렉터리를 만들고 이동하며 권한을 관리하는 기본 작업 공간이고, Docker는 운영체제·라이브러리·실행 명령을 이미지로 패키징해 컨테이너로 격리 실행하는 도구입니다. Git은 로컬에서 변경 이력을 기록하는 버전 관리 도구이며, GitHub는 그 저장소를 원격에 두고 공유·협업할 수 있게 하는 플랫폼입니다.

미션 목표
터미널 CLI로 프로젝트 디렉터리를 생성·이동·복사·삭제하고, 파일과 디렉터리의 권한을 확인·변경합니다.

Docker 환경을 실행하고, docker --version, docker info 등으로 설치와 데몬 동작을 점검합니다.

hello-world와 ubuntu 컨테이너를 실행해 이미지 다운로드, 컨테이너 생성, 내부 명령 실행, 로그·상태 확인을 경험합니다.

기존 베이스 이미지를 기반으로 Dockerfile을 작성해 커스텀 웹 서버 이미지를 빌드하고, 포트 매핑으로 브라우저 접속을 검증합니다.

바인드 마운트로 호스트 파일 변경이 컨테이너에 반영되는지 확인하고, Docker 볼륨으로 컨테이너를 삭제한 뒤에도 데이터가 유지되는지 검증합니다. Docker 볼륨은 컨테이너 수명과 분리되어 데이터를 보존하는 방식입니다.

Git 사용자 정보와 기본 브랜치를 설정하고, VSCode에서 GitHub 저장소와 연동해 소스코드와 실행 로그를 버전 관리합니다. Git은 프로젝트와 변경 이력을 관리하고, GitHub는 이를 클라우드에 저장하며 협업할 수 있게 합니다.

### 실습 환경
- OS: Windows
- 터미널: VS Code 통합 터미널
- 권한 확인 도구 : icacls
- Docker 버전 : 29.9.2, build a82e528
- git 버전 : git version 2.51.0.windows.1

### 수행 항목 체크리스트

### 터미널 기본 조작 (Windows CMD)
- [o] 현재 작업 디렉터리 확인 — `cd`
- [o] 숨김 파일 포함 목록 확인 — `dir /a`
- [o] 디렉터리 이동 — `cd`
- [o] 디렉터리 및 빈 파일 생성 — `mkdir`, `type nul >`
- [o] 파일 복사, 이름 변경/이동, 삭제 — `copy`, `ren`, `move`, `del`
- [o] 파일 내용 확인 — `type`
- 증거: [터미널 조작 로그]

# 터미널 조작 로그 (Windows CMD)

## 2026-11-03

```cmd
cd
C:\Users\codessey1\terminal-practice

2026-11-03  오전 06:48    <DIR>          .
2026-11-02  오후 10:57    <DIR>          ..
2026-11-03  오전 12:19    <DIR>          .git
2026-11-02  오후 11:24             4,884 .gitignore
2026-11-03  오전 03:31    <DIR>          .qodo
2026-11-03  오전 07:12    <DIR>          docs
2026-11-03  오전 12:44    <DIR>          practice
2026-11-03  오전 07:03             7,977 README.md
2026-11-03  오전 06:45    <DIR>          terminal-practice
               2개 파일              12,861 바이트
               7개 디렉터리   8,158,355,456 바이트 남음

mkdir docs
type nul > docs\terminal-log.md

copy docs\terminal-log.md docs\terminal-log-copy.md
ren docs\terminal-log-copy.md terminal-log-renamed.md
move docs\terminal-log-renamed.md docs\terminal-log-moved.md
del docs\terminal-log-moved.md

type docs\terminal-log.md
```

### 파일 및 디렉터리 권한 (Windows CMD)
- [o] 파일 권한 확인 — `icacls practice.txt`
- [o] 파일 권한 변경 및 변경 전/후 비교 — `icacls practice.txt /grant "%USERNAME%":R`
- [odocker info] 디렉터리 권한 변경 및 변경 전/후 비교 — `icacls practice-dir /grant "%USERNAME%":RX`
- 증거: [권한 실습 로그]

# 권한 실습 로그 (Windows CMD)

## 파일 권한

### 변경 전

```cmd
icacls practice.txt
```

```text
practice.txt ... 사용자이름:(I)(F)
```

### 변경 명령

```cmd
icacls practice.txt /grant "%USERNAME%":R
```

### 변경 후

```cmd
icacls practice.txt
```

```text
practice.txt ... 사용자이름:(R)
```

## 디렉터리 권한

### 변경 전

```cmd
icacls practice-dir
```

### 변경 명령

```cmd
icacls practice-dir /grant "%USERNAME%":RX
```

### 변경 후

```cmd
icacls practice-dir
```

### Docker 설치 및 점검
- [o] Docker 버전 확인 — `docker --version`
- [o] Docker 데몬 동작 확인 — `docker info`
- [o] OrbStack 기반 Docker 환경 사용 확인
- 증거: [Docker 설치·점검 로그]

# Docker 설치·점검 로그

## Docker 버전 확인

```cmd
docker --version
```

```text
29.9.2, build a82e528
```

## Docker 데몬 동작 확인

```cmd
docker info
```

```text
Client:
 Version: 28.5.1
 Context: orbstack

Server:
 Containers: 0
 Images: 0
 Server Version: 28.5.1Client:
 Version:    29.9.2
 Context:    desktop-linux
 Debug Mode: false
 Plugins:
  agent: Docker AI Agent Runner (Docker Inc.)
    Version:  v1.152.0
    Path:     C:\Users\jd\.docker\cli-plugins\docker-agent.exe
  ai: Docker AI Agent - Ask Gordon (Docker Inc.)
    Version:  v1.33.0
    Path:     C:\Users\jd\.docker\cli-plugins\docker-ai.exe
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.38.1
    Path:     C:\Users\jd\.docker\cli-plugins\docker-buildx.exe
  compose: Docker Compose (Docker Inc.)
    Version:  v5.6.1
    Path:     C:\Users\jd\.docker\cli-plugins\docker-compose.exe
  debug: Get a shell into any image or container (Docker Inc.)
    Version:  0.49.0
    Path:     C:\Users\jd\.docker\cli-plugins\docker-debug.exe
  desktop: Docker Desktop commands (Docker Inc.)
    Version:  4.97.0
    Path:     C:\Users\jd\.docker\cli-plugins\docker-desktop.exe
  dhi: CLI for managing Docker Hardened Images (Docker Inc.)
    Version:  v0.0.9
    Path:     C:\Users\jd\.docker\cli-plugins\docker-dhi.exe
  extension: Manages Docker extensions (Docker Inc.)
    Version:  v0.2.32
    Path:     C:\Users\jd\.docker\cli-plugins\docker-extension.exe
  init: Creates Docker-related starter files for your project (Docker Inc.)
    Version:  v1.4.1
    Path:     C:\Users\jd\.docker\cli-plugins\docker-init.exe
  mcp: Docker MCP Plugin (Docker Inc.)
    Version:  v0.44.2
    Path:     C:\Users\jd\.docker\cli-plugins\docker-mcp.exe
  model: Docker Model Runner (Docker Inc.)
    Version:  v1.2.9
    Path:     C:\Users\jd\.docker\cli-plugins\docker-model.exe
  offload: Docker Offload (Docker Inc.)
    Version:  v0.7.21
    Path:     C:\Users\jd\.docker\cli-plugins\docker-offload.exe
  pass: Docker Pass Secrets Manager Plugin (beta) (Docker Inc.)
    Version:  v0.3.2
    Path:     C:\Users\jd\.docker\cli-plugins\docker-pass.exe
  sandbox: "docker sandbox" is deprecated, use Docker Sandboxes instead (Docker Inc.)
    Version:  v0.13.0
    Path:     C:\Users\jd\.docker\cli-plugins\docker-sandbox.exe
  scout: Docker Scout (Docker Inc.)
    Version:  v1.27.0
    Path:     C:\Users\jd\.docker\cli-plugins\docker-scout.exe

Server:
 Containers: 3
  Running: 1
  Paused: 0
  Stopped: 2
 Images: 3
 Server Version: 29.9.2
 Storage Driver: overlayfs
  driver-type: io.containerd.snapshotter.v1
 Logging Driver: local
 Cgroup Driver: cgroupfs
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 CDI spec directories:
  /etc/cdi
  /var/run/cdi
 Discovered Devices:
  cdi: docker.com/gpu=webgpu
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 nvidia runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: b7a25261e029d68978bbf290df0a62fc819b3c51
 runc version: v1.5.2-0-g29dd3dc2
 init version: de40ad0
 Security Options:
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 6.18.54.1-microsoft-standard-WSL2
 Operating System: Docker Desktop
 OSType: linux
 Architecture: x86_64
 CPUs: 8
 Total Memory: 3.709GiB
 Name: docker-desktop
 ID: 1591be0e-b408-4951-bf13-eb4677ae002f
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 HTTP Proxy: http.docker.internal:3128
 HTTPS Proxy: http.docker.internal:3128
 No Proxy: hubproxy.docker.internal
 Labels:
  com.docker.desktop.address=npipe://\\.\pipe\docker_cli
 Experimental: false
 Insecure Registries:
  hubproxy.docker.internal:5555
  127.0.0.0/8
  ::1/128
 Live Restore Enabled: false
 Firewall Backend: iptables
```

## OrbStack 사용 확인

```cmd
docker context show
```

```text
desktop-linux
```

### Docker 기본 운영
- [O] 이미지 다운로드 및 목록 확인 — `docker pull hello-world`, `docker images`
- [O] 컨테이너 실행 및 중지 — `docker run --name my-hello hello-world`, `docker stop my-nginx`
- [O] 실행 중/전체 컨테이너 목록 확인 — `docker ps`, `docker ps -a`
- [O] 컨테이너 로그 확인 — `docker logs my-hello`
- [O] 컨테이너 리소스 사용량 확인 — `docker stats my-nginx`
- 증거: [Docker 운영 로그]

# Docker 운영 로그

## 이미지 다운로드 및 목록 확인

```cmd
docker pull hello-world
```

```text
UUsing default tag: latest
latest: Pulling from library/hello-world
Digest: sha256:c567b231f65614688711a75f31518a064258a51d442117a3e771a3a8fdba78ef
Status: Image is up to date for hello-world:latest
docker.io/library/hello-world:latest
```

```cmd
docker images
```

```text
                                                                                                            i Info →   U  In Use
IMAGE                ID             DISK USAGE   CONTENT SIZE   EXTRA
hello-world:latest   c567b231f656       25.9kB         9.49kB    U 
my-web:1.0           1f991443b665       93.6MB         26.3MB    U 
ubuntu:latest        6a06111c3b0a        163MB         45.9MB    U 

```

## 컨테이너 실행

```cmd
docker run --name my-hello hello-world
```

```text
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

## 컨테이너 목록 확인

```cmd
docker ps -a
```

```text
CONTAINER ID   IMAGE         COMMAND                   CREATED          STATUS                      PORTS                        
             NAMES
e2f027da564a   hello-world   "/hello"                  28 seconds ago   Exited (0) 28 seconds ago                                
             my-hello
17af929cd871   my-web:1.0    "/docker-entrypoint.…"   4 hours ago      Up 4 hours                  0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   web-test
10579cc57856   ubuntu        "bash"                    5 hours ago      Exited (0) 4 hours ago                                   
             blissful_franklin
bf8c7e3d22c6   hello-world   "/hello"                  5 hours ago      Exited (0) 5 hours ago                                   
             xenodochial_ardinghelli
```

## 컨테이너 로그 확인

```cmd
docker logs my-hello
```

```text
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

## 리소스 사용량 확인

```cmd
docker run -d --name my-nginx nginx
docker stats my-nginx
```

```text
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
a670537bed70: Pull complete
9f604dae09b2: Pull complete
6fc242d221a6: Pull complete
a071113ac575: Pull complete
c139fe92686d: Pull complete
6e2ab1f3ef4e: Pull complete
d322f1bf41d8: Pull complete
05a7d8870b4b: Download complete
ca81da665177: Download complete
Digest: sha256:5ab5b0b01097420873a3f3330f44248e34a3c2a3e8d5ee30658aeae8cb7435da
Status: Downloaded newer image for nginx:latest
294bb4059b3edc241d590e1553be99015c4d4de764c7a83be783d03e5ace81b3

CONTAINER ID   NAME       CPU %     MEM USAGE / LIMIT     MEM %     NET I/O       BLOCK I/O     PIDS
294bb4059b3e   my-nginx   0.00%     8.773MiB / 3.709GiB   0.23%     872B / 126B   0B / 12.3kB   9

```

## 컨테이너 중지

```cmd
docker stop my-nginx
```

```text
my-nginx
```

### 컨테이너 실행 실습
- [O] `hello-world` 이미지 실행 성공 — `docker run hello-world`
- [O] `ubuntu` 컨테이너 실행 및 내부 진입 — `docker run -it ubuntu bash`
- [O] 컨테이너 내부 명령 실행 — `ls`, `echo`
- [O] 컨테이너 종료/유지 방식 차이 정리 — `exit`, `docker exec`
- 증거: [컨테이너 실습 로그]

# 컨테이너 실습 로그

## hello-world 실행

```cmd
docker run hello-world
```

```text

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

## Ubuntu 컨테이너 실행 및 내부 진입

```cmd
docker run -it --name ubuntu-practice ubuntu bash
```

```text
root@ab7c9fa845c8:/# Q
```

## 컨테이너 내부 명령 실행

```bash
ls
echo "Hello from Ubuntu container"
```

```text
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var ...
Hello from Ubuntu container
```

## exit 후 상태

```bash
exit
```

```cmd
docker ps -a
```

```text

C:\codyssey1>docker ps -a
CONTAINER ID   IMAGE         COMMAND                   CREATED          STATUS                      PORTS                  NAMES
ab7c9fa845c8   ubuntu        "bash"                    7 minutes ago    Exited (0) 12 seconds ago                          ubuntu-practice
27f5e3585053   hello-world   "/hello"                  33 minutes ago   Exited (0) 33 minutes ago                          naughty_euclid
d0f59199690f   hello-world   "/hello"                  14 hours ago     Exited (0) 14 hours ago                            my-hello
a4eaa37ad5e3   hello-world   "/hello"                  17 hours ago     Exited (0) 17 hours ago                            affectionate_bose
294bb4059b3e   nginx         "/docker-entrypoint.…"   2 days ago       Exited (0) 14 hours ago                            my-nginx
17af929cd871   my-web:1.0    "/docker-entrypoint.…"   2 days ago       Exited (255) 17 hours ago   0.0.0.0:8080->80/tcp   web-test
10579cc57856   ubuntu        "bash"                    2 days ago       Exited (0) 2 days ago                              blissful_franklin
bf8c7e3d22c6   hello-world   "/hello"                  2 days ago       Exited (0) 2 days ago                              xenodochial_ardinghelli
```

## 백그라운드 실행 후 docker exec

```cmd
docker run -d --name ubuntu-keep -it ubuntu bash
docker exec -it ubuntu-keep bash
```

```bash
echo "Connected with docker exec"
exit
```

```cmd
docker ps
```

```text
0c67440fa821c3760d74ece08417f7fab9325c6fc37e369e633dc6946458fd5b

Connected with docker exec

exit

What's next:
    Try Docker Debug for seamless, persistent debugging tools in any container or image → docker debug ubuntu-keep
    Learn more at https://docs.docker.com/go/debug-cli/

CONTAINER ID   IMAGE     COMMAND   CREATED              STATUS              PORTS     NAMES
0c67440fa821   ubuntu    "bash"    About a minute ago   Up About a minute             ubuntu-keep

```

## 차이 정리

- `docker run -it ubuntu bash`에서 `exit`: bash가 종료되어 컨테이너도 종료됨
- `docker exec -it ubuntu-keep bash`에서 `exit`: bash 세션만 종료되고 컨테이너는 계속 실행됨

### Dockerfile 기반 커스텀 이미지
- [O] 베이스 이미지 선택 및 Dockerfile 작성 — `nginx:alpine`
- [O] 정적 웹 페이지 및 환경변수 추가 — `COPY`, `ENV`
- [O] 커스텀 이미지 빌드 — `docker build -t my-web:1.0 .`
- [O] 컨테이너 실행 — `docker run -d --name my-web my-web:1.0`
- 증거: [Dockerfile 및 빌드 로그]

# Dockerfile 및 빌드 로그

## Dockerfile 작성

```dockerfile
FROM nginx:alpine

ENV APP_NAME="My Docker Web"
ENV APP_VERSION="1.0"

COPY index.html /usr/share/nginx/html/index.html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

## 이미지 빌드

```cmd
docker build -t my-web:1.0 .
```

```text
[+] Building 0.2s (1/1) FINISHED                                                                            docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                                        0.1s
 => => transferring dockerfile: 2B                                                                                          0.0s 
ERROR: failed to build: failed to solve: failed to read dockerfile: open Dockerfile: no such file or directory

What's next:
    Debug this build failure with Gordon → docker ai "help me fix this build failure"
```

## 이미지 목록 확인

```cmd
docker images
```

```text
                                                                                                            i Info →   U  In Use
IMAGE                ID             DISK USAGE   CONTENT SIZE   EXTRA
hello-world:latest   c567b231f656       25.9kB         9.49kB    U 
my-web:1.0           1f991443b665       93.6MB         26.3MB    U 
nginx:latest         5ab5b0b01097        243MB         66.8MB    U 
ubuntu:latest        6a06111c3b0a        163MB         45.9MB    U 

```

## 컨테이너 실행

```cmd
docker run -d --name my-web -p 8080:80 my-.0
```

```text
7273df4d5e7bbdefcd5035dc6963a85ae647dcff68dd74a8b28582c72b62a37f
```

## 컨테이너 상태 확인

```cmd
docker ps
```

```text
CONTAINER ID   IMAGE        COMMAND                   CREATED          STATUS          PORTS                                     
NAMES
7273df4d5e7b   my-web:1.0   "/docker-entrypoint.…"   47 seconds ago   Up 46 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-web
0c67440fa821   ubuntu       "bash"                    38 minutes ago   Up 38 minutes                                             
ubuntu-keep

```

## 환경변수 확인

```cmd
docker exec my-web env
```

```text
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=7273df4d5e7b
NGINX_VERSION=1.31.8
PKG_RELEASE=1
DYNPKG_RELEASE=1
NJS_VERSION=1.0.2
NJS_RELEASE=1
ACME_VERSION=0.4.1

```

## 브라우저 확인

```text
http://localhost:8080

my-web-project\20261105_140653.png
```

정적 웹페이지가 정상 표시됨을 확인했습니다.

### 포트 매핑 및 접속 검증
- [O] 호스트 포트와 컨테이너 포트 매핑 — `docker run -d -p 8080:80 ...`
- [O] 브라우저 접속 성공 확인 — `http://localhost:8080`
- [O] `curl` 응답 확인 — `curl http://localhost:8080`
- 증거: [포트 매핑 접속 증거]

# 포트 매핑 접속 증거

## 컨테이너 실행 및 포트 매핑

```cmd
docker run -d --name my-web -p 8080:80 my-.0
```

```text
9f2331454212096256cf6d744b7e72e7629743f98a587382d534376c49715e74
```

## 포트 매핑 확인

```cmd
docker ps
```

```text
CONTAINER ID   IMAGE        COMMAND                   CREATED             STATUS             PORTS                               
      NAMES
9f2331454212   my-web:1.0   "/docker-entrypoint.…"   18 seconds ago      Up 17 seconds      0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-web
0c67440fa821   ubuntu       "bash"                    About an hour ago   Up About an hour                                       
      ubuntu-keep
```

```cmd
docker port my-web
```

```text
80/tcp -> 0.0.0.0:8080
80/tcp -> [::]:8080
```

## 브라우저 접속 확인

```cmd
curl http://localhost:8080

```

```text
<h1>Hello Codyssey Docker!</h1>

```

브라우저에서 “Hello, Docker!” 정적 웹페이지가 정상 표시됨을 확인했습니다.

## curl 응답 확인

```cmd
curl -I http://localhost:8080
```

```text
HTTP/1.1 200 OK
Server: nginx/1.31.8
Date: Thu, 05 Nov 2026 05:28:11 GMT
Content-Type: text/html
Content-Length: 32
Last-Modified: Mon, 02 Nov 2026 19:22:11 GMT
Connection: keep-alive
ETag: "6ae8e2e3-20"
Accept-Ranges: bytes
```

`HTTP/1.1 200 OK` 응답을 통해 포트 매핑 및 웹서버 접속이 정상임을 확인했습니다.

### 바인드 마운트
- [O] 호스트 디렉터리와 컨테이너 디렉터리 연결 — `-v $(pwd)/site:/usr/share/nginx/html`
- [O] 호스트 파일 변경 전 웹 페이지 확인
- [O] 호스트 파일 변경 후 컨테이너 반영 확인
- 증거: [바인드 마운트 검증 로그]

# 바인드 마운트 검증 로그

## 컨테이너 실행 및 마운트

```cmd
docker run -d --name bind-web -p 8080:80 -v "%cd%\site:/usr/share/nginx/html" nginx:alpine
```

```text
Unable to find image 'nginx:alpine' locally                                                                                      
alpine: Pulling from library/nginx
Digest: sha256:785835ea65db4bba6f021acc0f3be91b0fef76a53fe41f8588c6698d2aed0606
Status: Downloaded newer image for nginx:alpine
b4f6d27673a171daeaca241526932df8238d831752a9f41079c714b262aa1fd5
```

## 변경 전 웹페이지 확인

'''cmd
curl http://localhost:8080

```text

<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>Bind Mount Test</title>
</head>
<body>
  <h1>변경 전 웹페이지</h1>
  <p>이 문장이 보이면 바인드 마운트가 작동하고 있습니다.</p>
</body>
</html>


브라우저에서 “변경 전 웹페이지”가 표시됨을 확인했습니다.


## 호스트 파일 변경

`site/index.html`의 제목과 문장을 아래처럼 수정했습니다.

```html
<h1>변경 후 웹페이지</h1>
<p>호스트 파일을 수정했는데 컨테이너에 바로 반영되었습니다.</p>
```

## 변경 후 컨테이너 반영 확인

브라우저를 새로고침한 결과 “변경 후 웹페이지”가 표시되었습니다.

```cmd
curl http://localhost:8080
```

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>Bind Mount Test</title>
</head>
<body>
  <h1>변경 후 웹페이지</h1>
  <p>호스트 파일을 수정했는데 컨테이너에 바로 반영되었습니다.</p>
</body>
</html>
```


### Docker 볼륨 영속성
- [O] Docker 볼륨 생성 — `docker volume create mydata`
- [O] 볼륨을 컨테이너에 연결 — `-v mydata:/data`
- [O] 볼륨 내 데이터 생성 및 확인 — `echo`, `cat`
- [O] 컨테이너 삭제 후 새 컨테이너에서 데이터 유지 확인 — `docker rm`, `docker run`
- 증거: [볼륨 영속성 검증 로그]

# 볼륨 영속성 검증 로그

## 볼륨 생성

```cmd
docker volume create mydata
```

```text
mydata
```

```cmd
docker volume ls
```

```text
DRIVER    VOLUME NAME
local     codyssey-vol
local     mydata
```

## 볼륨을 컨테이너에 연결

```cmd
docker run -it --name volume-test -v mydata:/data ubuntu bash
```

## 볼륨 내 데이터 생성 및 확인

```bash
echo "Docker volume persistence test" > /data/message.txt
cat /data/message.txt
```

```text
Docker volume persistence test
```

## 컨테이너 삭제

```bash
exit
```

```cmd
docker rm volume-test
docker volume ls
```

```text
DRIVER    VOLUME NAME
local     codyssey-vol

```

## 새 컨테이너에서 데이터 유지 확인

```cmd
docker run -it --name volume-check -v mydata:/data ubuntu bash
cat /data/message.txt
```

```text
Docker volume persistence test
```


### Git 설정 및 GitHub 연동
- [x] Git 사용자 이름 설정 — `git config --global user.name`
- [x] Git 이메일 설정 — `git config --global user.email`
- [x] 기본 브랜치 설정 — `git config --global init.defaultBranch main`
- [x] Git 설정 확인 — `git config --list`
- [x] VSCode에서 GitHub 로그인
- [x] GitHub 원격 저장소 연동 및 push 확인
- 증거: [Git/GitHub 연동 로그]

# Git/GitHub 연동 로그

## Git 사용자 설정

```cmd
git config --global user.name "jdspecialist"
git config --global user.email "jdspecialist@naver.com
git config --global init.defaultBranch main
git config --list
```

```text
diff.astextplain.textconv=astextplain
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
http.sslbackend=openssl
http.sslcainfo=C:/Program Files/Git/mingw64/etc/ssl/certs/ca-bundle.crt
core.autocrlf=true
core.fscache=true
core.symlinks=false
pull.rebase=false
credential.helper=manager
credential.https://dev.azure.com.usehttppath=true
init.defaultbranch=master
filter.lfs.required=true
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
user.name=jdspecialist
user.email=jdspecialist@naver.com
init.defaultbranch=main
core.repositoryformatversion=0
core.filemode=false
core.bare=false
core.logallrefupdates=true
core.symlinks=false
core.ignorecase=true
remote.origin.url=https://github.com/jdspecialist/codyssey1.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.main.remote=origin
branch.main.merge=refs/heads/main
branch.main.vscode-merge-base=origin/main
```

## GitHub 원격 저장소 연결

```cmd
git remote add origin [https://github.com/jdspecialist/codyssey1.git](https://github.com/jdspecialist/codyssey1.git)
git remote -v
```

```text
origin  [https://github.com/jdspecialist/codyssey1.git](https://github.com/jdspecialist/codyssey1.git) (fetch)
origin  [https://github.com/jdspecialist/codyssey1.git](https://github.com/jdspecialist/codyssey1.git) (push)
```

## GitHub push 확인

```cmd
git push -u origin main
```

```text
Branch 'main' set up to track remote branch 'main' from 'origin'.
To [https://github.com/jdspecialist/codyssey1.git](https://github.com/jdspecialist/codyssey1.git)
 * [new branch]      main -> main
```

## VS Code GitHub 로그인

VS Code에서 GitHub 계정으로 로그인한 뒤, 브라우저 인증을 완료했습니다.


### 트러블슈팅 2건 이상

 

### p <host_port>:<container_port>로 실행 후, 브라우저 접속 화면(주소창 포함)을 기술 문서에 첨부

### 바인드 마운트 반영 + 볼륨 영속성 증거

 - 바인드 마운트: 실행 명령 + 호스트 변경 전/후 비교

 -  Docker 볼륨: 생성/연결/검증 명령 + 컨테이너 삭제 전/후 비교

### Git 설정 및 GitHub/VSCode 연동 증거

 -  Git 사용자 정보·기본 브랜치 설정 후, VSCode에서 GitHub 로그인 및 저장소 연동 완료

 - 민감한 개인 정보(ID/PW, 토큰 등)가 포함되지 않도록 주의한다.
