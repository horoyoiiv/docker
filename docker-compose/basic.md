

## docker-compse 기본구조  


```
version: '3'

services:
  web:
    build:
      context: .
      dockerfile: Dockerfile                     
    container-name: edgex-dashboard
    command: python manage.py runserver 0.0.0.0:8000
    ports:
      - "8090:8000"
    networks:
      dash-network:
        aliases:
          - web-server  
    depends_on:
      - redis

  redis:
    image: "redis"
    container-name: edgex-redis
    expose:
      - "6379"
    networks:
      dash-network:
        aliases:
          - redis
      
networks:
  dash-network:
    driver: "bridge"   
```


#### 1. build  
 * **context** : dockerfile이 위치한 경로를 지정  
 * **dockerfile** : 그 경로의 dockerfile을 빌드하여 이미지 생성  
```
    build:
      context: .
      dockerfile: Dockerfile                     
```

#### 2. container-name  
```
    container-name: edgex-dashboard
```
 * 이를 지정하지 않는다면, 디폴트 컨테이너 명이 생성된다.  

```
$ docker-compose ps 
디렉토리명_서비스이름
    networks:
      dash-network:
        aliases:
          - web-server  _[숫자]  // 이러한 디폴트 명으로 컨테이너가 생성
```

#### 3. port vs expose  
[stackoverflow](https://stackoverflow.com/questions/40801772/what-is-the-difference-between-docker-compose-ports-vs-expose)  

```
    ports:
      - "8090:8000"
```

```
    expose:
      - "6379"
```

* **export** : 호스트 포트와 매핑하며, expose한다.  
* **expose** : 호스트 포트를 사용하지 않으며, 내부 네트워크에서 접근가능한 포트를 expose한다.  


#### 4. networks  

```
    networks:
      dash-network:
        aliases:
          - web-server  
```
* 해당 서비스가 포함될 네트워크를 정의한다.  
* **aliasese**로 참여 시, 서비스 명이나 alias를 일종의 도메인 네임으로하여, 내부 네트워크에서 접근 가능.  


#### 5. outer network  
  * 해당 서비스들이 사용할 네트워크를 설정한다.  
  
```
networks:
  dash-network:  # 사용할 네트워크 명
    driver: "bridge"  # 네트워크 모드
```


