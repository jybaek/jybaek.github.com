---
layout: post
categories: dev 
title:  "OpenSSL make install 오류"
---

```bash
$ ./config --prefix=./build shared  
$ make  
$ make install  
```

위는 일반적인 *application* 컴파일 과정인데 약간의 문제가 있다.
*Makefile*의 구현법에 따라 차이는 어느정도 있겠지만 OpenSSL의 경우는 위 처럼 **prefix를 상대경로로 설정** 했을 때 아래와 같은 문제가 발생한다.

```bash
installing man3/SSL_write.3  
making install in crypto...  
make[1]: Entering directory '/root/link/openssl-1.0.2h/crypto'  
cp: cannot create regular file './build/include/openssl/crypto.h': No such file or directory  
chmod: cannot access './build/include/openssl/crypto.h': No such file or directory  
cp: cannot create regular file './build/include/openssl/opensslv.h': No such file or directory  
chmod: cannot access './build/include/openssl/opensslv.h': No such file or directory  
cp: cannot create regular file './build/include/openssl/opensslconf.h': No such file or directory  
chmod: cannot access './build/include/openssl/opensslconf.h': No such file or directory  
cp: cannot create regular file './build/include/openssl/ebcdic.h': No such file or directory  
chmod: cannot access './build/include/openssl/ebcdic.h': No such file or directory  
cp: cannot create regular file './build/include/openssl/symhacks.h': No such file or directory  
chmod: cannot access './build/include/openssl/symhacks.h': No such file or directory  
cp: cannot create regular file './build/include/openssl/ossl_typ.h': No such file or directory  
chmod: cannot access './build/include/openssl/ossl_typ.h': No such file or directory  
make[1]: *** [install] Error 1  
make[1]: Leaving directory `/root/link/openssl-1.0.2h/crypto'  
make: *** [install_sw] Error 1  
```

고로 prefix를 지정할때는 절대 경로로 설정하는 습관을 들여야 겠다.  
물론 상대경로가 굳이 꼭! 필요한 경우도 있기는 하다 (...)  

여하튼 나와같은 고민을 했던 누군가가 [stackoverflow](http://stackoverflow.com/questions/23343345/getting-cp-
cannot-create-regular-file-no-such-file-or-di
rectory-in-openssl)에도 있었다.  

역시 세상은 넓고, 개발자도 많고... 더 나은 사람이 되려면 더 많은 노력을 하고 더 많은 경험을 해야...

