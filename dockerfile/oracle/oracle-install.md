### ORACLE

#### ORACLE 12c

* reference: 
    - https://zero-coker-321.tistory.com/2

~~~bash

$ docker search oracle
$ docker pull docker.io/truevoly/oracle-12c
# $ docker run -d -p 8080:8080 -p 1521:1521 --name=oracle12c docker.io/truevoly/oracle-12c
$ docker run -d -p 1521:1521 --name=oracle12c docker.io/truevoly/oracle-12c
$ docker exec -it oracle12c bash    

root@134893939202: # sqlplus sys as sysdba
...
Enter password: oracle

SQL> 
~~~

#### ORACLE 19c

* reference: https://mkc110891.medium.com/oracle-19c-on-ubuntu-18-04-docker-6898cd2916f9

1. oracle docker images 를 clone
2. oracle 19c zip을 다운로드 (오라클 계정 필요)
3. 다운로드후 파일을 git의 19c 위치로 이동
3. git의 dockerfiles 위치로 이동하여 `buildContainerImage.sh` 를 실행
4. run docker images

```bash
# oracle docker images 를 clone
$ git clone https://github.com/oracle/docker-images

# 다운로드
https://www.oracle.com/database/technologies/oracle-database-software-downloads.html#19c

# 파일이동
$ mv LINUX.X64_193000_db_home.zip /{custom path}/docker-images/OracleDatabase/SingleInstance/dockerfiles/

$ cd {custom path}/docker-images/OracleDatabase/SingleInstance/dockerfiles
$ ./buildContainerImage.sh -v 19.3.0 -e
$ docker images
REPOSITORY        TAG         IMAGE ID       CREATED              SIZE
oracle/database   19.3.0-ee   c3aa6a5d7968   About a minute ago   6.68GB
```

* run docker images

```bash
$ docker run --name oracle19c --network host -p 1521:1521 -p 5500:5500 -v /u01/oracle:/opt/oracle oracle/database:19.3.0-ee
or 
$ docker run --name oracle19c -p 1521:1521 -p 5500:5500 -e ORACLE_PDB=orapdb1 -e ORACLE_PWD=topsecretpass -e ORACLE_MEM=3000 -v /opt/oracle/oradata -d oracle/database:19.3.0-ee

## docker start
$ docker start oracle19c
```

* web

```bash
You can access the Oracle Enterprise Manager Database Express on, https://localhost:5500/em/shell.
Username: system
Password: <<Use that you have copied from the previous step>>
Container: orclpdb1
```


#### Docker-compose 기본 명령 

~~~bash
$ docker-compose -f [docker compose 파일] [명령어]
$ docker-compose -f docker-compose.ora19c.yml up
$ docker-compose -f docker-compose.ora19c.yml up -d         # 백그라운드 모드 실행 
$ docker-compose -f docker-compose.ora19c.yml start         # 정지한 컨테이너를 시작
$ docker-compose -f docker-compose.ora19c.yml start ora19c  # 컨테이너가 여러개일경우 ora19c 
$ docker-compose -f docker-compose.ora19c.yml restart       # 이미 실행 중인 컨테이너 다시 시작
$ docker-compose -f docker-compose.ora19c.yml stop          # 컨테이너 중지
$ docker-compose -f docker-compose.ora19c.yml down          # 컨테이너 중지및 삭제
$ docker-compose -f docker-compose.ora19c.yml logs          # 컨테이너로그 확인
$ docker-compose -f docker-compose.ora19c.yml ps            # 컨테이너 목록 

## Docker 접속
$ docker-compose exec [컨테이너] [명령어]
$ docker-compose -f docker-compose.ora19c.yml exec ora19c /bin/bash​
~~~