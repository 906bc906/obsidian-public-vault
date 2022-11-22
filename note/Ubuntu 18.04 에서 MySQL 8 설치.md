---
title: Ubuntu 18.04 에서 MySQL 8 설치
date: 2022-10-25T14:55:09+09:00
last_modified_at: 2022-11-22T20:09:20+09:00
---
Ubuntu 18.04 에서 `sudo apt install mysql-server` 를 하면 MySQL 5.7 버전이 설치됨.

MySQL 5.7과 8 은 성능 차이가 꽤 크다. [MySQL Performance Benchmarking: MySQL 5.7 vs MySQL 8.0 | Severalnines](https://severalnines.com/blog/mysql-performance-benchmarking-mysql-57-vs-mysql-80/)

이전 프로젝트를 유지보수하는게 아니라 새 프로젝트를 MySQL로 만든다면 8.0 이상을 쓰는 것이 좋아보임.

## Repository 등록

최신 MySQL 을 사용하기 위해 MySQL Setup Packages 를 설치해야 한다.

https://dev.mysql.com/downloads/

![](attachments/Pasted%20image%2020221025133950.png)

위 주소에서 apt repository를 선택하고 다운로드 클릭.

![](attachments/Pasted%20image%2020221025134020.png)

No thanks, 에 마우스 커서를 대보면 링크를 확인할 수 있다. 링크 주소 복사해서 서버에 다운로드 하자.

```
wget -c https://dev.mysql.com/get/mysql-apt-config_0.8.24-1_all.deb
```

현재 디렉토리에 mysql~.deb 가 다운로드 되었으면 이제 리포지토리를 설치하면 된다.

```
sudo dpkg -i mysql-apt-config_0.8.24-1_all.deb
```

## 업데이트 및 업그레이드

```bash
sudo apt-get update
sudo apt-get upgrade
```

## MySQL 설치

```bash
sudo apt-get install mysql-server
```

## MySQL 서비스 실행

```bash
sudo systemctl start mysql.service
```

## root 비밀번호 설정

이하의 내용은 [How To Install MySQL on Ubuntu 20.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04) 를 참고하였음.

보통 `mysql_secure_installation` 스크립트로 초기 설정하는 것을 권장한다. 하지만 막상 실행해보면 오류가 발새앟는데 우분투 설치의 기본 값은 root 계정이 비밀번호를 사용하지 않도록 설정되어있기 때문임.

그렇기 때문에 MySQL root 계정의 비밀번호를 먼저 세팅해주고 `mysql_secure_installation` 을 실행해야 한다.

```bash
sudo mysql
```

mysql 실행 후

```mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '<사용할 비밀번호>';
```

비밀번호를 설정한다. 이 방법은 root의 인증 방법을 비밀번호로 바꿈. 

```mysql
exit
```

설정했으면 나온다.

## `mysql_secure_installation`

```bash
sudo mysql_secure_installation
```

초기 보안 설정용 스크립트를 실행하고 설정한다.

## 버전 확인

```bash
mysql -u root -p
```

root 계정으로 접속 후 아까 입력했던 비밀번호 입력

```mysql
select @@version;
```

```
+-----------+
| @@version |
+-----------+
| 8.0.31    |
+-----------+
1 row in set (0.00 sec)
```

## 원격 계정 추가

세팅하기 나름이겠지만 `mysql-secure-installation` 에서 root 원격 접속을 차단했다. root 가 아닌 다른 계정을 만들고 원격 접속이 가능하게끔 설정하고 사용하려 한다.

```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

텍스트 에디터는 편한걸 쓰면 된다. nano 대신 vi 써도 되고..

```
bind-address            = 0.0.0.0
```

`bind-address` 를 `0.0.0.0`[^0000] 로 변경한다. 

내 경우엔 `bind-address` 가 없어서 해당 줄을 새로 작성했다.

[^0000]: 모든 ipv4 주소를 지정. 연결된 모든 NIC 로 접속을 허용함. [0.0.0.0 - 위키피디아](https://en.wikipedia.org/wiki/0.0.0.0)

```bash
mysql -u root -p
```

```mysql
create user '<아이>'@'%' identified by '<비밀번호>';
```

호스트가 `%` 임에 주의하면 된다. 아니면 허용할 별도의 ip를 지정해도 된다.

이렇게 해서 계정을 생성..하려고 했더니 

```
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
```

패스워드 정책 문제가 발생한다.

```mysql
mysql> show variables like 'validate_password%';
+--------------------------------------+--------+
| Variable_name                        | Value  |
+--------------------------------------+--------+
| validate_password.check_user_name    | ON     |
| validate_password.dictionary_file    |        |
| validate_password.length             | 8      |
| validate_password.mixed_case_count   | 1      |
| validate_password.number_count       | 1      |
| validate_password.policy             | MEDIUM |
| validate_password.special_char_count | 1      |
+--------------------------------------+--------+
```

길이 최소 8, 대소문자 섞어서, 숫자 포함하여, 특수문자도 포함하는 정책이었다. 정책에 맞춰서 비밀번호를 생성하거나, 정책을 낮추는 방법[^pw-policy]도 가능.

[^pw-policy]: [MySQL - password 정책 낮춰서 단순한 패스워드 사용하기. ERROR 1819 (HY000): Your password does not satisfy the current policy requirements.](https://junho85.pe.kr/1484)

## 계정에 권한 부여

```mysql
grant all privileges on <데이터베이스>.* to '<계정명>'@'<호스트명>';
```

예를 들어  `user` 계정 + `%` 호스트에 `tempdb` 데이터베이스 권한을 부여하고 싶다면,

```mysql
grant all privileges on tempdb.* to 'user'@'%';
```

와 같이 할 수 있다.

모든 DB에 접근 권한을 허용하고 싶다면 데이터베이스 이름 대신 `*` 를 적는다.

## 원격 접속

우분투에서 별도의 방화벽을 설치한 경우 3306 포트를 허용한다.

클라우드 호스팅중이라면 인스턴스 포트 설정에서 3306 포트를 허용한다.

이후 원격 접속을 테스트하면 정상적으로 잘 되는 것을 확인할 수 있음.