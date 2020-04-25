

## docker-compse  
* 여러 컨테이너의 실행 옵션(설정) 및 서로 간의 의존성을 하나의 파일로 관리하는 기술  
* yml 파일로 관리된다.  


```
version: "3"

services:
  app:
    restart: always
    build: . 
    command: "python3 manage.py runserver 0.0.0.0:8000"
    volumes:
      - .:/code
    ports:
      - "8000:8000"
    depends_on:
      - db


  db:
    image: mysql:latest
    command: mysqld --default-authentication-plugin=mysql_native_password
    volumes:
      - "./mysql:/var/lib/mysql"
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=12345
      - MYSQL_DATABASE=django_app
      - MYSQL_USER=django_app
      - MYSQL_PASSWORD=12345
```

### 1. 분석 1  

```
services:
  app:
    restart: always
    build: . 
    command: "python3 manage.py runserver 0.0.0.0:8000"
    volumes:
      - .:/code
    ports:
      - "8000:8000"
    depends_on:
      - db
```

### 1. servcices  
```
services: 
```
* 도커 컴포즈에서는 하나의 컨테이너를 하나의 **서비스**로 지칭한다.  


### 2. app:
```
services:
  app:
```
* 해당 Django 서버의 이름을 app으로 지정  

### 3. build . 
```
services:
  app:
    restart: always
    build: . 
```

* docker-compose.yml 파일이 위치한 곳에서 Dockerfile을 찾은 후 이를 빌드한다.   
* 그렇게 생성된 이미지를 사용하여 컨테이너로 실행...!  


### 4. command  
```
services:
  app:
    restart: always
    build: . 
    command: "python3 manage.py runserver 0.0.0.0:8000"
```
* 컨테이너 실행 시, 수행할 커맨드를 작성한다.  
* command는 shell script를 작성하지 않는 이상, 하나이고 이것이 종료되면 컨테이너도 종료된다.  


### 5. volumes  
```
services:
  app:
    restart: always
    build: . 
    command: "python3 manage.py runserver 0.0.0.0:8000"
    volumes:
      - .:/code
```
* [로컬 경로]:[컨테이너 경로] - 컨테이너의 경로에 로컬 경로를 마운트시켜버린다.  
* dockerfile과 달리, 상대경로를 사용할 수 있다.  


### 6. ports  
```
services:
  app:
    restart: always
    build: . 
    command: "python3 manage.py runserver 0.0.0.0:8000"
    volumes:
      - .:/code
    ports:
      - "8000:8000"
```
* [호스트 포트]:[컨테이너 포트] - 호스트 포트를 컨테이너 포트로 포워딩시킨다.  


### 7. depends_on  
```
services:
  app:
    restart: always
    build: . 
    command: "python3 manage.py runserver 0.0.0.0:8000"
    volumes:
      - .:/code
    ports:
      - "8000:8000"
    depends_on:
      - db
```
* 실행 순서에 대한 제어  
* db 컨테이너가 먼저 실행된 후 해당 컨테이너를 실행하는 옵션  


### 8. environment  
```
  db:
    image: mysql:latest   // 해당 이미지를 베이스로 사용하여 컨테이너 생성  
    command: mysqld --default-authentication-plugin=mysql_native_password
    volumes:
      - "./mysql:/var/lib/mysql"
    restart: always
    environment:                            // 환경변수 세팅
      - MYSQL_ROOT_PASSWORD=12345
      - MYSQL_DATABASE=django_app
      - MYSQL_USER=django_app
      - MYSQL_PASSWORD=12345
```
