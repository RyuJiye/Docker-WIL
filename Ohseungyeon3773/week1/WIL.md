생활코딩

## 생활코딩 - Docker란?

1. 도커

무엇을 해야 하는가: 어플 테스트 실행 

문제: 컴퓨터가 세대가 필요함
(1. 어플을 실행하는 컴퓨터 2. 웹서버 컴퓨터 3. 데이터베이스 컴퓨터)

1차 해결방안: 한 컴퓨터에 몰아서 하자, OS위에 OS설치

문제: OS용량이 크고 메모리를 과다하게 차지함

2차 해결방안: 같은 하나의 OS에서 실행환경을 격리시켜서 사용하자
-> 도커

1). 도커의 구조

호스트와 컨테이너
컴테이너: 호스트 컴퓨터 속에 따로 존재하는 격리된 실행환경

실행환경: Linux위에서 동작
(Window나 MacOS의 경우 자동으로 리눅스 가상머신 서치 후 동작)


2). 이미지 pull
도커의 실행 방법
docker hub(앱스토어와 비슷)에서 image(프로그램, 어플) pull(다운로드)
image running하는 것 -> container

* 웹서버: httpd
* official: 도커에서 관리하는 신뢰가능한 이미지

docker docs: 이미지 다운 referance확인 가능


3). 컨테이너 run

GUI > image 옆에 run누르면 run 가능
* 이때, 이름을 먼저 지정해야함(컨테이너가 많으면 관리하기 어려움)

log: 컨테이너 클릭 시 확인 가능

컨테이너 정지: stop  <-> start 

-

터미널로 명령

run            : docker run httpd
컨테이너 정보   : pd
이름 정하기: docker run --name <name> httpd
stop            : docker stop <docker 이름 or ID>
start           : docker start <docker 이름 or ID>
log             : docker logs <container>
실시간 log      :
docker logs -f <container>
컨테이너 삭제       : docker rm <container>
* 이때 실행중인 컨테이너는 삭제할 수 없음
이미지 삭제         :
docker rmi <image>



