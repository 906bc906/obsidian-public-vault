---
title: Reids 클라우드를 위한 여러 인스턴스 설정하기
date: 2022-10-25T20:21:55+09:00
last_modified_at: 2022-10-26T01:11:29+09:00
---
MySQL을 한 클라우드 인스턴스에서 개발용, 프로덕션용으로 나눴다. Redis도 마찬가지의 작업을 하려고 한다. 모든 작업은 우분투 18.04 기준임.

## 하려는 것

ncloud 인스턴스 하나에 Redis 인스턴스를 두 개를 만든다.

한 인스턴스는 로컬에서만 접근 가능하고, 다른 인스턴스는 외부에서 접근 가능하게 할 것이다.

외부에서 접근 가능한 인스턴스는 유저이름과 비밀번호로 식별할 것이다.

## 사전 지식

Redis는 기본적으로 식별 수단이 설정되어 있지 않다.

따라서 아무런 식별 수단 없이 외부에 노출하면 보안에 치명적이다.

또한 유저이름과 비밀번호로 식별한다 하더라도 고성능인 Redis 특성 상 병렬로 무작위 공격을 할 수 있으므로 비밀번호는 매우 길게 설정해야 한다.[^auth]

[^auth]: [AUTH | Redis](https://redis.io/commands/auth/)

`/etc/redis/redis.conf` 에서 다양한 것들을 설정할 수 있다.

redis는 한 인스턴스에서 넘버로 식별되는 여러 데이터베이스를 사용할 수 있다. 기본적으로는 16개로 세팅이 되어있다. `db-number`를 `select` 명령어를 통해 지정할 수 있다.

하지만 db-number 단위로 bind-address를 수정할 수 없다. (적어도 내가 알기론) 따라서 개발용, 프로덕션용 인스턴스를 따로 만들 것이다.


## 인스턴스 추가 생성

참조한 사이트들

- [How to run multiple Redis instances on Ubuntu 18.04](https://gist.github.com/Paprikas/ef55f5b2401c4beec75f021590de6a67)
- [Redis - Setting up multiple server instances on a Linux host - 2020](https://www.bogotobogo.com/DevOps/Redis/Redis_Setting_up_mulitple_server_instances_on_a_linux_host.php)
- [run multiple redis instances on the same server for centos](https://gist.github.com/akhdaniel/04e4bb2df76ef534b0cb982c1dc6225b)
- [multiple Redis server instances on a Linux host](https://hellomyblog.tistory.com/9)

### conf 복제

```bash
cd /etc/redis
cp redis.conf redis.dev.conf
```

### conf 세팅

```bash
sudo nano redis.dev.conf
```

변경해야 하는 것

- port
- pidfile
- logfile
- dir

추가로, 이후에 사용자를 등록하고 이를 파일로 저장하여 관리하고 싶다면 `aclfile` 항목을 주석 제거해야 한다.

```
# aclfile /etc/redis/users.acl
```

대충 이런 느낌으로 있을 건데, `#` 제거하여 주석 해제하고 실제로 해당 경로에 `acl` 파일을 **반드시 빈 파일로라도 만들어둬야** 한다.

`aclfile`로 지정한 파일이 없으면 레디스 서버 실행이 안 된다.

```
touch /etc/redis/users.acl
```

### conf 소유자 세팅

```bash
sudo chown redis:redis /etc/redis/redis.dev.conf
```

### working directory 생성

conf 세팅할 때 dir로 지정했던 경로를 만들어야 함.

```bash
mkdir /var/lib/redis-dev
sudo chown redis:redis /var/lib/redis-dev
```

### 서비스 init script 파일 찾기 및 복제

```bash
find / -name "*redis*service*"
```

대충 ```/usr/lib/systemd/system/redis-server.service``` 이런 경로를 찾을 수 있다. 내 경우에는 `/lib/systemd/system/redis-server.service` 였다.

```bash
sudo cp /lib/systemd/system/redis-server.service /lib/systemd/system/redis-dev-server.service
```

이후

```
...
ExecStart=/usr/bin/redis-server /etc/redis/redis.dev.conf
...
PIDFile=/run/redis/redis-dev-server.pid
...
RuntimeDirectory=redis-dev
...
ReadWriteDirectories=-/var/lib/redis-dev
...
Alias=redis.dev.service
```

이 정도 수정하고 `sudo systemctl start redis-dev-server` 로 실행했다.

```bash
redis-cli -p <새 포트번호>
```

로 접속을 확인할 수 있다.

뭔 짓을 해도 connection refused 가 안 돼서 3시간 가량 찾아봤는데 bind 주소가 127.0.0.1이 아니라 127.0.0.7 로 적혀있어서 생긴 문제였다. 127과 포트만 봤지 0.7로 끝난다곤 상상도 못 했다. 대체 뭐하다가 저게 바뀐건지..

## auth 등록 및 bind 주소 변경

### 패스워드를 사용하는 USER 등록

[ACL | Redis](https://redis.io/docs/manual/security/acl/)

[AUTH | Redis](https://redis.io/commands/auth/)

처음에 언급했듯 redis의 비밀번호는 가능한 길게 설정해주는 것이 좋다. 비밀번호를 생성하기 위해 redis `ACL` 의 `GENPASS` 기능을 이용하자.

```
127.0.0.1:6389> ACL GENPASS
"90143be03110fa323e7bec30183b1d57c8d57b5f203a4ff438537b044990fabf"
```

생성된 비밀번호로 계정을 생성한다.

```
127.0.0.1:6389> ACL SETUSER <아이디> on ><비밀번호> ~* &* +@all
OK
```

예를 들어, 아래와 같이 작성한다.

```
127.0.0.1:6389> ACL SETUSER david on >90143be03110fa323e7bec30183b1d57c8d57b5f203a4ff438537b044990fabf ~* &* +@all
OK
```

`~*` 는 모든 키, `&*` 는 모든 하위 채널, `+@all` 는 모든 명령을 의미한다. 디테일하게 허가하고 싶다면 [ACL | Redis](https://redis.io/docs/manual/security/acl/)의 ACL 규칙 항목을 참고하면 된다.

```
127.0.0.1:6389> ACL LIST
1) "user default on nopass ~* &* +@all"
2) "user <아이디> on #<비밀번호 해쉬> ~* &* +@all"
```

`ACL LIST` 로 계정이 생성되었는지 확인한다.

패스워드 없이 로그인할 수 있는 `default` 계정을 제거하고 싶지만 정책상 `default` 계정은 제거할 수 없다.[^def]

[^def]: [ACL DELUSER | Redis](https://redis.io/commands/acl-deluser/)

`default` 계정으로 인증할 수 없도록 비활성화하는 정도가 최선.

```
127.0.0.1:6389> ACL SETUSER default off
OK
```

기존 연결된 커넥션은 유지되기 때문에 새로 `redis-cli` 를 실행하면 `AUTH` 없이는 어떠한 명령어도 실행할 수 없는 것을 확인할 수 있다.

참고로, [protected mode](https://redis.io/docs/manual/security/#protected-mode) 가 활성화되어있을 때(기본으로 활성화되어있다), default 계정이 `nopass`인 경우, 원격 접속을 차단하고 루프백 인터페이스로만 동작한다. 따라서 default 유저를 사용하지 않더라도 패스워드는 지정해줘야 한다.

redis-cli 진입 후 AUTH는 `AUTH <아이디> <비밀번호>` 의 형식으로 진행하면 된다.

### bind 주소 변경

`redis.conf` (또는 복사한 conf 파일) 에서 `bind` 를 수정하면 된다.

내 경우 모든 ipv4 에 대해서 허용하기 위해 `bind *` 로 설정했다.

ipv6은 `-::*` 로 지정하는듯.

### 재시작

```bash
sudo systemctl stop <레디스 서비스 명>
sudo systemctl start <레디스 서비스 명>
```

아까 `redis-server.service` 를 복사해뒀던 파일명이 서비스 명이다. 내 경우엔 `redis-dev-server.service`, 확장자는 생략해도 괜찮다.

원격으로 redis cli 접속이 정상적으로 동작하는 것을 확인했다.