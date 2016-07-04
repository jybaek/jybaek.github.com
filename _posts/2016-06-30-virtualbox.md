---
layout: post
categories: dev 
title:  "VMWare Image Clone Problem: eth0 Renamed As eth1"
---


**virtualbox**에서 OS(CentOS-6.4 64bit)를 복제하고자 할 때 인터페이스에 문제가 발생한다.  
(모든 네트워크 카드의 MAC 주소 초기화 활성화 했음)  

문제 내용은 eth0가 사라진다는 것인데..  
eth0가 사라지고 eth1가 생긴다.  

이것은 아마도 virtualbox에서 MAC주소를 초기화 하지 못하는 것으로 보인다.  
(32bit 복제 시에는 문제 없음)  

해결 방법은 아래 파일을 직접 수정하는 것이다..  
>/etc/udev/rules.d/70-persistent-net.rules   

<img src="/image/rule.png"  style="max-width:100%;max-height:100%;">

eth0의 MAX주소가 원본 이미지와 동일하다. 그래서 부팅 시에 인터페이스 생성에 실패하고 다음 인터페이스인 eth1을 생성했던 것이다..
**이제 eth0 부분을 삭제하고 NAME="eth1"을 NAME="eth0"로 수정**하도록 한다..

그리고 재부팅 해서 인터페이스가 정상적으로 올라오는 것을 확인하면 된다..
