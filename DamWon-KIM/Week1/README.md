## 생활코딩 Docker 입구 수업


```bash
Docker위에서 돌아가는 contain, 또 그 container안에서 동작하는 app들은 Linux안에서 동작하는 app들이다.
운영체제가 Window나 MacOS라 한다면 컴퓨터에다 가상머신을 깔고 리눅스 운영체제를 깔면 리눅스 운영체제
안에서 위에 docker를 설치해 준다.
```

docker hub -> [pull] -> image -> [run] -> container

### image pull
docker pull httpd
docker images

### container run
docker run httpd
docker run --name ws httpd
docker stop ws
docker start ws
docker ps -a
docker logs ws
docker logs -f ws
docker rm ws
docker rm --force ws
docker rmi httpd


![Docker_네트워크.jpg](Docker_네트워크.jpg)
