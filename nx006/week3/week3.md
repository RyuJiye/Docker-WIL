# 도커, 컨테이너 빌드업!

초반 내용은 사실 이전 강의와 거의 유사해서, 이는 생략합니다.

대신 제가 중요하게 생각하는 부분들만 정리하겠습니다.

## 자주 쓰이는 이미지들
### nginx
```bash
$ docker pull nginx:latest
# Nginx 컨테이너를 실행, 하나의 NginX 구동 서버가 된다.
$ docker run --name webserver1 -d -p 8001:80 nginx:latest
```

- `--name`: 컨테이너 이름을 지정합니다
- `-d`: 컨테이너를 백그라운드에서 실행합니다(데몬), 컨테이너 ID만 출력합니다.
- `-p`: 컨테이너 80 포트를 로컬 호스트 8001 포트로 연결

```bash
$ sudo netstat -nlp | grep 8001
$ curl localhost:8001
# 컨테이너의 실행중인 프로세스 표시
$ docker top webserver1
```

### Python
```bash
$ docker run -it -d --name=python_test -p 8900:8900 python
$ docker cp py_lotto.py python_test:/
$ docker exec -it python_test python /py_lotto.py
```