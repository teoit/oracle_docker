# oracle_docker
Hãy yêu thương nhau cho nhau qua 1 tu doc

*** intall docker destop ***
--> create folder docker/oracle-data
1. create file env --> env.sample
2. docker-compose up -d

*** docker list *** 
docker container ls ( tim id c332ed83afe4)
winpty docker container ls

*** run docker destop --> terminal ***
winpty docker exec -it c332ed83afe4 bash
- cmd
docker exec -it c332ed83afe4 bash


**clear screen**
cl src

*** version ***
select * from v$version


# 1. Connect database:
sqlplus sys as sysdba 

*** SET SESSION ***
alter session set "_ORACLE_SCRIPT"=true;

***Create user***
create user THANHSANG identified by 123;
SELECT default_tablespace, temporary_tablespace FROM dba_users WHERE username='THANHSANG';
disconnect;

*** Gan quyen dang nhap cho user***
$ GRANT CREATE SESSION TO THANHSANG; (grant connect to THANHSANG;)

*** List User ***
SELECT username FROM dba_users;
*** xEM QUYEN***
SELECT * FROM DBA_SYS_PRIVS WHERE GRANTEE='THANHSANG';

# 3. Cap quyen va gan dung luong cho user, va tao bang moi
connect sys as sysdba;
GRANT ALL PRIVILEGES TO THANHSANG;

ALTER USER THANHSANG QUOTA 100M ON users;                                             
GRANT CREATE TABLE TO THANHSANG;

connect THANHSANG/123;
GRANT CREATE SESSION TO THANHSANG;

disconnect;
connect THANHSANG/123;
CREATE TABLE thanhsangtb (name varchar2(30)) tablespace users;

# 5. Gan dung luong khong gioi han:
connect sys as sysdba                                                                      
ALTER USER THANHSANG QUOTA UNLIMITED ON users;                                           
GRANT UNLIMITED TABLESPACE TO THANHSANG;                                                 
SELECT tablespace_name, username, bytes FROM DBA_TS_QUOTAS;

# 6. Tao user va gan dung luong cung luc:
CREATE USER THANHSANG IDENTIFIED BY 123 DEFAULT TABLESPACE USERS QUOTA 500M ON USERS;
SELECT tablespace_name, username, bytes FROM DBA_TS_QUOTAS;    

# 7. Doi mat khau
alter user THANHSANG identified by 1234;   

# 8. Thiet lap het han mat khau
alter user new_thanhtu identified by 1234 password expire;                                 
connect new_thanhtu/1234                                                                   
disconnect                                                                                 
connect new_thanhtu/123

#9. create table
Create table users(
    id NUMBER PRIMARY KEY,
    username VARCHAR2(50),
    email VARCHAR2(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);


#10 gan quyen table cho user

# tao profile
 CREATE PROFILE "PASS_PROFILE" LIMIT
  PASSWORD_LIFE_TIME 60
  PASSWORD_GRACE_TIME 10
  PASSWORD_REUSE_TIME 1
  PASSWORD_REUSE_MAX 5
  FAILED_LOGIN_ATTEMPTS 3;

***xem profile ***
select profile, resource_name, limit from dba_profiles where profile='pass_profile'

*** unlock tai khoa **
alter user {user} account unlock
