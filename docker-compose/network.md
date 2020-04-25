

## 도커 컴포즈의 네트워크 세팅  

[docker network 명령어](https://blog.d0ngd0nge.xyz/docker-network/)  
[docker doc 설명](https://docs.docker.com/compose/networking/#links)  

## 1. 도커 컴포즈 실행 시 네트워크 세팅 환경  

* 도커 컴포즈 실행 시 해당 컨테이너들은 default로 생성되는 네트워크에 참여하게 된다.  
* 또한 yml파일의 services에서 **정의된 컨테이너의 이름을 호스트명으로 하여, 네트워크에 참여**한다.  

## 2. 예제 !!  

```
version: "3"

services:
  app:
    restart: always
    build: . 
    command: ["python3", "manage.py", "runserver", "0.0.0.0:8000"]
    volumes:
      - .:/code
    ports:
      - "8093:8000"
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

#### 1. app 컨테이너에서는 db라는 호스트명으로 db 컨테이너에 접근가능하다.  

#### 2. docker-compse up -d 실행 시  


