

[network](https://bluese05.tistory.com/15)   

#### 1. 컨테이너의 IP  

1. 컨테이너는 기본적으로 하나의 Private IP를 할당받는다.  

```
Reserved for Private IP
192.168.0.0 - 192.168.255.255 (65,536 IP addresses)
172.16.0.0 - 172.31.255.255 (1,048,576 IP addresses)
10.0.0.0 - 10.255.255.255 (16,777,216 IP addresses)
```

#### 1.1. docker inspect [container id]  

해당 컨테이너의 정보 조회 (네트워크 ip 등)  


#### 2. docker run -p 80:5000 [이미지]  

-p 도커 호스트의 포트 : 컨테이너에서 실행되는 프로세스의 포트  

![image](https://user-images.githubusercontent.com/62331555/80016835-a6a3a400-850e-11ea-98e5-513c0fadc2f9.png)  

아래와 같이 호스트 포트를 3개의 컨테이너에 포워딩 가능  

```
docker run -p 80:5000 webapp
docker run -p 81:5000 webapp
docker run -p 82:5000 webapp
```




