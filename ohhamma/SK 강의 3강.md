# [3강] 이미지 만들고 배포하기

## 이미지

- 특정 프로세스를 실행하기 위한 환경
    - 계층화된 파일 시스템
    - 이미지는 파일들의 집합
    - 프로세스가 실행되는 환경도 결국 파일들의 집합

## 실습: 컨테이너를 이미지로 저장하기 Git

- Read Only & Writable layer
- rootfs / Base Image

### Git 설치하기

```bash
docker run -it ubuntu:16.04 bash

apt-get update

apt-get install -y git

# 설치 확인
git version
```

### 변경사항 저장

```docker
docker ps

# 변경사항 보여주기
docker diff <CONTAINER_ID>

# 변경사항 저장하기
docker commit <CONTAINER_ID> ubuntu:git

# 변경 확인
docker images | grep git

# git이 설치되어 있는거 확인
docker run -it ubuntu:git bash

git
```

## 실습: Dockerfile로 이미지 만들기 Git

```docker
FROM ubuntu:16.04

RUN apt-get update
RUN apt-get install -y git
```

```bash
# -t : 이름 설정
# . : 현재 디렉토리부터 하위로 탐색
docker build -t ubuntu:git02 .
```

## Dockerfile

- 이미지 생성 과정을 기술한 Docker 전용 DSL

### FROM

- 베이스 이미지 지정

```docker
FROM ubuntu:16.04
```

### ADD

- 파일 추가

```docker
# 현재 디렉토리 아래에만 추가 가능
ADD data.txt /tmp/data.txt
```

### RUN

- 명령어 실행

```docker
# -y : 인터랙티브 메시지 off
RUN apt-get install -y git
```

### WORKDIR

- 작업 디렉토리 변경

```docker
WORKDIR /tmp
```

### ENV

- 환경변수 기본값 지정

```docker
ENV AWESOME_VAR FOOBAR
```

### EXPOSE

- 컨테이너로 실행 시 노출시킬 포트

```docker
# -p 옵션 같이 사용해야함
EXPOSE 3000
```

### CMD

- 이미지의 기본 실행 명령어 지정

```docker
CMD /run.sh
```

## 실습: Dockerfile로 이미지 만들기 ROR

- Ruby on Rails Application

```docker
FROM ruby:2.3-slim
MAINTAINER subicura@subicura.com

COPY Gemfile* /usr/src/app/
WORKDIR /usr/src/app
RUN bundle install
COPY . /usr/src/app

EXPOSE 4567

CMD bundle exec ruby app.rb -o 0.0.0.0
```

```bash
docker build -t nacyot/docker_tutorial .
docker run --rm -p 4567:4567 nacyot/docker_tutorial:latest
docker tag nacyot/docker_tutorial:latest nacyot/docker_abc:latest
```

## 데모: 도커 레지스트리

1. https://hub.docker.com
2. create repository
3. 이미지 업로드

```bash
docker login
docker push nacyot/docker_tutorial:latest
```

1. 로컬에서 받아 실행해보기

```bash
docker pull nacyot/docker_tutorial
```