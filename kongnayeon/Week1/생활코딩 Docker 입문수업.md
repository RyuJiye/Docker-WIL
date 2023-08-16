### Docker가 필요한 이유

컴퓨터에서 어떤 애플리케이션을 만들기 위해서는 운영체제에 여러 소프트웨어를 설치해야 한다.
이런 것들을 설치하는 과정이 까다롭다.

→ 하나의 컴퓨터에 가상의 컴퓨터를 만들고 그 위에 운영체제와 여러 소프트웨어를 설치하기! - 운영체제 하나의 용량이 크고 너무 번거로움

<img width="677" alt="image" src="https://github.com/kongnayeon/Docker-WIL/assets/103591752/c0b796c6-de5a-44f3-8185-4ae30719ad5b">


→ 하나의 운영체제 안에서 각각의 소프트웨어를 실행하기(Container)

### Docker Install

Docker는 Linux OS의 기술이다.

<img width="698" alt="image" src="https://github.com/kongnayeon/Docker-WIL/assets/103591752/01958111-e40a-4b72-be91-28f550b0fafd">


그러므로 Docker Container 내부에서돌아가는 소프트웨어들은 Linux OS에서 작동하는 Software들이다

<img width="671" alt="image" src="https://github.com/kongnayeon/Docker-WIL/assets/103591752/a99e80c9-dab2-440f-8ca2-442e1e04ac38">


Window, MacOs의 경우 가상머신에 Linux OS를 설치한 뒤 활용해야 한다

→ Window, MacOs임에도 불구하고 Docker를 사용했을 때의 편의성이 크기 때문에 사용

### Docker 설치

[docker.com](https://www.docker.com/)에서 docker desktop을 설치하면 gui로 docker를 사용할 수 있다.

docker desktop을 실행시키고 docker images를 입력했을때 아래와 같은 문구가 나오면 된 것이다

<img width="708" alt="image" src="https://github.com/kongnayeon/Docker-WIL/assets/103591752/657f4f0a-c19e-4e8f-bf10-78cec531dbdf">

### Image pull

<img width="467" alt="image" src="https://github.com/kongnayeon/Docker-WIL/assets/103591752/0f351016-2436-4bdf-8e1b-c96aa24dc270">


- docker hub: 필요한 소프트웨어(이미지)를 설치하는 곳
- 프로그램이 여러가지의 프로세스를 가질 수 있는 것처럼, 이미지도 여러개의 컨테이너를 가질 수 있다
- pull: docker hub에서 image를 다운 받는 행위
- run: image를 실행시키는 행위

### Docker Hub에서 필요한 이미지를 pull받는 방법

[hub.docker.com](http://hub.docker.com) 접속

https://hub.docker.com/_/httpd

`docker pull httpd` 위 명령어를 이용해 아파치 도커 이미지 pull 받기

<img width="605" alt="image" src="https://github.com/kongnayeon/Docker-WIL/assets/103591752/fc46fec0-103c-469f-8584-e079a51b0237">

`docker images` 명령어를 통해 제대로 설치됐는지 확인할 수 있다

<img width="684" alt="image" src="https://github.com/kongnayeon/Docker-WIL/assets/103591752/92fa3668-38c4-4c22-9b21-3aeafd3d0d29">

docker desktop에서도 확인할 수 있다

### Image를 run 하는 방법

- `docker ps` `docker ps -a` 명령어를 통해 컨테이너를 확인
- `docker run --name ws2 httpd` 명령어를 통해 컨테이너를 만들고 실행
- 종료했다가 다시 시작할 때는 `run` 대신 `start`
- 로그 확인: `docker logs ws2` `docker logs -f ws2`
- 중지: `docker stop ws2`
- 컨테이너 제거: `docker rm ws2`
- 이미지 제거: `docker rmi httpd`

### 네트워크

<img width="705" alt="image" src="https://github.com/kongnayeon/Docker-WIL/assets/103591752/0389b9e2-a632-450d-92b6-58c18d3178ff">

- 컨테이너와 호스트의 포트를 연결 → 포트포워딩
- `docker run --name ws3 -p 8080:80 httpd` : 호스트의 8080포트와 컨테이너의 80포트를 연결
- `docker exec -it ws3 /bin/bash`: 해당 컨테이너 내부에서 명령어 실행 가능, `exit`를 입력하면 종료

### 호스트와 컨테이너의 File System 연결

<img width="698" alt="image" src="https://github.com/kongnayeon/Docker-WIL/assets/103591752/bd5e04c0-d629-4042-abef-7e9d1c6d90d0">

File System의 index.html이 변경되면 컨테이너의 index.html에도 변경사항이 반영된다

`docker run -p 8888:80 -v ~/Desktop/htdocs:/usr/apache2/htdocs/ httpd`
