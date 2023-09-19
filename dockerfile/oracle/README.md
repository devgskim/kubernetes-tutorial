### ORACLE

* 오라클 환경변수 수정
    - `. oraenv` 명령어를 사용하여 환경변수를 수정할 수 있다.
~~~bash
. oraenv
ORACLE_SID = [orcl] ? prod
ORACLE_HOME = [/opt/oracle] ? /opt/oracle/app/product/11.2.0/dbhome_1
The Oracle base for ORACLE_HOME=/opt/oracle/app/product/11.2.0/dbhome_1 is /opt/oracle/app
[oracle@6host ~]$ echo $ORACLE_SID
prod
[oracle@host ~]$ echo $ORACLE_HOME
/opt/oracle/app/product/11.2.0/dbhome_1
~~~

* 사용자 
~~~sql
CREATE USER user01 IDENTIFIED BY userpassword;
-- 12이상부터 공용계정에는..
CREATE USER C##user01 IDENTIFIED BY userpassword;
-- 아래와 같이 하면 C## 을 붙이지 않아도 됨.
ALTER SESSION SET "_ORACLE_SCRIPT" = false;
-- tablespace 선택
CREATE USER user01 IDENTIFIED BY userpassword DEFAULT TABLESPACE usertablespace;
-- tablespace 선택
ALTER USER user01 DEFAULT TABLESPACE usertablespace
;
SELECT username, user_id, common FROM all_users
;
SELECT username, user_id, common, oracle_maintained FROM dba_users WHERE 1=1
;
ALTER USER user01 ACCOUNT UNLOCK
;
ALTER USER user01 IDENTIFIED BY newpassword
;

-- 권한
GRANT CONNECT, RESOURCE TO user01;
GRANT CREATE TABLE TO user01;
GRANT DBA TO user01;

REVOKE DBA FROM user01;
~~~

* TABLESPACE
~~~sql
CREATE TABLESPACE test_user DATAFILE '/u01/app/oracle/oradata/xe/test01.dbf' 
    SIZE 10m                -- 초기 데이터 파일 크기 설정
    AUTOEXTEND ON NEXT 10M  -- 초기 크기 공간을 모두 사용하는 경우 자동으로 파일의 크기가 커지는 기능
    MAXSIZE 100M            -- 데이터파일이 최대로 커질 수 있는 크기 지정 기본값 = unlimited
    UNIFORM SIZE 1M         -- EXTENT 한개의 크기를 설정
;
-- 테이블스페이스 조회
SELECT tablespace_name, contents, status, logging, shared FROM dba_tablespaces
;
SELECT username, user_id, default_tablespace, common FROM user_users WHERE 1=1
AND username = 'user01'
;
-- 테이블스페이스 위치 조회
SELECT file_id, tablespace_name, file_name, autoextensible, maxbytes/1024/1024 AS "max(MB)", online_status 
  FROM dba_data_files
;
-- Default Temporary Tablespace 변경
SELECT * FROM database_properties WHERE property_name LIKE 'DEFAULT_TEMP%'
;
ALTER USER test_user DEFAULT TABLESPACE {TABLESPACE_NAME}
;
ALTER DATABASE DEFAULT TEMPORARY TABLES=<테이블스페이스명>
;
ALTER TABLESPACE CREATE TABLESPACE test_user DATAFILE '/u01/app/oracle/oradata/xe/test01.dbf' 
    ADD DATAFILE '/u01/app/oracle/oradata/xe/test02.dbf' SIZE 10m
;
ALTER TABLESPACE CREATE TABLESPACE chub DATAFILE '/opt/oracle/oradata/ORCLCDB/chub01.dbf' 
    ADD DATAFILE '/opt/oracle/oradata/ORCLCDB/chub02.dbf' SIZE 10m
;
SELECT tablespace_name, file_name 
  FROM dba_data_files WHERE 1=1
AND tablespace_name = 'test_user'
;
-- contents : 모든 세그먼트를 삭제
-- datafiles : 모든 데이터파일까지 삭제
DROP TABLESPACE test01 INCLUDING CONTENTS AND DATAFILES 
;
~~~

* tables
~~~sql
SELECT owner, table_name, tablespace_name, status, logging, compression FROM all_tables WHERE 1=1
AND tablespace_name IN ('USER')
;
SELECT owner, table_name, table_type FROM all_tab_comments WHERE 1=1
;
~~~

* sqlplus 

~~~sql
SET LINESIZE 1000

-- tablespace
COLUMN tablespace_name FORMAT a15
COLUMN default_tablespace FORMAT a20
COLUMN file_name FORMAT a50
COLUMN file_id FORMAT 999
COLUMN username FORMAT a30
COLUMN DEFAULT_COLLATION FORMAT a30
COLUMN Name FORMAT a55
COLUMN Type FORMAT a30
COLUMN contents FORMAT a20
COLUMN owner FORMAT a20
COLUMN table_name FORMAT a30
COLUMN table_type FORMAT a20
COLUMN open_time FORMAT a30
~~~

* PDB
    - reference: https://fliedcat.tistory.com/128
~~~sql
--CREATE PLUGGABLE DATABASE cms_skylife ADMIN USER chub IDENTIFIED BY chub2023 ROLE = (dba) FILE_NAME_CONVERT = ('소스 PDB 위치','PDB 생성 위치') 
--;
CREATE PLUGGABLE DATABASE orclpdb ADMIN USER pdbadmin IDENTIFIED BY pdbadminpassword FILE_NAME_CONVERT = ('/pdbseed','/PDB1/') 
;
SELECT name, status FROM v$datafile
;
SELECT name, status FROM v$tempfile
;
SELECT name, open_mode, restricted, open_time FROM V$PDBS
;
ALTER PLUGGABLE DATABASE cms_skylife CLOSE
;
-- RAC 인 경우
ALTER PLUGGABLE DATABASE cms_skylife CLOSE instances=all   
;
DROP PLUGGABLE DATABASE pdb_name [ including | keep ] DATAFILES
;
DROP PLUGGABLE DATABASE chub_skylife INCLUDING DATAFILES
;
DROP PLUGGABLE DATABASE cms_skylife INCLUDING DATAFILES
;
ALTER PLUGGABLE DATABASE chub_cms OPEN
;
GRANT connect, resource, create table TO pdbadmin
;
SHOW PDBS
~~~

~~~bash
$ sqlplus / as sysdba

SQL> show pdbs;
SQL> alter pluggable database HRPDB close;                  -- PDB 종료
SQL> drop pluggable database  HRPDB including datafiles;    -- 데이타파일 포함하여 PDB 삭제
SQL> show pdbs;
~~~
