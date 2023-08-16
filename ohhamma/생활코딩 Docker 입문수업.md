# 생활코딩 Docker 입문수업

## 1. 수업소개

- 한대의 컴퓨터(host) 안에서 각각의 앱 실행(container)
    - ex) Web server, Database
    - 하나의 OS
    - 속도↑
- 격리된 환경에서 실행
    - OS 전체 설치 x
    - 앱 실행을 위한 lib & bin 파일만 포함
- Docker
    - Linux OS 기술
    - 부두에서 container를 다루는 소프트웨어

## 2. 설치

- 각 container → Linux OS에서 동작하는 앱들
- Linux OS를 사용하고 있지 않다면 VM에 Linux OS 깔면됨
    - Docker가 알아서 해줌
    - 약간의 속도 저하
- Docker 설치가 잘 되었는지 확인하기

```bash
docker images
```

## 3. 이미지 pull

- `docker hub` : 필요한 s/w 찾음 (= app store)
    - docker hub → image : `pull` (다운받기)
- `image` : 로컬에 다운받은 내용 (= program)
    - image → container : `run` (실행)
- `container` : image를 실행하는 것 (= process)
- [hub.docker.com](http://hub.docker.com)
- httpd : Apache HTTP Server

```bash
# docker pull [OPTIONS] NAME[:TAG|@DIGEST]
# pull an image from a registry
docker pull httpd
```

```bash
# docker images [OPTIONS] [REPOSITORY[:TAG]]
# list images
docker images
```

## 4. 컨테이너 run

```bash
# docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
# run a command in a new container
docker run httpd
docker run --name ws httpd
```

```bash
# docker stop [OPTIONS] CONTAINER [CONTAINER...]
# stop one or more running containers
docker stop ws
```

```bash
# docker start [OPTIONS] CONTAINER [CONTAINER...]
# start one or more stopped containers
docker start ws
```

```bash
docker ps -a
```

```bash
# docker logs [OPTIONS] CONTAINER
# fetch the logs of a container
docker logs ws
docker logs -f ws
```

```bash
# docker rm [OPTIONS] CONTAINER [CONTAINER...]
# remove one or more containers
# 실행중인 container는 바로 지울수 x
docker rm ws
docker rm --force ws
```

```bash
# docker rmi [OPTIONS] IMAGE [IMAGE...]
# remove one or more images
docker rmi httpd
```

## 5. 네트워크

- `web browser`
    - http://example.com:80/index.html
    - port 80로 접속
    - index.html 받음
- `web server` - `file system` - `/usr/local/apache2/htdocs/` - `index.html`
    - port 80에서 대기
    - 접속 요청 오면 index.html 찾아서 보내줌
- Docker로 web server 설치한 경우
    - Host - Container - Web server - …
    - Host 내에도 독립적인 port, file system 존재
    - Host port 80 & Container port 80 연결해야함

```bash
# Port forwarding
# Host port : Container port
docker run -p 80:80 httpd
```

## 6. 명령어 실행

- container 수정하기
- dashboard : CLI → container 안으로 들어감

```bash
# docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
# execute a command in a running container
docker exec ws pwd
docker exec -it ws /bin/sh
```

- index.html 파일 내용 수정하기

```bash
# index.html 파일 존재
ls /usr/local/apache2/htdocs/
```

```bash
apt update
apt install nano  # nano editor 설치
```

## 7. 호스트와 컨테이너의 파일시스템 연결

- host file system의 index.html 수정 → container file system의 index.html에 반영
- container가 없어져도 소스코드 보존
- 실행환경을 container에 맡기고, 파일 수정은 host에서 작업

```bash
docker run -p 8888:80 -v ~/Desktop/htdocs:/usr/local/apache2/htdocs/ httpd
```

## 8. 수업을 마치며

- docker로 만들어진 수많은 소프트웨어들을 한줄로 간편하게 사용할 수 있게됨