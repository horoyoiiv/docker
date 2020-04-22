#### 가상화를 제공하기 위한 조건 1 : 독립된 실행환경 보장  

프로세스 레벨에서 독립된 실행환경을 만들어보자.  

어떻게?  
linux의 서브시스템을 활용해서...  
1) chroots  
2) cgroups  
3) namespaces  

[chroots, cgroups, namespace](https://itnext.io/chroot-cgroups-and-namespaces-an-overview-37124d995e3d)  


## 1. int chroots(const char* path)  

 1. root directory를 변경한다.  
 2. 변경된 root directory는 자식 프로세스에 상속된다.  
 
```c
#include<unistd.h>

    if (chroot("/home/mydir") != 0){
        perror("chroot");
        exit(0);
    }
    
    if (execl("/bin/bash","bash", NULL) == -1) {
        perror("error");
    }
```

[chroots](https://www.joinc.co.kr/w/man/2/chroot)  

## 2. cgroups  

 * Control group  
1. 프로세스를 그룹지어, 그곳에 할당되는 리소스(메모리, CPU, network io 등)를 제한할 수 있다.  
2. 그룹화된 프로세스/어플리케이션에 대한 리소스를 할당 및 제한, 고립을 시킬 수 있는 것인 cgroups  


윈도우와 달리, 리눅스는 장치들을 **위치, 경로**로 식별한다. 사용할 장치와 리눅스의 위치를 연결시키는 것이 **마운트**.  

[리눅스에서 마운트란?](http://itnovice1.blogspot.com/2019/01/linux-mount.html)  


### 2.1. 사용법  

1. cgroups 자체는 파일 시스템이다.  



## 3. namespace  

* 





