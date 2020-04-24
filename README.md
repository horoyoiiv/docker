# docker
[1. All-in-one YOUTUBE](https://www.youtube.com/watch?v=fqMOX6JJhGo)  


프로세스 레벨의 가상화 기술  

## 1. docker의 기반이 된 Linux systems  
1. croots  
2. cgroups  
3. namespace  

[cgroups, namespace](/linux-subsystem.md)  


## 2. docker 기본 커맨드  
[2.1.basic commands](/command.md)  
[2.2. CMD vs ENTRYPOINT](/commands/cmd.md)  

2.3. ENTRYPOINT 는 하나의 명령어만 실행가능한데, 여러 명령어는 어떻게 실행하는가?  
[shell scrippt를 ENTRYPOINT에서 실행해라!](/commands/shell.mc)  

[2.4. exec](/commands/exec.md) : 컨테이너 외부에서 컨테이너 내부의 명령어를 실행하고 싶다면?   
- /bin/bash  





## 3. docker network  

[1. container's network setting](/network.md)  
[2. --link](/network/links.md) : 컨테이너 간 통신을 위하여 Dynamic IP가 거슬린다면...?  


## 4. docker persistence  

#### 1. 레이어드 아키텍쳐  
[1.1. COMMIT ](/layered-architecture.md)  


[-v volume](/persistence.md)  


# Image  

[1. Dockerfile 작성과 Instructions](/image/how-to.md)  

# Docker compose  
[1. docker compose란?](/docker-compose/what.md) : 여러 컨테이너를 하나의 config파일로 관리.  




# Examples  

#### 1. MySQL with Docker  

[1. -v 가 중요하다](/example/mysql.md)  







