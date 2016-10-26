---
layout: post
categories: dev
title:  "도쿠위키 패스워드 분실"
---


### 도와주세요! 패스워드를 분실했어요!

[도쿠위키](https://www.dokuwiki.org/dokuwiki#)에서 일반 사용자가 패스워드를 분실한 경우에는 관리자가 찾아주면 된다. 혹은 평범하게 **패스워드 찾기** 기능을 사용해도 되지만 관련 설정이 없는경우 패스워드 찾기 기능이 동작하지 않기 때문에 결국 관리자에게 문의해야 한다.

### 도와주세요! **관리자**도 패스워드를 분실했어요!

일단 일반 사용자는 생략하고 가장 큰 문제는 관리자가 패스워드를 분실한 경우다. 메일로 패스워드를 찾는 것조차 되지 않는 경우에는 서버 터미널(ssh)에 직접 접속해서 해결해야 한다.

터미널 접속해서도 패스워드를 찾는 것은 불가능하다. 왜냐하면 기본적으로 단방향 암호화이기 때문에 암호화된 값을 통해 원래 값을 찾는 것이란 불가능하다. 그래서 우리는 분실한 패스워드를 대신해 특정 패스워드로 초기화해서 사용해야 한다. 초기화에 앞서 도쿠위키 패스워드가 어떤 식으로 관리되는지 잠깐 살펴보자.

### 패스워드 초기화

패스워드는 `conf/users.auth.php` 파일에 계정을 생성하거나 정보 변경 시에 아래와 같은 형태로 자동으로 기록된다.

    jybaek:$1$test$nqEFoiSKcbjVQ1.f02YtG/:홍길동:jybaek@example.com:admin,user

하나씩 천천히 살펴보자. 일단 구분자는 `콜론(:)`이다. 문장을 뜯어서 보면 아래처럼 구분된다.

    아이디:$hash함수$salt$패스워드:이름:이메일:그룹

hash 함수 부분에 $1은 도쿠위키의 기본 패스워드 암호화 설정인 smd5 (salt + md5)를 나타낸다. 각각에 대한 것은 [여기](https://www.dokuwiki.org/config:passcrypt)서 확인하면 된다.

그럼 salt 값을 줘서 임의의 패스워드를 생성해보자. 방법은 [stackexchange의 답변](http://unix.stackexchange.com/a/76337 )을 참조하도록 한다. 요즘 python이 핫하므로 python으로 테스트해본다.

    python -c "import crypt, getpass, pwd; print crypt.crypt('admin', '\$1\$test\$')"

`admin`이라는 문자열을 `test`라는 salt로 사용해서 $1(smd5)로 암호화 하라는 의미다.  
이제 패스워드를 찾았으니 다시 도쿠위키를 즐기면 되겠다.