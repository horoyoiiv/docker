
# How to make an image for my application  


## 1. 이미지를 만들기 위하여, 해당 어플리케이션(테스크)가 실행될 환경을 생각해라.  

![image](https://user-images.githubusercontent.com/62331555/80022296-cdfe6f00-8516-11ea-940b-edddcf22a87b.png)  

Flask를 통한 웹 어플리케이션을 배포하고자 한다면  

#### 1.1. Dependency를 컨테이너에 설치  
1) OS를 ubuntu로 선택 - base image  
2) 패키지 툴(APT)를 통한 디펜던시인 python 설치  
3) 패키지 툴(pip)을 통한 Python의 dependency를 설치  

#### 1.2. 실행할 코드를 등록 후 실행  
4) COPY  


## 2. Dockerfile 구조     

![image](https://user-images.githubusercontent.com/62331555/80022670-5da41d80-8517-11ea-8b19-18991888538d.png)  

#### 1. FROM [base image]  
1.1. 모든 dockefile은 FROM으로 시작하여, base image를 선택해야 한다.  
1.2. OS를 base image로 하거나 그 OS를 base image로 쌓아올린 다른 image를 선택할 수 있다.  

#### 2. RUN : 이미지 빌드 시 실행할 command  
 * Dockerfile을 정의하는 것은 **실행환경을 설정**하는 것이다.  
 * Dockerfile을 빌드하여 이미지를 만드는 것은 **실행환경을 빌드**하는 것이다.  
 
```
RUN apt-get install python
RUN pip install django
```
#### 2.1. RUN vs CMD  
RUN - RUN instruction allows you to install your application and packages required for it. It executes any commands on top of the current image and creates a new layer by committing the results. Often you will find multiple RUN instructions in a Dockerfile.  

CMD - CMD instruction allows you to set a default command, which will be executed only when you run container without specifying a command. If Docker container runs with a command, the default command will be ignored. If Dockerfile has more than one CMD instruction, all but last 
CMD instructions are ignored.  


#### 3. COPY : local의 특정 파일을 docker image에 복사한다.  

```
COPY . /opt/source-code
```
Dockerfile이 위치한 (.) 곳의 파일들을 image의 /opt/source-code 디렉토리 밑에 복사한다.  

[COPY vs ADD](https://nickjanetakis.com/blog/docker-tip-2-the-difference-between-copy-and-add-in-a-dockerile)  

#### 4. ENTRYPOINT : image가 컨테이너로 실행되었을 때의 실행 지점  


## 3. docker build .  
. : Dockerfile 이 위치한 곳  

#### 3.1. docker build -t sleepy-ubuntu .
* -t 를 통해 해당 이미지의 이름을 설정할 수 있다.  



## 4. layered 구조의 이미지  

* 이미지 자체는 한번 생성되면 Immutable  
* 도커의 이미지는 layered 구조로, **변경된 부분에 대해서만** 새로운 이미지의 layer를 쌓는다.  
* Dockerfile에 정의된 각 Instruction 실행 시 새로운 Layer가 추가된다.  

1) (이를 통해) 이미지 빌드 실패 시, 실패한 레이어부터 다시 빌드할 수 있다.  
2) (이를 통해) 공통된 하위 레이어는 한번만 다운로드받고, 변경된 레이어만 다운받으면 된다.  


```
이런 특징을 잘 활용하면 다운로드 받을 이미지의 크기를 줄일 수 있다. 
예를 들어 10개의 자바 어플리케이션을 이미지로 만들 때 openjdk:8u212-jdk-alpine 이미지를 하위 레이어로 사용하면 
openjdk:8u212-jdk-alpine 이미지와 관련된 파일은 한 번만 다운로드 하고 
10개 자바 어플리케이션의 변경 부분만 다운로드하므로 
어플리케이션을 구동하기 위해 다운로드해야 하는 이미지의 크기가 줄어든다.
(https://javacan.tistory.com/entry/docker-start-6-docker-image-layer)
```

![image](https://user-images.githubusercontent.com/62331555/80026928-e3c36280-851d-11ea-90cd-f837bcfe61c2.png)  


