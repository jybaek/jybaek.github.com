---
layout: post
categories: dev 
title:  "Fix apache with SSL compile error"
---


요즘 보안이 핫이슈다 보니 많은 오픈소스들이 암호화 통신을 지원한다. 암호화 통신에는 여러가지 종류가 있겠지만 통상 SSL/TLS를 기본으로 하고, 이 암호화 방식은 apache에서도 채택되어 사용되고 있다.

하지만 구버전의 apache는 ssl과의 연동을 지원하지 않기 때문에 보안 담당자나 apache를 사용하는 유저들에게는 탈출구가 필요했다.
뭐 이유야 어쨌든 결국 apache+ssl 을 사용할 수 있는 patch 파일이 제공되기에 이르게 된다.

[http://www.modssl.org/example/](http://www.modssl.org/example/)

요약해보면 아래와 같다.

### apache, mod_ssl, openssl 파일을 준비한다.  
```bash
$ lynx http://httpd.apache.org/dist/httpd/apache_1.3.41.tar.gz  
$ lynx ftp://ftp.modssl.org/source/mod_ssl-2.8.31-1.3.41.tar.gz  
$ lynx ftp://ftp.openssl.org/source/openssl-0.9.8g.tar.gz  
$ gzip -d -c apache_1.3.41.tar.gz | tar xvf -  
$ gzip -d -c mod_ssl-2.8.31-1.3.41.tar.gz | tar xvf -  
$ gzip -d -c openssl-0.9.8g.tar.gz | tar xvf -  
```

### OpenSSL 을 빌드한다. 
```bash
$ cd openssl-0.9.8g 
$ ./config 
$ make 
$ cd .. 
```

### mod_ssl을 configure하고 apache를 빌드한다.
```bash
$ cd mod_ssl-2.8.31-1.3.41  
$ ./configure \  
	   --with-apache=../apache_1.3.41 \  
	   --with-ssl=../openssl-0.9.8g \  
	   --prefix=/usr/local/apache  
$ cd ..  
$ cd apache_1.3.41  
$ make  
$ make certificate  
$ make install  
```


### 처리가 끝난 파일 삭제
```bash
$ rm -rf apache_1.3.41  
$ rm -rf mod_ssl-2.8.31-1.3.41  
$ rm -rf openssl-0.9.8g  
```


### SSL 옵션을 주고 apache 시작
```bash
$ /usr/local/apache/bin/httpd -DSSL  
$ netscape https://local-host-name/  
```
