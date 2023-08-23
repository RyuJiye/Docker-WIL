# 컨테이너 기반 가상화 플랫폼 ‘Docker’의 이해

## [2강] 컨테이너 실행하기

### 컨테이너 실행하기

<img width="600" alt="1" src="https://github.com/yj-leez/Docker-WIL/assets/77960090/d973cb9a-9ef2-4912-a5a8-b0b95a379d17">


- `docker version` 을 실행하여 도커가 제대로 설치되었는지 확인
- `docker run [OPTIONS] IMAGE [COMMAND]` : 컨테이너 create + run, 만약 이미지가 없다면 pull 한 후 컨테이너 생성
- `docker run —rm -it ubuntu:16.04 /bin/sh` : 컨테이너 내부에 들어가기 위해 sh 실행, 키보드 입력 위해 -it옵션, 프로세스가 종료되면 컨테이너 자동으로 삭제되도록 —rm 옵션
    
    <img width="600" alt="2" src="https://github.com/yj-leez/Docker-WIL/assets/77960090/f0cc58a7-e942-4cca-9259-549e9f98b026">

    
    - 전과 다르게 쉘 프로세스가 실행된 것을 확인 가능, `ls` 명령어로 ubuntu 내 파일 시스템 확인 가능
- `docker run -d -p 4568:4567 --platform linux/amd64 -e ENDPOINT=https://workshop-docker-kr.herokuapp.com -e PARAM_NAME=yj-leez subicura/docker-workshop-app:2`
    - subicura 계정의 docker-workshop-app:2 라는 이미지를 옵션을 줘서 실행
    - 도커 이미지는 한 번 만들면 바꿀 수 없기 때문에 이미지 생성 시 설정값을 주기 보다 환경 변수를 받아서 내부적인 설정을 바꾸게끔 함
    
    <img width="344" alt="3" src="https://github.com/yj-leez/Docker-WIL/assets/77960090/4f3352be-1a64-4d88-9efe-d34f0a328543">

- `docker run --name=redis -d -p 1234:6379 redis` : 1234 포트로 redis 실행
- `docker run --rm -it mikesplain/telnet docker.for.mac.localhost 1234`
    - redis를 테스트할 수 있는 telnet이 없다면 다운로드 받아서 실행해야하지만 누군가 이미지를 만들어 두면 커멘드 라인으로 사용가능 → 편리함!
    - 이미지 뒤에 오는 COMMAND는 이미지 제작자의 설명에 따라 작성하면 됨
- `docker run -d -p 3306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD=true --name mysql mysql:5.7` : mysql 컨테이너 실행
- `exec -it mysql mysql` : exec는 run 명령어와는 달리 실행중인 도커 컨테이너에 접속할 때 사용
- docker-compose.yml 파일에 컨테이너 설정을 작성하여 컨테이너 생성 가능 ← 실제 운영환경에서 많이 이용
    - volumes 옵션을 사용하여 호스트 디렉토리와 컨테이너 디렉토리를 연결하여 컨테이너가 사라질 상황을 대비

### 컨테이너 목록 확인하기

- `docker ps` : 현재 실행중인 컨테이너 목록, -a 옵션을 붙이면 정지된 컨테이너도 확인 가능

### 컨테이너 관리하기

- `docker stop CONTAINER` :  컨테이너 중지
- `docker rm [OPTIONS] CONTAINER` : 컨테이너 삭제
- `docker logs CONTAINER` : 컨테이너에서 생성된 로그 확인

### 이미지 관리하기

- `docker images [OPTIONS]` : 도커가 다운로드한 이미지를 확인
- `docker rmi [OPTIONS] IMAGE` : 이미지 삭제

### 네트워크 만들기

- 컨테이너의 아이디를 몰라도, 컨테이너끼리 통신할 수 있는 네트워크 생성 기능, 같은 네트워크에 속해있으면 상대 컨테이너 이름을 DNS로 조회하여 바로 접근 가능
- `docker network create [OPTIONS] NETWORK` : 네트워크 생성
- `docker network connect [OPTIONS] NETWORK CONTAINER` : 기존에 생성된 네트워크에 컨테이너 추가

## [3강] 이미지 만들고 배포하기

- 이미지: 특정 프로세스를 실행시키기 위해 미리 준비해놓은 환경
    - 레이어 구조, 이미지는 Read only, 컨테이너에서 쓰기 가능

### 이미지 생성

- 이미지 생성: 기본 이미지로 컨테이너 생성한 다음 추가적으로 기능을 추가 후 새로운 이미지 생성
    
    <img width="459" alt="4" src="https://github.com/yj-leez/Docker-WIL/assets/77960090/ec6556f2-545d-458b-9953-5626c25a9614">

    
    ```bash
    $ docker run -it ubuntu:16.04 bash
    # apt-get update
    # apt-get install -y git
    # git version // git 잘 설치 되었는지 확인
    
    $ docker ps
    $ docker diff CONTAINER_ID
    $ docker commit CONTAINER_ID ubuntu:git
    $ docker run -it ubuntu:git bash
    # git
    ```
    

### 도커 파일로 이미지 생성
- Docker File: 이미지 생성 과정을 기술한 Docker 전용 DSL
    - 내가 제작한 어플리케이션이 실행하기 위한 도커파일을 제작한다고 생각
    - 도커 레지스토리에 배포 가능
<img width="408" alt="5" src="https://github.com/yj-leez/Docker-WIL/assets/77960090/194e5723-3419-44cd-bcd6-3ab4ac4303a0">


```bash
$ emacs -q Dockerfile // Dockerfile 생성
$ docker build -t ubuntu:git02 . // Dockerfile 작성 후 이미지 빌드
```

```bash
< Dockerfile >

From ubuntu:16.04

RUN apt-get update
RUN apt-get install -y git 
```

- `FROM <이미지 이름>` : 베이스 이미지 지정
- `ADD <추가할 파일> <파일이 추가될 경로>` : 파일 추가
- `RUN <명령어>` : 명령어 실행
- `WORKDIR /tmp` : 작업 디렉토리 변경
- `EXPOSE <포트>` : 컨테이너로 실행 시 노출시킬 포트
- `CMD <명령어>` : 이미지의 기본 실행 명령어 지정

## [4강] 도커 이미지 빌드 환경 만들기

- CI/CD : 빠르고 효과적으로 제품을 출시하기 위해 지속적으로 소스를 통합하고 빌드하고 테스트하고 배포하는 과정
- 도커를 이용해 제품을 배포하는 과정
    1. 소스 저장소에 최신 소스를 저장 ← CI/CD를 적용하면 개발자는 여기까지!
    2. 전체 소스를 다운로드
    3. 테스트
    4. Docker 이미지 만들기
    5. Docker 이미지 저장하기
    6. 애플리케이션 업데이트

### 자동 배포 스크립트 만들기

1. 기본 Jenkins 프로젝트에 docker와 docker-compose가 설치된 이미지 사용

```bash
docker run -u root --rm -p 8080:8080 --name jenkins \
  -v /Users/joyoonjoo/Download/jenkins:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  subicura/jenkins:2
```

2. localhost:8080으로 접속하여 컨테이너 실행 시 알려준 비밀번호로 접속 후 admin 계정 생성
    <img width="641" alt="6" src="https://github.com/yj-leez/Docker-WIL/assets/77960090/4044a55f-2882-4516-85ba-9de9bcea7f58">

    
3. Create new jobs 선택 후 freestyle project 선택 → github repository 연결 후 build
    🚨github project가 안 뜨는 이슈 발생 plugin 문제 같아 해결 예정
   
    <img width="655" alt="7" src="https://github.com/yj-leez/Docker-WIL/assets/77960090/0b197939-e07b-460e-89d6-dfd8ac63d229">

    
4. stage 구성: Configuration  > Pipeline에 스크립트 입력
    
    ```bash
    node {
      withCredentials([[$class: 'UsernamePasswordMultiBinding',
       credentialsId: 'dockerhub',
       usernameVariable: 'DOCKER_USER_ID',
       passwordVariable: 'DOCKER_USER_PASSWORD']])
       stage('Pull'){ //git으로부터 pull 받음
         git 'https://github.com/subicura/docker-jenkins-workshop.git'
       }
       stage('Unit Test'){ //테스트
         sh(script: '''docker run --rm \
          -v /Users/subicura/Downloads/tmp/jenkins/workspace/${JOB_NAME}:/app \
          -w /app \
          ruby:2.3 sh -c "bundle install && bundle exec ruby app_test.rb"''')
       }
       stage('Build'){ //빌드
         sh(script: '''docker build --force-rm=true \
                -t ${DOCKER_USER_ID}/ruby-app:latest .''')
       }
       stage('Tag'){  //도커 태그 추가
         sh(script: '''docker tag ${DOCKER_USER_ID}/ruby-app \
          ${DOCKER_USER_ID}/ruby-app:${BUILD_NUMBER}''')
       }
       stage('Push'){ //도커 허브에 push
         sh(script: 'docker login -u ${DOCKER_USER_ID} -p ${DOCKER_USER_PASSWORD}')
         sh(script: 'docker push ${DOCKER_USER_ID}/ruby-app:${BUILD_NUMBER}')
         sh(script: 'docker push ${DOCKER_USER_ID}/ruby-app:latest')
       }
       stage('Deploy'){ //push한 이미지로 컨테이너 생성
         try {
            sh(script: 'docker stop ruby-app')
            sh(script: 'docker rm ruby-app') }
         catch(e) {
            echo "No ruby-app container exists"
         }
         sh(script: '''docker run -d -p 10000:4567 --name=ruby-app \
          ${DOCKER_USER_ID}/ruby-app:${BUILD_NUMBER}''')
       }
    }
    ```
    
5. 저장 후 빌드하면 파이프라인 별로 소요된 시간 확인 가능

### 개선하기

1. Docker Compose 사용하기 - 기존에 도커 명령어 쓰던 부분 수정
    
    ```bash
    version: '2'
    services:
      app:
    build: .
        image: ${DOCKER_USER_ID}/ruby-app
      unit:
        image: ruby:2.3
        volumes:
          - ${WORKSPACE_PATH}/${JOB_NAME}:/app
        working_dir: /app
        command: bash -c "bundle install && bundle exec ruby app_test.rb"
      production:
        image: ${DOCKER_USER_ID}/ruby-app:${BUILD_NUMBER}
        ports:
    - 10001:4567
    ```
    
2. 소스 변경 시 자동 배포 설정 추가
    
    Configure > Build Triggers > Poll SCM 에 ‘ * * * * * ‘ 추가
    
    스크립트에 `git poll: true, url: 'https://github.com/subicura/docker-jenkins-workshop.git'` 추가
    

### 실제 사례

- ChatOps나 Slack-message buttons를 사용하여 이미지 생성이 완료되면 배포는 채팅 명령어로 실행
- 브랜치 기반 테스트 배포 전략 : 브랜치별로 이미지 생성하여 배포하여 nginx에서 설정값을 바꿔줌
    <img width="530" alt="8" src="https://github.com/yj-leez/Docker-WIL/assets/77960090/bbcf970c-5a81-4a52-9d40-b40782967a36">

    
- 무중단배포 : 동일한 컨테이너를 두개 띄워서 중단 없이 컨테이너 배포 가능
