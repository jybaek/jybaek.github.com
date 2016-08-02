---
layout: post
categories: dev 
title:  "SSL Handshake (Diffie-Hellman)"
---


<script type="text/x-mathjax-config">
    MathJax.Hub.Config({
      jax: ["input/TeX", "output/HTML-CSS"],
      tex2jax: {
        //inlineMath: [ ['$', '$'], ["\(", "\)"] ],
        inlineMath: [ ['$', '$'] ],
        displayMath: [ ['$$', '$$'] ],
        processEscapes: true,
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre', 'code']
      }
      //,
      //displayAlign: "left",
      //displayIndent: "2em"
    });
</script>
#### **Message 1: "Client Hello"**
RSA방식 처럼 *client hello*는 protocol version, random, cipher suites 목록, 그리고 선택적으로 SNI(Server Name Indication)를 확장할 수 있다. 만약 client가 ECDHE를 사용하려면 지원하는 목록을 포함해야 한다. 만약 생략되거나 부정확하다면 디버깅 하기 곤란할 것이다.  

#### **Message 2: "Server Hello"**
*client hello*를 받으면 서버는 handshake를 계속 진행하기 위한 파라메터로 ECDHE가 포함된 curve를 선택한다. 서버 *"hello"* 메시지는 random값과 server가 선택 한 cipher suite, 그리고 인증서가 포함되어 있다.  

*RSA*와 *Diffie-Hellman* handshake는 이 시점에 새로운 메시지와 함께 다르게 시작한다. (RSA의 다음 단계는 *Client Key Exchange*지만 *Diffi-Hellman*은 Server Key Exchange 과정이 새로운 메시지로 전송 된다)  


#### **Message 3: "Server Key Exchange"**
*Diffie-Hellman*은 key 교환을 시작할 목적으로 서버는 약간의 시작 파라메터가 필요하고, 그것을 client에게 전송한다. --- 이것은 $$g^a$$에 해당한다. 서버는 또한 입증하기 위한 방법이 필요하다. 그것은 개인키의 제어이다. 그래서 서버는 이 시점까지의 모든 메시지의 전자서명을 계산한다. *Diffie-Hellman* 파라메터와 서명은 이 메시지에 모두 전송된다.  

#### **Message 4: "Client Key Exchange"**
검증이 끝난 인증서는 신뢰하고, client는 server로부터 온 전자서명이 연결하려고 시도하는 위치에 속하는지 유효성을 검사한다. 또한 client는 *Diffie-Hellman*의 나머지 절반을 전송한다. ( $$g^b$$에 상응하는)  

이 시점에 양쪽은 *Diffie-Hellman* 파라메터의 pre-master secret을 계산한다. ( $$g^{ab}$$에 상응하는 ). pre-master secret으로 client와 server는 동일한 세션키를 사용할 수 있다. client와 server는 다음 메시지부터 암호화될 것이라는 것이 표시된 메시지를 교환한다.   

*RSA handhake*처럼 *Diffie-Hellman handshake*도 client와 server가 "Finished" 메시지를 교환할때까지 공식적으로 완전하다. 이후 client와 server의 통신은 세션키와 함께 암호화 된다.  


#### **출처**
[https://blog.cloudflare.com/keyless-ssl-the-nitty-gritty-technical-details/](https://blog.cloudflare.com/keyless-ssl-the-nitty-gritty-technical-details/)
