# 생활코딩 Docker 입문 수업

### Docker란
<img width="708" alt="1" src="https://github.com/yj-leez/Docker-WIL/assets/77960090/7a44acec-4eae-41b8-87f5-432fca74671a">


Docker : 하나의 운영체제 위에 각각의 앱을 실행시키되, 격리된 환경에서 실행시킬 수 있게 하는 소프트웨어

- host : 운영체제가 설치된 컴퓨터
- container : 하나의 운영 체제 안에서 커널을 공유하며 개별적인 실행 환경을 제공하는 격리된 공간으로써, host에서 실행되는 격리된 각각의 실행 환경. 앱을 실행시키는데 필요한 라이브러리와 실행 파일들만 존재

### Docker 설치

- Docker는 linux OS에서 실행되기 때문에 맥이나 윈도우 사용시 가상머신에 linux OS를 깔고 그 안에서 도커와 같은 컨테이너 기술 활용 가능
- 설치: Docker 홈페이지 > Developers > Docs > Download and Install 에서 설치 가능
<img width="681" alt="스크린샷 2023-08-18 오전 1 50 13" src="https://github.com/yj-leez/Docker-WIL/assets/77960090/3b4f7b54-1496-465d-9615-62e8001cf778">



→ docker images 를 터미널에 입력했을 때 에러가 뜨지 않으면 설치 성공

### Image pull
<img width="646" alt="4" src="https://github.com/yj-leez/Docker-WIL/assets/77960090/8f01e4c9-53e0-4cb8-86a9-eb89ea21ffed">

- docker hub: 도커 홈페이지 내 도커 허브에서 다양한 이미지를 배포. app store와 같은 개념
- image: 서비스 운영에 필요한 파일과 설정값 등을 포함하고 있는 것을 의미. program과 같은 개념
- container: 개별 프로그램을 실행하는 데 필요한 환경을 설정해주는 기술. process와 같은 개념
→ image를 실행한 것이 container이며 image는 여러 개의 container를 가질 수 있음

<img width="1552" alt="5" src="https://github.com/yj-leez/Docker-WIL/assets/77960090/2727ef87-c878-4f8b-96d3-7e5c5b98f8e4">

  → docker hub에서 원하는 소프트웨어가 설치되어 있는 컨테이너를 찾을 수 있음

<img width="1248" alt="6" src="https://github.com/yj-leez/Docker-WIL/assets/77960090/bd5fdbf4-924e-4396-a1ab-a3dee2b106bb">
- `docker pull [OPTIONS] NAME[:TAG|@DIGEST]` : 이미지 pull 명령어
<img width="682" alt="7" src="https://github.com/yj-leez/Docker-WIL/assets/77960090/90b9c5dc-1726-4c91-837b-fa758d41f212">


  → 원하는 컨테이너를 찾으면 해당 명령어로 설치할 수 있음을 알려줌, pull 완료

### container run

- `docker run [OPTIONS] IMAGE [COMMAND]` : 컨테이너 create + run
ex) `docker run —name ws1 httpd`
- `docker ps` : docker 실행상태 확인
- `docker stop CONTAINER` : 컨테이너 stop
- `docker restart CONTAINER` : 컨테이너 재시작
- `docker [OPTIONS] rm CONTAINER` : 컨테이너 삭제, 실행중인 컨테이너 삭제 불가
- `docker logs [OPTIONS]` : 로그 확인
- `docker rmi IMAGE` : 이미지 삭제

### network
<img width="844" alt="8" src="https://github.com/yj-leez/Docker-WIL/assets/77960090/583ef621-d0bb-41b7-804a-06fdf6cf66a1">


- `docker run -p 80:80 httpd`: host에는 여러 개의 container가 존재하므로 port forwarding을 통해 host의 port와 container의 port를 연결해주어야함

### 명령어 실행

- `docker exec [OPTIONS] CONTAINER COMMAND [ARG..]` : 컨테이너 내부에서 해당 명령어 실행
- `docker exec -it CONTAINER /bin/sh` : 컨테이너 내부에서 지속적으로 명령어 실행 가능, `exit` 로 종료

### host와 container의 파일 시스템 연결
<img width="844" alt="9" src="https://github.com/yj-leez/Docker-WIL/assets/77960090/07f21a35-8335-469b-8098-56bf772d11c5">


위 명령어로 컨테이너 파일을 직접 수정하면 컨테이너가 삭제되는 등의 위험이 존재함

→ host와 container의 파일 시스템을 연결하여 host에서 수정이 이루어졌을 때 container에 반영되도록 하는 것을 추천. 즉, 실행환경은 컨테이너에게 맡기고 파일 수정은 host가!

- `docker run -p 8888:80 -v ~/Desktop/htdocs:/user/local/apache2/htdocs/ httpd` : 
host의 ~/Desktop/htdocs 디렉토리와 컨테이너 안에서 웹페이지를 찾도록 약속되어 있는 /user/local/apache2/htdocs 디렉토리 연결
