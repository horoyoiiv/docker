

# layered-architecture  

## 1. 도커는 레이어 구조이다.  
#### 1.1. Dockerfile부터 시작되는 레이어 구조  

```
FROM ubuntu

RUN apt-get update                # layer + 1
RUN apt-get install python        # layer + 2
 
RUN pip install django            # layer + 3

COPY . /opt/source-code           # layer + 4

ENTRYPOINT  flask run             # layer + 5
```
![image](https://user-images.githubusercontent.com/62331555/80084144-718c6580-8591-11ea-845a-6ea320a28921.png)  

1. base image로부터 변경되는 부분만 하나의 레이어로 쌓여진다.  
2. Dockerfile을 빌드하는 과정에서 layer를 쌓이게 되고, 그 결과 **READ-ONLY**의 이미지가 생성된다.  



## 2. 이미지를 실행하여 컨테이너가 생성되면 WRITABLE 레이어가 생성된다.  

![image](https://user-images.githubusercontent.com/62331555/80082484-6afcee80-858f-11ea-98ff-e825a9bc83c9.png)  


1. 이 레이어는 컨테이너 실행 중 데이터나 로그가 저장되지만, 컨테이너 종료 시 삭제된다.  
2. 컨테이너에서 변경된 레이어의 상태(새 폴더, 파일 생성 변경 등)를 이미지로 저장하고 싶다면 commit  


## 3. docker commit [컨테이너id] [새로만들 이미지명]  

#### 1. docker stop을 통해 컨테이너는 종료된다.  
#### 2. docker ps -a 를 통해 종료된 컨테이너를 확인할 수 있다.  
#### 3. stop은 종료된 컨테이너를 이미지로 만들 수 있도록 기회를 주는 것  

```
docker commit [종료된 컨테이너 id] [새로 만들 이미지명]
```

#### 4. commit을 통해, 원래 image 위에 '컨테이너 실행 중 생성된 레이어'가 추가된 이미지를 생성한다.  


