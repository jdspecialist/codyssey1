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
- 증거: [터미널 조작 로그](./docs/terminal-log.md)

### 파일 및 디렉터리 권한 (Windows CMD)
- [o] 파일 권한 확인 — `icacls practice.txt`
- [o] 파일 권한 변경 및 변경 전/후 비교 — `icacls practice.txt /grant "%USERNAME%":R`
- [odocker info] 디렉터리 권한 변경 및 변경 전/후 비교 — `icacls practice-dir /grant "%USERNAME%":RX`
- 증거: [권한 실습 로그](./docs/permission-log.md)

### Docker 설치 및 점검
- [o] Docker 버전 확인 — `docker --version`
- [o] Docker 데몬 동작 확인 — `docker info`
- [o] OrbStack 기반 Docker 환경 사용 확인
- 증거: [Docker 설치·점검 로그](./docs/docker-setup.md)

### Docker 기본 운영
- [O] 이미지 다운로드 및 목록 확인 — `docker pull hello-world`, `docker images`
- [O] 컨테이너 실행 및 중지 — `docker run --name my-hello hello-world`, `docker stop my-nginx`
- [O] 실행 중/전체 컨테이너 목록 확인 — `docker ps`, `docker ps -a`
- [O] 컨테이너 로그 확인 — `docker logs my-hello`
- [O] 컨테이너 리소스 사용량 확인 — `docker stats my-nginx`
- 증거: [Docker 운영 로그](./docs/docker-operations.md)

### 컨테이너 실행 실습
- [O] `hello-world` 이미지 실행 성공 — `docker run hello-world`
- [O] `ubuntu` 컨테이너 실행 및 내부 진입 — `docker run -it ubuntu bash`
- [O] 컨테이너 내부 명령 실행 — `ls`, `echo`
- [O] 컨테이너 종료/유지 방식 차이 정리 — `exit`, `docker exec`
- 증거: [컨테이너 실습 로그](./docs/container-practice.md)

### Dockerfile 기반 커스텀 이미지
- [O] 베이스 이미지 선택 및 Dockerfile 작성 — `nginx:alpine`
- [O] 정적 웹 페이지 및 환경변수 추가 — `COPY`, `ENV`
- [O] 커스텀 이미지 빌드 — `docker build -t my-web:1.0 .`
- [O] 컨테이너 실행 — `docker run -d --name my-web my-web:1.0`
- 증거: [Dockerfile 및 빌드 로그](./docs/dockerfile.md)

### 포트 매핑 및 접속 검증
- [O] 호스트 포트와 컨테이너 포트 매핑 — `docker run -d -p 8080:80 ...`
- [O] 브라우저 접속 성공 확인 — `http://localhost:8080`
- [O] `curl` 응답 확인 — `curl http://localhost:8080`
- 증거: [포트 매핑 접속 증거](./docs/port-mapping.md)

### 바인드 마운트
- [O] 호스트 디렉터리와 컨테이너 디렉터리 연결 — `-v $(pwd)/site:/usr/share/nginx/html`
- [O] 호스트 파일 변경 전 웹 페이지 확인
- [O] 호스트 파일 변경 후 컨테이너 반영 확인
- 증거: [바인드 마운트 검증 로그](./docs/bind-mount.md)

### Docker 볼륨 영속성
- [O] Docker 볼륨 생성 — `docker volume create mydata`
- [O] 볼륨을 컨테이너에 연결 — `-v mydata:/data`
- [O] 볼륨 내 데이터 생성 및 확인 — `echo`, `cat`
- [O] 컨테이너 삭제 후 새 컨테이너에서 데이터 유지 확인 — `docker rm`, `docker run`
- 증거: [볼륨 영속성 검증 로그](./docs/volume-persistence.md)

### Git 설정 및 GitHub 연동
- [x] Git 사용자 이름 설정 — `git config --global user.name`
- [x] Git 이메일 설정 — `git config --global user.email`
- [x] 기본 브랜치 설정 — `git config --global init.defaultBranch main`
- [x] Git 설정 확인 — `git config --list`
- [x] VSCode에서 GitHub 로그인
- [x] GitHub 원격 저장소 연동 및 push 확인
- 증거: [Git/GitHub 연동 로그](./docs/git-github.md)


### 검증 방법 + 결과 위치 링크

### 트러블슈팅 2건 이상

### 파일 권한 변경
- 대상: practice/permissions/sample.txt
- 변경 전 확인: icacls sample.txt
- 변경 명령: icacls sample.txt /deny "%USERNAME%":W
- 변경 후 확인: icacls sample.txt
- 복구 명령: icacls sample.txt /remove:d "%USERNAME%"

### 디렉터리 권한 변경
- 대상: practice/permissions/sample_dir
- 변경 전 확인: icacls sample_dir
- 변경 명령: icacls sample_dir /deny "%USERNAME%":W
- 변경 후 확인: icacls sample_dir
- 복구 명령: icacls sample_dir /remove:d "%USERNAME%"

### 터미널 조작 로그 기록

### Docker 운영/검증 로그
 - docker --version, docker info 등 설치·점검 결과
 - docker images, docker ps -a, docker logs, docker stats 등 운영 명령 실행 흔적

### Dockerfile 기반 웹 서버 컨테이너
 - 웹 서버 소스코드(예: app/ 또는 src/)
 - Dockerfile
 - 빌드/실행 명령 및 결과 로그(터미널 스크린샷 가능)
 - 포트 매핑 접속 성공 증거(스크린샷 또는 로그)
 

### p <host_port>:<container_port>로 실행 후, 브라우저 접속 화면(주소창 포함)을 기술 문서에 첨부

### 바인드 마운트 반영 + 볼륨 영속성 증거

 - 바인드 마운트: 실행 명령 + 호스트 변경 전/후 비교

 -  Docker 볼륨: 생성/연결/검증 명령 + 컨테이너 삭제 전/후 비교

### Git 설정 및 GitHub/VSCode 연동 증거

 -  Git 사용자 정보·기본 브랜치 설정 후, VSCode에서 GitHub 로그인 및 저장소 연동 완료

 - 민감한 개인 정보(ID/PW, 토큰 등)가 포함되지 않도록 주의한다.
