

## persistence  

도커의 컨테이너는 그 상태(데이터)를, 컨테이너가 삭제되면 같이 없어진다.  
이 데이터를 유지하기 위하여 컨테이너 외부의 도커 호스트의 디렉토리와 매핑시켜 그곳에 저장시킨다.  
(혹은 socket으로 내보내거나)  

#### 1. docker run -v /opt/datadir:/var/lib/mysql mysql  

**-v 도커 호스트의 디렉토리:컨테이너의 디렉토리**  
호스트의 디렉토리를 마운트시킴으로써, 컨테이너에 저장되는 데이터를 호스트에서도 저장시킬 수 있다.  


![image](https://user-images.githubusercontent.com/62331555/80017513-a9eb5f80-850f-11ea-85d3-0a3c48dd5a87.png)  



