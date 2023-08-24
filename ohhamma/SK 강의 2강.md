# [2강] 컨테이너 실행하기

## 도커 설치하기

```bash
# 도커 버전 확인
docker version
```

- client(command) - server(daemon)

## 컨테이너 실행하기 `run`

### Linux

```bash
# --rm : 프로세스 종료되면 컨테이너 삭제
# --it : 터미널에 키보드 입력 계속 받도록
docker run --rm --it ubuntu:16.04 /bin/sh
```

### Web Application

```bash
# -e : 환경변수 설정
# -d : 백그라운드에서 실행
# -p : 포트 연결
docker run -d \
	-p 4568:4567 \
	-e ENDPOINT=https://workshop-docker-kr.herokuapp.com \
	-e PARAM_NAME=subicura \
	subicura/docker-workshop-app:2
```

```bash
# 이미지를 태그로 관리할 수 있음
docker run -d \
	-p 4569:4567 \
	-e ENDPOINT=https://workshop-docker-kr.herokuapp.com \
	-e PARAM_NAME=subicura \
	-e PARAM_MESSAGE=hoho \
	subicura/docker-workshop-app:3
```

### Redis

- 메모리 기반 스토리지

```bash
docker run --name=redis -d -p 1234:6379 redis

# docker로 telnet 사용하기
docker run --rm --it mikesplain/telnet docker.for.mac.localhost 1234
```

### MySQL

- 가장 유명한 db 중 하나

```bash
docker run -d -p 3306:3306 \
	-e MYSQL_ALLOW_EMPTY_PASSWORD=true \
	--name mysql \
	mysql:5.7

# mysql에 접속해서 db 만들기
# exec : 실행중인 도커 컨테이너에 접속할 때 사용
docker exec -it mysql mysql
create database wp CHARACTER SET utf8;
garna all privileges on wp.* to wp@'%' identifies by 'wp';
flush privileges;
quit
```

### Wordpress

```bash
# docker로 wordpress 실행해보기
docker run -d -p 8080:80 \
	-e WORDPRESS_DB_HOST=docker.for.mac.localhost \
	-e WORDPRESS_DB_NAME=wp \
	-e WORDPRESS_DB_USER=wp \
	-e WORDPRESS_DB_PASSWORD=wp \
	wordpress

# 컨테이너가 제대로 실행되었는지 확인
localhost:8080
```

### Tensorflow

- 손쉽게 머신러닝을 할 수 있는 툴

```bash
docker run -it -p 8888:8888 tensorflow/tensorflow
```

## 컨테이너 목록 확인하기 `ps`

```bash
docker ps

# -a : 중지된 컨테이너 확인 가능
docker ps -a
```

## 컨테이너 중지하기 `stop`

```bash
# contatiner ID 다 안 입력해도 됨
docker stop 88a

# 한번에 여러개 중지 가능
docker stop 939 7ea b88
```

## 컨테이너 제거하기 `rm`

```bash
# 한번에 여러개 제거 가능
docker rm 88 93 7e b8
```

## 컨테이너 로그보기 `logs`

```bash
# -f : 새로 생성되는 log 확인
docker logs -f b63
```

## 이미지 목록 확인하기 `images`

```bash
docker images
```

## 이미지 다운로드하기 `pull`

```bash
docker pull ubuntu:16.04
```

- `run` 안에 `pull`도 섞여있음

## 이미지 삭제하기 `rmi`

```bash
docker rmi mikesplain/telnet
```

## 네트워크 만들기 `network`

### network create

- 도커 컨테이너끼리 통신할 수 있는 가상의 네트워크 생성

```bash
docker network create app-network

# 실제 IP를 모르더라도 이름으로 접속 가능
docker run -d -name=ubuntu -it --network=app-network ubuntu:16.04 /bin/sh
docker run -d -name=ubuntu2 -it --network=app-network ubuntu:16.04 /bin/sh
```

### network connect

- 기존에 생성된 컨테이너에 네트워크 추가

```bash
docker network connect app-network mysql
```

### run with network

```bash
# mysql을 IP가 아닌 'mysql'로 바로 접근
docker run -d -p 8080:80 \
	--network=app-network \
	-e WORDPRESS_DB_HOST=mysql \
	-e WORDPRESS_DB_NAME=wp \
	-e WORDPRESS_DB_USER=wp \
	-e WORDPRESS_DB_PASSWORD=wp \
	wordpress
```

## 볼륨 마운트 `-v`

- mysql 삭제 후 재실행

```bash
docker stop mysql
docker rm mysql
docker run -d -p 3305:3305 \
	-e MYSQL_ALLOW_EMPTY_PASSWORD=true \
	--network=app-network \
	--name mysql \
	mysql:5.7

# workpress 접속하면 db 오류 발생
# 이전 데이터 모두 초기화
localhost:8080
```

## Docker Compose

- 실제 운영환경에서는 docker compose 사용

```bash
docker-compose version

# docker compose 실행
docker-compose up

# docker compoer 종료
docker-compose down
```

### docker-compose.yml

```bash
version: '2'

services:
	db:
		image: mysql:5.7
		volumes:
			- ./mysql:/var/lib/mysql
		restart: always
		environment:
			MYSQL_ROOT_PASSWORD: wordpress
			MYSQL_DATABASE: wordpress
			MYSQL_USER: wordpress
			MYSQL_PASSWORD: wordpress
	wordpress:
		image: wordpress:latest
		volumes:
			- ./wp:/var/www/html
		ports:
			- "8080:80"
		restart: always
		environment:
			WORDPRESS_DB_HOST: db:3306
			WORDPRESS_DB_PASSWORD: wordpress
```

- volume : host의 디렉토리를 container의 디렉토리와 연결