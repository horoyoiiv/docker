

## 1. docker run [이미지명]  
이미지를 컨테이너로서 실행  
1) 이미지가 로컬에 없다면, 자동으로 원격 레포지토리에서 pull한 후 실행  

#### 1.1. docker run ubuntu  
운영체제를 실행하면 바로 Exit된다.  
도커는 특정 task, 프로세스를 실행시키기 위함이다. 그 프로세스가 종료되면 container로 종료된다.  
OS 그 자체는 프로세스를 실행시키기 위한 **base image**이기에 그 자체로 실행되지 않는다.  

#### 1.2. attach 와 detach  
attach : container에서 동작하는 프로세스의 stdout을 현재 콘솔에 출력한다.  
콘솔에서 추가작업을 할 수 없기에 detach모드로 백그라운드에서 실행되게 한다.  

#### 1.2.1. docker run -d [이미지]  
```
// detach option
docker run -d [이미지명]
```

#### 1.2.2. docker attach [아이디]  
detach모드에서 실행되는 컨테이너를 다시 attach한다.  

#### 1.3. -tag  
what is tag?  
태그는 **버젼**이다.  

```
docker pull redis
```
위의 명령어는 redis의 최신버젼을 가져온다.  

```
docker pull redis:4.0
```
위와 같이 : 으로 구분하여 버전을 명시하고, 이는 곧 tag를 의미  

```
REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
redis                     latest              df5748206578        47 hours ago        98.3MB
redis                     4.0                 13c9467679a5        5 days ago          89.3MB
```
두 가지 버전의 redis의 이미지가 생긴다.  

#### 1.4. 컨테이너의 STDIN과 컨테이너의 Terminal  

**docker run -i [이미지]** : 컨테이너의 STDIN을 현재 프롬프트에서 사용가능.  

**docker run -t [이미지]** : 컨테이너의 terminal을 attach할 수 있다.  

```
docker run -it [이미지명]
```

#### 1.5. 환경 변수 세팅  

-e : 컨테이너에서 사용할 환경변수를 세팅할 수 있다.  
```
docker run -e WATER=123 [이미지명]
```






## 2. docker ps  
실행 중인 컨테이너 상태 확인  

## 2.1. docker ps -a  
Exit포함 모든 컨테이너 상태 확인  


## 3. docker stop []  
실행 중인 컨테이너 중지  


## 4. docker rm []  
컨테이너 삭제  

## 5. docker images  
로컬의 모든 이미지 확인  









