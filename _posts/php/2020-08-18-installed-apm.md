---
title: "CentOS7 Apache Php MariaDB설치하기"
categories:  CentOS
tags: centos php apache mariaDB
toc: true
toc_sticky: true
toc_lavel: "목차"
---
apm 컴파일 설치하기  
설치순서 apache → php → mariadb

|os | apache | php | mariaDB|
|---|:------:|:-----:|----:|
|centos7| 2.4.43|7.3.18|10.4.13|

*시작하기전 선행작업
```
yum update   

//의존성 설치
yum install libjpeg* libpng* freetype* gd-* gcc gcc-c++ gdbm-devel libtermcap-devel
yum -y install make pcre-devel
```

# Apache 설치
----
아파치는 설치전 의존성 패키지인 pcre, apr, apr-util이 필수로 설치되어야 한다.  
## 1.pcre 설치
[pcre.org](https://www.pcre.org/)에서 가장 최신버전 다운로드  
`/usr/local/src` 경로에 `apache 디렉토리` 생성 후 생성된 파일에 압축파일 다운로드 및 압축풀기,  
압축푼 파일로 이동후 컴파일 한다.
```
mkdir -p /usr/local/src/apache

cd /usr/local/src/apache

wget https://ftp.pcre.org/pub/pcre/pcre-8.43.tar.gz

tar -xvzf pcre-8.43.tar.gz

cd pcre-8.43
./configure
make
make install
```
pcre 설치완료.


## 2.apache 설치 (apr,apr-util 설치)
[apache.mirror](http://apache.mirror.cdnetworks.com/) 아파치설치에 필요한 `apr, apr-util, httpd`를 최신 버전으로 다운받은 후 압축풀기
```
wget http://apache.mirror.cdnetworks.com/httpd/httpd-2.4.43.tar.gz

wget http://apache.mirror.cdnetworks.com/apr/apr-1.6.5.tar.gz

wget http://apache.mirror.cdnetworks.com/apr/apr-util-1.6.1.tar.gz

tar -xvzf httpd-2.4.43.tar.gz

tar -xvzf apr-1.6.5.tar.gz

tar -xvzf apr-util-1.6.1.tar.gz
```
압축 해제한 apr, apr-util을 `httpd-2.4.43/scrlib` 폴더로 이동.
```
mv apr-1.6.5 httpd-2.4.43/srclib/apr

mv apr-util-1.6.1 httpd-2.4.43/srclib/apr-util
```
이제 httpd-2.4.43을 컴파일한다.
```
./configure --prefix=/usr/local/apache --enable-so --enable-ssl=shared --with-ssl=/usr/local/ssl --enable-rewrite

make

make install
```
## 3.apache 서비스 등록
```
vi /usr/lib/systemd/system/httpd.service // (새로운 파일 작성)
```
```
[Unit]

Description=The Apache HTTP Server


[Service]

Type=forking

PIDFile=/usr/local/apache/logs/httpd.pid

ExecStart=/usr/local/apache/bin/apachectl start

ExecReload=/usr/local/apache/bin/apachectl graceful

ExecStop=/usr/local/apache/bin/apachectl stop

KillSignal=SIGCONT

PrivateTmp=true


[Install]

WantedBy=multi-user.target
```
서비스 파일명이 곧 서비스 이름이기 때문에 httpd든 apache든 원하는 이름으로 생성, 저장

## 4.apache 실행
```
systemctl httpd start
```
### apache 실행 시 에러발생
아파시 실행 후 openssl version 에러발생

```
# cd /usr/local/src

# wget https://www.openssl.org/source/openssl-1.1.1b.tar.gz (해당링크는 항상바뀌니 사이트에 접속후 링크를 따도록하세요)

tar -xvzf openssl-1.1.1b.tar.gz

cd openssl-1.1.1b

./config --prefix=/usr/local/ssl --openssldir=/usr/local/ssl shared

make

make install

vi /etc/ld.so.conf.d/openssl-1.1.1b.conf (새로운파일 생성입니다)

/usr/local/ssl/lib

ldconfig -v
```
아래2개는 필수는 아니지만 접근성을 위해 해두면 좋습니다.
```
ln -s /usr/local/ssl/lib/libssl.so.1.1 /usr/lib64/libssl.so.1.1
ln -s /usr/local/ssl/lib/libcrypto.so.1.1 /usr/lib64/libcrypto.so.1.1
```
`§` 아래2개도 필수까진 아닙니다.
기존버전을 엄밀히 주사용으로 두고 싶다면 아래는 스킵하셔도됩니다.
```
mv /usr/bin/openssl /usr/bin/openssl1.0.2
ln -s /usr/local/ssl/bin/openssl /usr/bin/openssl
```
업데이트된 openssl버전을 확인합니다.
```
openssl version
OpenSSL 1.1.1b  26 Feb 2019
```
`§`를 적용시키지 않았다면
```
openssl version
OpenSSL 1.0.2k-fips  26 Jan 2017

/usr/local/ssl/bin/openssl version
OpenSSL 1.1.1b  26 Feb 2019
```
# MairaDB 설치
---
yum을 이용해 저장소를 직접만들고 원하는 버전의 mariadb를 설치할것이다.
## 1.yum저장소 생성
[mariadb.org](https://downloads.mariadb.org/mariadb/repositories/#mirror=nixnet)에서 os선택을 하고 원하는 버전을 클릭하면 yum 저장소에 입력할 내용이 나옵니다.  
해당 내용을 yum저장소 /etc/yum/repos.d 에 새로운 파일을 만들어 복사/붙여넣기 한다.
```
vi /etc/yum/repos.d/MariaDB.repo
>>
 내용 붙여넣기
```

## 2.yum으로 설치 후 서비스 실행
```
yum install MariaDB-server MariaDB-client

systemctl start start
```

## 3.보안설정
설치 후 기본적인 보안설정을 해줍니다.  
[참고사이트](https://dejavuqa.tistory.com/349)
```
mysql_secure_installation
```
```
Enter current password for root (enter for none):
//먼저 root 계정과 비전을 입력하라고 합니다. 초기 설치 mysql root의 비번은 없으므로 엔터 입력  

Set root password? [Y/n] y
New password:
Re-enter new password:
//비번 설정, y입력후 비밀번호 입력

Remove anonymous users?
//다음은 anonymous 계정을 제거할 건지 묻습니다. 인증된 계정만 접속하기 위해 y입력


Disallow root login remotely? [Y/n] y
//원격으로 root접속 차단 여부, (지금은 차단후 일반 사용자 계정으로 접속하겠다)

Remove test database and access to it? [Y/n] y
//테스트 베이스를 지우고 접근도 못하게 한다

Reload privilege tables now? [Y/n] y
//설정한 내용들을 적용할 것인가.

```

## 4.mysql실행, 사용자 계정 설정
다 완료한 후 mysql 실행
```
mysql -u root -p
```
root계정의 원격접속을 막았기 때문에, 원격접속이 가능한 사용자 계정을 만들어 줍니다. (+모든 데이터베이스에 접속가능)
```
CREATE USER 'userID'@'%' IDENTIFIED BY 'userPW' ;
GRANT ALL PRIVILEGES ON *.* TO 'userID'@'%' IDENTIFIED BY '@userPW' WITH GRANT OPTION;
FLUSH PRIVILEGES ;
```
완료 후 서비스 재실행
```
systemctl mysql restart
```


# PHP설치
---
사전 패키지 설치
```
yum install -y libxml2-devel libcurl-devel gd-devel libmcrypt-devel
```

(libjpeg나 libpng기능을 굳이 사용하지 않는다면 설치할 필요 없습니다. 아래의 configure는 포함입니다.
필요없다면 적절히 수정해주세요.  
만약 그누보드 이용자시라면 반드시 설치해야합니다.)

## 1.php설치
```
cd /usr/local/src

tar -xvzf php-7.3.18.tar.gz

cd php-7.3.18

./configure --prefix=/usr/local/php --with-mysqli --with-openssl=/usr/local/ssl --with-pdo-mysql=mysqlnd --with-apxs2=/usr/local/apache/bin/apxs --with-config-file-path=/usr/local/apache/conf --with-zlib --disable-debug --enable-calendar --enable-ftp --enable-sockets --enable-sysvsem --with-gd --with-jpeg-dir=/usr/lib64

make

make install
```
php 설정파일을 복사합니다. development추천
```
cp /usr/local/src/php-7.3.18/php.ini-development /usr/local/apache/conf/php.ini

vi /usr/local/apache/conf/php.ini
```
socket 이라는 항복을 검색한뒤/var/lib/mysql/mysqlsok 을 입력해줍니다(설치과정에 따라 변경될 수 있으니 ./etc/my.cnf에서 soket을 검색 후 확인하고 입력하세요)
```
; Default socket name for local MySQL connects.  If empty, uses the built-in
; MySQL defaults.
; http://php.net/mysqli.default-socket
mysqli.default_socket = **/var/lib/mysql/mysql.sock**
```
## 2.아파치 설정 값 변경
```
vi /usr/local/apache/conf/httpd.conf

.
.
.

<IfModule dir_module>

    DirectoryIndex index.html  **index.php**  index.jsp

</IfModule>

.

.
AddType application/x-compress .Z
AddType application/x-gzip .gz .tgz
**AddType application/x-httpd-php .php .html .htm .inc
 AddType application/x-httpd-php-source .phps**
```
/으로 검색 후 적절한 위치에 굵은 글씨를 추가 해준다.  
아파치 서비스를 재시작 해줍니다.
```
systemctl stop httpd
systemctl start httpd
```
참고문헌  
[순백염소](https://blanche-star.tistory.com/entry/CentOS-7-APM-%EC%84%A4%EC%B9%98-Apache-%EC%84%A4%EC%B9%98%EC%BB%B4%ED%8C%8C%EC%9D%BC%EC%84%A4%EC%B9%98%EC%9E%91%EC%84%B1%EC%A4%91)
