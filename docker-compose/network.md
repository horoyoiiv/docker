

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

```
$ docker network ls        
NETWORK ID          NAME                         DRIVER              SCOPE
46374da32272        bridge                       bridge              local
bbd8bb5a3d0c        documents_of_edgex_default   bridge              local
```
* docker-compose가 실행된 디렉토리 명 뒤에 default가 붙은 네트워크 생성   
* network 확인 시  
```
docker network ls
```
* 아래와 같이, 컴포즈에 속한 컨테이너들은 같은 서브넷 망 안에 포함된다.  
* 도커 컴포즈는 links가 필요없다.  
```
[
    {
        "Name": "documents_of_edgex_default",
        "Id": "bbd8bb5a3d0c47fadfaf81a5e0707566ad8f360531f53f0659d4622a4331d100",
        "Created": "2020-04-26T03:45:00.913981754+09:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.23.0.0/16",
                    "Gateway": "172.23.0.1"
                }
            ]
        },
       ....
       "Containers": {
            "847525609bec37f59748732cb2abbd2af8088e4cffa245e6f4e4587384fd74ce": {
                "Name": "documents_of_edgex_db_1",
                "EndpointID": "369c6e59c0013bed9d143042a3ab4b9e2e8653bc1a89658b8406a37ff3c7c4b0",
                "MacAddress": "02:42:ac:17:00:02",
                "IPv4Address": "172.23.0.2/16",
                "IPv6Address": ""
            },
            "a6fba22430ba9366fb6c41845294ad489611ba895f6d103aad28e2ef1b1c981b": {
                "Name": "documents_of_edgex_app_1",
                "EndpointID": "f9c6b7364d7ecbb591ca358582b6162bc8094fa4c498baab645d19cff660ea83",
                "MacAddress": "02:42:ac:17:00:03",
                "IPv4Address": "172.23.0.3/16",
                "IPv6Address": ""
            }
        }    
```


