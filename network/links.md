

# Legacy Links  
[Link](https://bluese05.tistory.com/54)  

* 컨테이너 사이의 연동(접속)을 위하여 연결해주기 위하여 links를 사용한다.  


### 1. 각 컨테이너의 Private IP가 아닌, link를 써야 하는 이유  

* Django가 실행되는 컨테이너와 Redis 컨테이너 사이에 통신을 하기 위하여 Redis의 Private IP를 사용할 수 있다.  
```python
// Django 컨테이너에서 동작하는 redis 연결 코드
import redis
r = redis.Redis(host='172.168.0.2', port=6379, db=0)
```

* 하지만 **redis container의 Private IP는 실행될 때마다 바뀐다.**  

### 2. --links 사용 시  

#### 1. 우선 redis 서버의 이름을 my-redis-db로 지정한 후 실행한다.  
```
sudo docker run -d --name my-redis-db redis
```

redis서버는 "172.17.0.3" private ip를 할당받았다.  

#### 2. --link [실행 중인 컨테이너 명]:[내부에서 사용할 별칭]  
```
sudo docker run -d --name my-django --link my-redis-db:redis-server webapp
```

#### 3. web 서버 내부의 /etc/hosts 파일  
```
# cat hosts
127.0.0.1       localhost
::1     localhost ip6-localhost ip6-loopback
...
[172.17.0.3      redis-server a3d375daa755 my-redis-db]
...
```

* link를 사용 시 domain name resolve를 위한 /etc/hosts 파일에 외부 컨테이너 레코드가 저장된다.  
* web app에서는 아래와 같이 redis-server라는 내부에서 사용할 별칭을 사용하면 된다.  

```python
// Django 컨테이너에서 동작하는 redis 연결 코드
import redis
r = redis.Redis(host='redis-server', port=6379, db=0)
```







```python
// Django 컨테이너에서 동작하는 redis 연결 코드
import redis
r = redis.Redis(host='172.168.0.2', port=6379, db=0)
```



### 2. links를 주었을 때의 /etc/hosts 파일  

