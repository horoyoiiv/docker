

# exec  

* 도커 컨테이너 외부에서 컨테이너 내부의 명령어를 실행할 수 있는 커맨드  


```
docker exec [option] [conatiner name, id] [명령] [parameter]
```

* 컨테이너 내부에서 bash shell을 여는 명령어  
```
docker exec -it 7cd12 /bin/bash
```

* 컨테이너 내부에서 ping을 보내는 명령어  
```
docker exec -it 7cd12 ping 172.17.0.6
```



