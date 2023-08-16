## 컨테이너 기반 가상화 플랫폼‘도커(Docker)'의 이해 1강

도커 이미지 링크

https://bit.ly/docker-sk

### 도커의 등장

- 도커는 `Go` 언어로 이루어져 있다
- 2013년에 처음 등장했다
- `docker run -it [프로세스] [Command`] 명령어를 활용하면 원하는 프로세스에서 Command를 실행할 수 있다
- 아래 세 명령어는 모두 다른 환경에서 `Hello, world`를 출력하는 명령어이다
    
    ```powershell
    docker run -it busybox:latest echo "Hello, world" # busybox 환경에서 실행
    docker run -it ubuntu:latest echo "Hello, world" # ubuntu 환경에서 실행
    docker run -it centos:latest echo "Hello, world" # centos 환경에서 실행
    ```
    

### 컨테이너
<img width="385" alt="image" src="https://github.com/kongnayeon/Docker-WIL/assets/103591752/5f081e5a-8ab2-4d4f-a745-079261071765">

- 컨테이너 ≠ VM
    - VM은 하드웨어 가상화(소프트웨어로 구현된 하드웨어)
    - 컨테이너는 하드웨어 가상화가 아님
    - 컨테이너는 OS에서 지원하는 기능을 사용
    - 컨테이너는 격리된 환경에서 프로세스를 실행 → 가상화된 하드웨어가 필요하지 않다

→ **하드웨어 가상화가 없는 격리된 환경에서 실행되는 프로세스**

- chroot: 루트 디렉토리를 바꿔 주는 역할을 하는 도구
    - 도커가 나오기 전에 활용되었던 기술
    - `root/box` 위치에서 `chroot`를 실행하면 `box`가 루트 디렉토리가 된다

### 이미지

- 특정 프로세스를 실행하기 위한 환경
    - 계층화된 파일 시스템
    - 이미지는 파일들의 집합
    - 프로세스가 실행되는 환경도 결국 파일들의 집합

### 아키텍처

- 도커가 작동하는 기본 아키텍처
    - 리눅스 머신 - 컨테이너를 네이티브하게 지원
        
        <img width="651" alt="image" src="https://github.com/kongnayeon/Docker-WIL/assets/103591752/cdb7b03c-f42a-4b87-876a-66fcf76f46c3">

    - Docker for macOs - xhyve라는 가상화 방식(경량) 위에서 작동
        
        <img width="651" alt="image" src="https://github.com/kongnayeon/Docker-WIL/assets/103591752/2093ff47-0e9c-4b9e-8c12-41058218b499">

    - Virtual Machine on macOs(일반적인 가상 머신)
        
        <img width="648" alt="image" src="https://github.com/kongnayeon/Docker-WIL/assets/103591752/6a5d72c4-d884-4fb6-80c3-8567e3e736dd">


### 컨테이너 가상화가 필요한 이유

- 언제 어디서나 동일하게
- 보편적이지 않은 컴퓨터의 환경
- 특수한 환경에서도 동일하게 돌아갈 수 있도록
- Dockerfile: 깨끗한 환경으로부터 애플리케이션 실행 환경까지 최단경로
- 이미지 = 작동되는 상태
- 이미지로 만들면 공유 가능 → 재현성
