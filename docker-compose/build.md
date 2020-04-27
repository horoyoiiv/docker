
## 도커 컴포즈 build 방법  

* 도커 컴포즈를 구성하는 서비스들의 이미지만을 build한다.  
```
docker-compose build 
```

* 서비스들의 이미지를 build한 후 실행까지 수행한다.  
이미 이미지가 빌드되어 있다면 바로 실행한다.  
```
docker-compose up -d  
```

* 이미지가 이미 생성되어 있더라도, build를 수행한 후 실행한다.  
```
docker-compose up --build 
```

