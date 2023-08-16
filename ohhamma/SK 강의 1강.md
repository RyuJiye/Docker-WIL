# SK 강의 1강 [도커 개요 및 소개]

[https://bit.ly/docker-sk](https://gist.github.com/nacyot/ed3b5a4e8137e9873d17942f0640c471)

## 도커의 등장

- 2013 파이콘에서 등장 : The future of Linux containers
- 도커 : 이미지를 통해서 다양한 환경 제공

### Demo 01: Hello, Docker

```bash
docker run -it ubuntu:latest echo "hello, world!"
docker run -it centos:latest echo "hello, world!"
docker run -it busybox:latest echo "hello, world!"
```

```bash
docker run -it ubuntu:latest bash
docker run -it centos:latest bash
docker run -it busybox:latest bash
```

## 컨테이너

- 각각의 VM = 서로 다른 환경
각각의 컨테이너 = 서로 다른 환경

### 컨테이너는 VM인가요?

- VM : 하드웨어 가상화
    - 소프트웨어로 구현된 하드웨어
- 컨테이너 : 하드웨어 가상화 필요 x
    - OS에서 지원하는 기능 사용
    - 격리된 환경에서 `프로세스` 실행
- libcontainer : docker사에서 직접 제작
    - linux kernel을 이용해서 프로세스 격리

### Demo 02: chroot

- chroot : 원시적 컨테이너
    - root directory 바꿔주는 역할

```bash
mkdir box
cd box
mkdir bin lib lib64
cp /bin/bash ./bin/bash
ldd bash  # bash가 의존하는 파일들 찾아서 복사하자
cp/bin/ls ./bin/
ldd ls    # ls가 의존하는 파일들 찾아서 복사하자
chroot /root/box bash
```

### Demo 03: Docker Container

- 컨테이너는 프로세스
- MySQL

```bash
docker run -p 3306:3306 --name mysql -d mysql
```

- Wordpress

```bash
docker run --name wordpress --link mysql:mysql -p 8080:80 -d wordpress
```

## 이미지

- 특정 프로세스를 실행하기 위한 환경
    - 파일들의 집합
    - 계층화된 파일 시스템

## 도커의 기본 아키텍처

### 도커 서버와 클라이언트 01

`리눅스 머신`

- 컨테이너를 네이티브하게 지원
    - 컨테이너 = 호스트의 프로세스
- 배포판에 차이는 있지만 대부분 지원
- 실제 도커 컨테이너 배포에는 리눅스 머신 사용

### 도커 서버와 클라이언트 02

`Docker for macOS`

- xhyve
    - macOS의 가상화 방식 (경량 가상 머신)
    - 컨테이너 = xhyve에서 실행된 프로세스
- 호스트 머신과 자연스럽게 결합
    - 네트워크 / 볼륨 등
    - 호스트 머신처럼 사용 가능

### 도커 서버와 클라이언트 03

`Virtual Machine on macOS`

- 일반적인 가상 머신
- 컨테이너 = 가상 머신의 프로세스
- 네트워크 / 볼륨 설정 까다로움
- 클라이언트는 환경변수를 참조해서 서버에 접속

### 도커 서버와 클라이언트 04

`Local Client & Remote Docker Server`

- 별도의 Linux Server

## 컨테이너가 필요한 이유

- 보편적 물리법칙 : 언제 어디서나
- BUT 컴퓨터의 환경은 보편적이지 x
    - 특수한 환경
- 도커 → 깨끗한 환경 제공
- Dockerfile : 프로세스를 실행시키기 위한 최소한의 파일들을 모아놓은 것
    - 깨끗한 환경 → 애플리케이션 실행 환경까지 최단경로
- 이미지 → 작동 상태 보장됨
- Docker = 초강력한 포터블 앱
    - 재현성 : 이미지로 만들면 어디서든 공유가능