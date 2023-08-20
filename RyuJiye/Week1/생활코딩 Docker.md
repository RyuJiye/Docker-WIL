Docker 개념
=============
### 등장 배경  

웹 서버를 만드는 사람이 컴퓨터를 구해서 운영체제를 깔고 그 위에 웹서버를 설치하여 우리에게 줘야함   
  
-> 하나의 컴퓨터에 가상으로 컴퓨터를 만들고 운영체제를 설치하여 웹 서버 운영   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;but 속도 저하, 운영체제를 여러개 깔아야함  
      
-> 하나의 os 위에 각각의 앱을 격리된 상태에서 실행(예: Docker)  
&nbsp;&nbsp;
&nbsp;&nbsp;
## Docker
* 각 컨테이너에는 운영체제가 아닌, 앱에 필요한 라이브러리와 실행 파일이 포함되어 있음
![image](https://github.com/RyuJiye/Docker-WIL/assets/90456695/903f288c-94b4-4246-8270-315eebe9544a)   
* 리눅스 운영체제 기술  
* 컴퓨터의 OS가 리눅스가 아니더라도 Docker가 가상머신을 설치하여 리눅스 환경을 제공  
![image](https://github.com/RyuJiye/Docker-WIL/assets/90456695/301a215a-15c6-4c01-90f8-60fcd305a9ee)
&nbsp;&nbsp;
&nbsp;&nbsp;

## pull, run
#### 컴퓨터
* app store:program: process  
&nbsp;&nbsp;
#### Docker
* docker hub: image: container   
* program이 여러 process를 가질 수 있는 것 처럼 image도 여러 container를 가질 수 있음  
* dockerhub&nbsp;&nbsp;&nbsp;&nbsp;--pull-->&nbsp;&nbsp;&nbsp;&nbsp;image&nbsp;&nbsp;&nbsp;&nbsp;--run-->&nbsp;&nbsp;&nbsp;&nbsp;container  
&nbsp;&nbsp;
&nbsp;&nbsp;

네트워크
--------
* Docker 사용 X  
  웹브라우저와 웹서버를 연결하는 port가 오직 하나  
![image](https://github.com/RyuJiye/Docker-WIL/assets/90456695/f9799840-b463-4778-9fd1-190a3990f3cb)
&nbsp;&nbsp;
* Docker  
  port forwarding: 웹브라우저와 웹서버가 각각 다른 port에 접근  
  웹브러우저는 Host port, 웹서버는 container port에 연결되어있음
![image](https://github.com/RyuJiye/Docker-WIL/assets/90456695/d780298d-b18c-4ac0-9934-bc5c5af41dec)
&nbsp;&nbsp;
&nbsp;&nbsp;

명령어 실행
------------
* index.html 파일을 편집하여 나만의 web을 만드는 것이 중요  
* 호스트와 컨테이너의 파일시스템을 연결하여 파일 편집  
![image](https://github.com/RyuJiye/Docker-WIL/assets/90456695/ff730aaa-864c-48d6-bd57-a5c1159cfbb8)  
&nbsp;&nbsp;
* container를 대상으로 명령 내리기  
&nbsp;&nbsp;
1. docker desktop 사용   
![image](https://github.com/RyuJiye/Docker-WIL/assets/90456695/6cebe643-fd00-4d6e-b132-ca112c8e82d3)  
&nbsp;&nbsp;
2. cmd창 사용  
![image](https://github.com/RyuJiye/Docker-WIL/assets/90456695/e7e429a7-3c3a-4b75-9872-9f5d484977e7)  
![image](https://github.com/RyuJiye/Docker-WIL/assets/90456695/25499977-5a7a-4b0f-87dc-3255942feb81)  
&nbsp;&nbsp;
&nbsp;&nbsp;

실습
------
1. 도커 사용 준비 완료  
![image](https://github.com/RyuJiye/Docker-WIL/assets/90456695/655361ef-2dce-4daa-92b0-835e20f69ff5)  
&nbsp;&nbsp;
3. 이미지 pull, httpd 설치 완료  
![image](https://github.com/RyuJiye/Docker-WIL/assets/90456695/e2c89b30-28fc-43f3-911d-52cd807c8bef)  
&nbsp;&nbsp;
3. 서버 생성  
   이름: ws2, host port:8080, container port:80  
![image](https://github.com/RyuJiye/Docker-WIL/assets/90456695/8e282fe6-9703-4111-b831-802eff7153f7)  
![image](https://github.com/RyuJiye/Docker-WIL/assets/90456695/0a1f0bdf-7f5a-4837-a454-80ee33270e7b)  
![image](https://github.com/RyuJiye/Docker-WIL/assets/90456695/960bc0ae-965f-4adc-9084-afbf13091575)  
&nbsp;&nbsp;

4. 파일 위치 찾기   
   ![image](https://github.com/RyuJiye/Docker-WIL/assets/90456695/1b09f8a1-10d4-45c2-9f89-dbe50200a20a)






