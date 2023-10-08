GitHub Action 
============
action이란?    
빌드, 테스트, 배포 파이프라인을 자동화할 수 있는 CI(Continuous Integration, 지속 통합)와 CD(Continuous Deployment, 지속 배포) 플랫폼

## Core concepts

* Workflow  
-build, test, package 등등 원하는 작업을 GitHub project에 대해 수행하는 자동화된 process    
-이를 위한 configuration을 정의하는 YAML 파일이 workflow file    
-repository의 root에서 .github/workflows에 위치하고 기본 파일명은 main.yml임     
* Event  
-workflow가 실행되도록 만드는 trigger       
-예) repository에 push, pull request        
-https://help.github.com/en/actions/automating-your-workflow-with-github-actions/events-that-trigger-workflows
 
* Runner  
-GitHub Actions runner 애플리케이션이 설치된 machine          
-host는 GitHub이 될 수도 있고 user가 될 수도 있음        
-즉, Github-hosted runner와 Self-hosted runner로 나뉨       
-runner는 event가 발생하면 해당 workflow를 수행하고 관련된 정보를 GitHub으로 보내줌          

* Job    
-독립된 환경에서 돌아가는 하나의 처리 단위     
-하나의 Workflow에 여러개의 Job을 정의할 수 있음      
-job은 별도의 설정이 없다면 병렬적으로 수행되고 dependency를 설정해주면 순차적으로 수행        
-여러 step이 모인 steps 단위로 정의되는 task      

* Step      
 -task를 정의하는 가장 작은 단위      
 -commands나 actions를 같은 runner에서 실행하도록 작동됨     
 
 * Action      
-steps로 결합한 task들    
-공유될 수 있는 재사용 가능한 단위           
-특정 task들을 action으로 정의하면 다른 workflow에서 use라는 keyword를 통해 해당 task들을 그대로 step 중 하나로 사용할 수 있음      


* Custom Action        
-특정 동작을 수행하기 위한 로직들을 커스텀 액션으로 개발 가능    
-Workflow 파일엔 커스텀 액션을 호출하여 사용    
-여러 레포지토리에 공유하여 사용   
-로직을 중복하여 구현하지 않아도 됨   
-커스텀 액션을 사용하지 않으면 Workflow 파일에 세부적인 구현 코드들이 위치해있어야 함   
![image](https://github.com/RyuJiye/Docker-WIL/assets/90456695/c6e717b7-cd2f-437e-b2b1-2fe19a88d404)   
![image](https://github.com/RyuJiye/Docker-WIL/assets/90456695/3c1a06a7-dd32-4eed-a1fc-a3e737e66420)   


## Jenkins VS CitHub Actions
* Jenkins
-서버 설치 필요      
-동기(제품을 배포하는데 더 많은 시간 소요)                              
-계정과 트리거에 기반, 깃허브 이벤트를 처리할 수 없음      
-환경 호환성을 위해 Docker 이미지에서 동작해야 함   
-캐싱 기법을 위해 플러그인 제공   
   
* Github Actions     
-클라우드에서 동작       
-비동기 CI/CD 가능         
-Github event에 대해 Github Actions 제공          
-모든 환경에 호환됨          
-캐싱 매커니즘을 직접 작성해야함          
  
