

# MySQL with docker  

[steps](http://jmlim.github.io/docker/2019/07/30/docker-mysql-setup/)  


### 1. PULL MySQL docker 이미지 파일  

```
docker pull mysql:8.0.17
```

### 2. (우선) 도커 호스트의 디렉토리를 마운트하지 않고 실행  

```
docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=password --name ym-mysql mysql:8.0.17
```

* 환경변수에 mysql root 패스워드를 설정한다.  

### 3. mysql이 실행되는 컨테이너에 접속(쉘 실행)  

```
docker exec -it [컨테이너 id] /bin/bash
```

 * 컨테이너 내에서 mysql 접속  
```
$ mysql -u root -p
```

### 4. mysql의 데이터는 /var/lib/mysql 에서 관리  

```
root@4dccb4418f36:/var/lib/mysql# ls

#innodb_temp   binlog.000002  binlog.index  client-cert.pem  ib_logfile0  ibtmp1     performance_schema  server-cert.pem  undo_001
auto.cnf       binlog.000003  ca-key.pem    client-key.pem   ib_logfile1  mysql      private_key.pem     server-key.pem   undo_002
binlog.000001  binlog.000004  ca.pem        ib_buffer_pool   ibdata1      mysql.ibd  public_key.pem      sys              user
```

* create database user;  
database 생성 시 /var/lib/mysql/user 디렉토리 생성  

* create table student ~ ;  
table 생성 시 해당 디렉토리 밑에 .ibd 파일 생성  

```
root@4dccb4418f36:/var/lib/mysql/user# ls

student.ibd
```

### 5. 호스트 볼륨을 도커의 컨테이너로 마운트하기  

 * /var/lib/docker/volumes/ 아래에 디렉토리를 생성할 수 있다.  
 
![image](https://user-images.githubusercontent.com/62331555/80089922-aa303d00-8599-11ea-95dc-02a07c82ad21.png)  


#### 5.1. -v 옵션  
  
  -v 호스트 패스 : 컨테이너 패스  
```
sudo docker run -d -e MYSQL_ROOT_PASSWORD=password --name ym-mysql -v ym-volume:/var/lib/mysql mysql:8.0.17
```


#### 5.2. 컨테이너에서 데이터 저장  

```
docker exec -it [mysql id] /bin/bash
```


#### 5.3. insert 후 도커 호스트의 volumes 디렉토리에도 그 결과가 반영된다.  

```
ubuntu@ip-172-31-39-205:/var/lib/docker$ sudo ls volumes/ym-volume/_data
'#innodb_temp'   binlog.000002   ca.pem            ib_buffer_pool   ibdata1   mysql.ibd            public_key.pem    sys        user
 auto.cnf        binlog.index    client-cert.pem   ib_logfile0      ibtmp1    performance_schema   server-cert.pem   undo_001
 binlog.000001   ca-key.pem      client-key.pem    ib_logfile1      mysql     private_key.pem      server-key.pem    undo_002

ubuntu@ip-172-31-39-205:/var/lib/docker$ sudo ls volumes/ym-volume/_data/user
student.ibd

```


#### 5.4. 기존의 컨테이너 종료 후 새로운 컨테이너 실행 시 -v 옵션만 잘 쓰면 된다...!   

**도커 호스트 볼륨을 그대로 사용하여 데이터를 유지할 수 있게 된다...!**  
```
sudo docker run -d -e MYSQL_ROOT_PASSWORD=password --name ym-mysql -v ym-volume:/var/lib/mysql mysql:8.0.17
```



