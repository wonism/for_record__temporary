# 임시

## AWS EC2 인스턴스 세팅하기
- AWS EC2 대쉬보드에서 Launch 로 인스턴스 생성

```sh
# 인스턴스 접속
$ ssh -i ~/.ssh/인증서.pem 계정명@주소

# 패키지 업데이트
$ sudo apt-get update
$ sudo apt-get upgrade
```

## MySQL 설치
__5.5 or 5.6__
```sh
$ sudo apt-get install mysql-server mysql-client
```

__5.7__
```sh
$ wget http://dev.mysql.com/get/mysql-apt-config_0.7.2-1_all.deb
$ dpkg -i mysql-apt-config_0.7.2-1_all.deb

$ sudo apt-get update
$ sudo apt-get install mysql-server
```

```sh
# 비밀번호 설정 뒤, 
# Security Script 실행 및 설정
$ sudo mysql_secure_installation

# 외부에서 MySQL 접속이 가능하도록 설정
$ mysql -u root -p
mysql> use mysql;
mysql> GRANT ALL PRIVILEGES ON *.* to 'root'@'%' IDENTIFIED BY 'password';
mysql> flush privileges;
```

## MySQL my.cnf 설정
```sh
$ sudo vi /etc/mysql/my.cnf
```
```
# bind-address = 127.0.0.1 부분 주석 처리
# bind-address = 127.0.0.1

# 인코딩 설정
[mysqld]
...
character-set-server = utf8
collation-server = utf8_general_ci
init_connect = set collation_connection = utf8_general_ci
init_connect = set names utf8

[mysql]
...
default-character-set = utf8

[mysqld_safe]
...
log-error = /var/log/mysql/error.log
pid-file = /var/run/mysqld/mysqld.pid
default-character-set = utf8

[client]
...
default-character-set = utf8

[mysqldump]
...
default-character-set = utf8

# innoDB설정 (안됌)
[mysqld]
...
innodb_data_home_dir = /var/lib/mysql
innodb_data_file_path = ibdata1:10M:autoextend
innodb_autoextend_increment = 10M
innodb_log_group_home_dir = /var/lib/mysql
innodb_buffer_pool_size = 256M
innodb_additional_mem_pool_size = 20M
innodb_log_file_size = 64M
innodb_log_buffer_size = 8M
innodb_flush_method = O_DIRECT
innodb_flush_log_at_trx_commit = 1
innodb_lock_wait_timeout = 50
innodb_thread_concurrency = 8
``` 

## MySQL 재시작
```sh
$ sudo /etc/init.d/mysql restart
```

## Data Directory 설정
__MySQL 5.7.6 이전 버전을 사용하는 경우__
```sh
$ sudo mysql_install_db

# mysql_install_db 는 5.7.6 에서 deprecated 되었다.
```
__MySQL 5.7.6 이후 버전을 사용하는 경우__
```sh
$ mysqld --initialize
```
