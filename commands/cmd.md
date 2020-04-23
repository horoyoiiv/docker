

#### 1. CMD  


* docker는 OS를 실행하고자 설계된 것이 아니다. 그렇기에 아래와 같은 컨테이너는 바로 종료된다.  
```
docker run ubuntu
```

* docker의 컨테이너는 그 안의 프로세스가 살아있는 동안만 지속된다.  

![image](https://user-images.githubusercontent.com/62331555/80074721-9201f300-8584-11ea-9832-f6f40ef56470.png)  

* container가 실행되었을 때, 실행할 명령어를 CMD로 등록한다.  


![image](https://user-images.githubusercontent.com/62331555/80074907-d68d8e80-8584-11ea-9d51-09947e4322cd.png)  


#### 2. ENTRYPOINT  

* CMD와 ENTRYPOINT는 둘 다 컨테이너의 실행시점에 실행할 커맨드를 정의한다.  
```
CMD ["echo", "HELLO"]
```

```
ENTRYPOINT ["echo"]
```

* 차이점은 **docker run 커맨드라인 뒤에 오는 파라미터**가...   
1. CMD 명령어는 씹힌다.(override)   
2. ENTRYPOINT 명령어는 그 명령어 뒤에 파라미터가 append된다.  


#### 1. CMD이 예제  

```
FROM ubuntu
CMD ["echo", "hello world"]
```

```
docker run ubuntu-iamge ls
```

* CMD 명령어는 무시되고, 파라미터인 ls가 실행된다.  


#### 2. ENTRYPOINT 예제  
```
FROM ubuntu
ENTRYPOINT ["echo"]
```

```
docker run ubuntu-iamge ls
```

* echo ls 가 실행된다. 즉, 출력인 "ls"라는 문자열이 나옴.  

#### 3. ENTRYPOINT 와 CMD 의 조합  

* run 커맨드라인에서 유저의 입력을 받을 수도 있고, 안받을 수도 있다.  
안받았을 때의 디폴트 값을 CMD에 지정한다.  

```
FROM ubuntu

ENTRYPOINT ["echo"]
CMD ["default value"]
```
* 커맨드라인의 파라미터가 없다면, CMD의 값이 ENTRYPOINT 커맨드 뒤에 append 된다.  





























