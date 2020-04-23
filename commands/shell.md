


## shell script를 사용해서, 여러 명령어를 도커에서 실행하자.  

[How to](https://siner308.github.io/2019/02/25/django-docker-custom-image/)  

#### 1. 기존의 Django 서버 이미지를 위한 Dockerfile  

```
FROM python:3.4

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        postgresql-client \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /usr/src/app
COPY requirements.txt ./
RUN pip install -r requirements.txt
COPY . .

EXPOSE 8000
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```

#### 2. ENTRYPOINT에 Shell script를 실행시도록 개선  

```
FROM python:3.6

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
       postgresql-client \
    && rm -rf /var/lib/apt/lists/*

COPY . /app  
RUN pip install -r /app/requirements.txt  
RUN chmod 755 /app/start  
WORKDIR /app
EXPOSE 8000 

ENTRYPOINT ["/app/start"]  # start = shell script file
```

* start 파일을 다음과 같이 만들면 된다.  
```
#!/bin/bash

python manage.py makemigrations
python manage.py migrate

python manage.py runserver 0.0.0.0:8000
```
