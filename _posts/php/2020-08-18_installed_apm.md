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
## pcre 설치
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


## apache 설치 (apr,apr-util 설치)
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
## apache 서비스 등록
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

## apache 실행
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

참고문헌  
[순백염소](https://blanche-star.tistory.com/entry/CentOS-7-APM-%EC%84%A4%EC%B9%98-Apache-%EC%84%A4%EC%B9%98%EC%BB%B4%ED%8C%8C%EC%9D%BC%EC%84%A4%EC%B9%98%EC%9E%91%EC%84%B1%EC%A4%91)
