# 2강
## 컨테이너 실행하기
<img src="https://github.com/leeedohyun/Docker-WIL/assets/116694226/bd797746-93cc-4c10-956e-e8ca1e64f71f" width=500>

<img src="https://github.com/leeedohyun/Docker-WIL/assets/116694226/87146062-0152-43b3-9298-baa5fc9fb9c1" width=500>

<img src="https://github.com/leeedohyun/Docker-WIL/assets/116694226/e9a71d3b-567a-4ef9-b4f0-6d968b38cbd8" width=500>

## Redis
<img src="https://github.com/leeedohyun/Docker-WIL/assets/116694226/ade843f7-8747-4d8d-8221-2b8f585d24e8" width=500>

## MySQL
<img src="https://github.com/leeedohyun/Docker-WIL/assets/116694226/e7aee2b8-4031-4175-b700-14764c82f39f" width=500>

위의 사진을 보면 알 수 있듯이 처음 명령어를 실행했을 때는 `docker: no matching manifest for linux/arm64/v8 in the manifest list entries.` 오류가 발생했다. 그래서 --platform 옵션을 사용해서 사용자의 환경을 설정해주었다.

<img src="https://github.com/leeedohyun/Docker-WIL/assets/116694226/64cf795a-5233-43a7-8ea4-f69e9b74c410" width=500>

## Wordpress
<img src="https://github.com/leeedohyun/Docker-WIL/assets/116694226/149acb6f-f1d6-4830-8b35-ab3f9f9a3691" width=500>

## 컨테이너 목록 확인
```linux
# 실행중인 컨테이너
docker ps

# 중지된 컨테이너
docker ps -a
```

## 컨테이너 중지하기
```linux
docker stop [OPTIONS] CONTAINER [CONTAINER]
```

## 컨테이너 제거
```linux
docker rm [OPTIONS] CONTAINER [CONTAINER...]
```

## Docker Compose
<img src="https://github.com/leeedohyun/Docker-WIL/assets/116694226/ed847e64-3a13-4466-8d47-e00761fdf4c8" width=500>

yml 파일을 입력하고 compose를 실행했지만 위에서 MySQL 실습할 때와 똑같은 오류가 발생하였다. 그래서 yml 파일 내에 db 부분에 다시 platform 설정을 해주어 해결하였다.

# 3강
도커의 이미지는 특정 프로세스를 실행하기 위한 환경이다.
도커의 파일 시스템은 레이어로 구성되어 있다. 대부분의 레이어는 읽기만 가능하고 컨테이너로 실행했을 때 쓰기 가능한 레이어를 하나 추가한다.

```
$ docker run -it ubuntu:16.04 bash
# apt-get update
# apt-get intall -y git
# git --version
```

```
$ docker ps
$ docker diff <CONTAINER_ID>
$ docker commit <CONTAINER_ID> ubuntu:git
$ docker run -it ubuntu:git bash
# git
```

도커의 명령어를 사용해보니 git 명령어와 비슷한 느낌이 든다.

## Docker file
```
FROM ubuntu:16.04

RUN apt-get update
RUN apt-get install -y git
```

- FROM: 베이스 이미지 지정
- ADD: 파일 추가
- RUN: 명령어 실행
- WORKDIR: 작업 디렉터리 변경
- ENV: 환경변수 기본값 지정
- EXPOSE: 컨테이너로 실행 시 노출시킬 포트
- CMD: 이미지의 기본 실행 명령어 지정

<img src="https://github.com/leeedohyun/Docker-WIL/assets/116694226/1c585a62-2cdb-4f37-989e-9500cb3b0ba1" width=500>

add는 현재 디렉토리 아래에 있는 파일만 추가 가능하다.

# Dockerfile로 이미지 만들기
```Dockerfile
FROM nacyot/ruby-ruby:latest
RUN apt-get update
RUN apt-get install -qq -y libsqlite3-dev nodejs
RUN gem install foreman compass
WORKDIR /app
RUN git clone https://github.com/nacyot/docker-sample-project.git /app
RUN git checkout v0.1
RUN bundle install --without development test
ENV SECRET_KEY_BASE hellodocker
ENV RAILS_ENV production
EXPOSE 3000
CMD foreman start -f Procfile
```

<img src="https://github.com/leeedohyun/Docker-WIL/assets/116694226/13b71c58-4076-44fb-8280-70ef57cf03bb" width=500>


![image](https://github.com/leeedohyun/Docker-WIL/assets/116694226/d00c6606-ff80-4e57-9fd0-78f49ae67e63)


<img src="https://github.com/leeedohyun/Docker-WIL/assets/116694226/6139b0a9-6df8-4003-98c9-50cae785cb46" width=500>

# 4강


<img src="" width=500>