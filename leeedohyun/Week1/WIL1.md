# 생활코딩
## 설치
도커같은 컨테이너 기술은 리눅스 운영체제의 기술이다. 이를 통해서 우리는 2가지를 알 수 있다.
- 도커 위에서 돌아가는 컨테이너, 컨테이너 안에서 돌아가는 각각의 앱들은 리눅스 운영체제에서 동작하는 앱이라는 것을 알 수 있다.
- 만약 내 컴퓨터의 운영체제가 리눅스가 아니라면 도커를 사용할 수 없을까? => 그렇지 않다. 컴푸터에 가상 머신을 설치해서 그 가상 머신에 리눅스 운영체제를 설치하면 리눅스 운영체제에서 도커와 같은 컨테이너 기술을 사용할 수 있다.

이렇게 하는 것은 굉장히 복잡하다. 하지만, 도커가 알아서 가상 머신을 설치해주고 그 위에 리눅스를 설치해준다. 내 컴퓨터의 운영체제가 리눅스가 아니라도 도커를 사용할 수 있지만 가상 머신을 설치하기 때문에 어느 정도의 속도 저하는 감수해야 한다. 그럼에도 도커를 사용하는 이유는 도커를 사용했을 때의 편리성이 크기 때문이다.

![image](https://github.com/leeedohyun/Docker-WIL/assets/116694226/534d5499-0e39-4cc0-9487-3cf67f1bc4eb)

<img src="https://github.com/leeedohyun/Docker-WIL/assets/116694226/3d7ec35a-875a-4faf-8851-9fb0f61fcd9f" width=500>

설치가 완료된 것을 확인할 수 있다.

## 이미지 pull
도커 허브라고 하는 레지스트리 서비스에서 필요한 소프트웨어를 찾게 된다. 도커 허브에서 찾아낸 소프트웨어를 내 컴퓨터에 다운한 것을 이미지라고 한다. 이미지를 실행하는 것을 컨테이너라고 한다. 이미지는 여러 개의 컨테이너를 가질 수 있다. 도커 허브에서 이미지를 다운받는 행위를 `pull`이라고 한다. 이미지를 실행시키는 행위를 `run`이라고 한다. 지금부터 도커 허브에서 이미지를 다운받는 방법을 알아보자.

다운로드 받은 이미지를 확인하기 위해서 다음 명령어를 사용하면 된다.

```linux
docker images
```

<img src="https://github.com/leeedohyun/Docker-WIL/assets/116694226/cb5d9e95-d6f4-4da1-9f72-13f1f4ad9106" width=500>

![image](https://github.com/leeedohyun/Docker-WIL/assets/116694226/cd75e48a-03cf-4665-a3f5-e36560ae450d)

docker 대시보드를 통해서도 다운 받은 이미지를 확인할 수 있다.

## 컨테이너 run
대시보드를 통해서 실행하면 다음과 같이 로그가 출력되면서 실행이 된다.

![image](https://github.com/leeedohyun/Docker-WIL/assets/116694226/4a45d9a9-4ee4-4ad7-b1bb-6e2ab5c9a3bb)

도커는 컴퓨터의 자원을 쓰고 있기 때문에 자원을 아끼기 위해서 컨테이너를 종료할 필요가 있는데 위의 사진을 보면 오른쪽 상단에 종료 버튼이 있다. 또한, 컨테이너를 더이상 사용할 필요가 없다는 삭제할 수 있다.

이제 명령어를 통해서 컨테이너를 run 시켜보자.

<img src="https://github.com/leeedohyun/Docker-WIL/assets/116694226/39903420-7861-41db-87ac-86a473d29ed3" width=500>

docker ps를 통해서 실행 중인 컨테이너를 확인할 수 있다.

<img src="https://github.com/leeedohyun/Docker-WIL/assets/116694226/650acc66-d2fd-43a4-a058-fac5dcfaf290" width=500>

## 네트워크
```linux
docker run --name ws3 -p 8081:80 httpd
```

위의 명령어를 통해서 컨테이너를 생성할 수 있다.

`localhsot:8080`으로 들어가보면 다음과 같이 나온다.

![image](https://github.com/leeedohyun/Docker-WIL/assets/116694226/11a26505-a9d6-416c-a7e2-f99057e9bcf4)

## 명령어 실행
<img src="https://github.com/leeedohyun/Docker-WIL/assets/116694226/b76a0511-5f33-441b-b4a3-04d5bc98a076" width=500>

그리고 index.html을 바꿔주면 바뀐 index가 출력된다.

![image](https://github.com/leeedohyun/Docker-WIL/assets/116694226/ed71e75b-c2d5-450d-8598-f4976ad25e04)

## 호스트와 컨테이너의 파일 시스템 연결
```linux
docker run -p 8888:80 -v ~/study/htdocs:/usr/local/apache2/htdocs/ httpd
```

위의 명령어를 통해서 index.html을 수정하면 호스트의 파일 시스템을 변경하면 컨테이너 파일 시스템에 적용이 되는 것을 확인할 수 있다.

![image](https://github.com/leeedohyun/Docker-WIL/assets/116694226/72b44ee4-3f82-4324-9c9c-0501537dc0e5)