---
layout: post
categories: dev 
title:  "Kali Linux에 Docker 설치"
---


개발용으로 사용하는 OS는 버전이 낮기 때문에 docker을 지원하지 않는다. docker를 지원하기 위한 OS의 최소 사양은 docker의 [**docs**](https://docs.docker.com/v1.8/installation/centos/)에 명시된 대로 kernel 버전이 최소 3.10 이상이어야 한다.   

그래서 전에 놀이용으로 설치한 Kali Linux에 docker를 설치하고 사용해보기로 했다.  
[**여기**](https://docs.docker.com/engine/installation/)에 설치법이 자세히 나와있지만 누군가를 위해 옮겨 적어본다.  

일단 Kali Linux는 debian-wheezy 바탕이므로 Debian 쪽으로 설명하도록 한다.  

### **Update your apt repository**
1.*/etc/apt/sources.list.d/backports.list* 파일을 에디터로 열도록 한다. (파일이 없다면 생성해주면 되고, 내용이 있다면 삭제하도록 하자 )
그리고 아래 내용을 입력 한다.  

```bash
$ deb http://http.debian.net/debian wheezy-backports main
```

2.package 정보를 업데이트 한다.  

```bash
$ apt-get update
```

3.오래된 repository를 삭제하도록 한다.  

```bash
$ apt-get purge "lxc-docker*"
$ apt-get purge "docker.io*"
```

4.package 정보를 업데이트하고 APT는 https로 통신되므로 CA 인증서를 인스톨 하도록 한다.  

```bash
$ apt-get update
$ apt-get install apt-transport-https ca-certificates software-properties-common
```

5.GPG 키를 등록한다.

```bash
$ apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
```

6.*/etc/apt/sources.list.d/docker.list* 파일을 에디터로 열도록 한다. (파일이 없다면 생성해주면 되고, 내용이 있다면 삭제하도록 하자)  
OS의 종류에 따라 아래 내용을 입력하도록 하자.

```bash
$ # On Debian Wheezy  
$ deb https://apt.dockerproject.org/repo debian-wheezy main  
$ # On Debian Jessie  
$ deb https://apt.dockerproject.org/repo debian-jessie main
$ # On Debian Stretch/Sid  
$ deb https://apt.dockerproject.org/repo debian-stretch main
```

7.package의 index를 업데이트 하도록 한다.  

```bash
$ apt-get update
```

8.APT를 검증하고 docker-engine repositry를 받아오도록 한다.  

```bash
$ apt-cache policy docker-engine
```

### **Install Docker**
1.APT package의 index를 업데이트 하도록 한다.  

```bash
$ sudo apt-getupdate
```

2.docker를 설치한다.   

```bash
$ sudo apt-get install docker-engine
```

3.docker를 시작한다.  

```bash
$ sudo service docker start
```

4.docker가 올바르게 설치되었는지 확인한다.  

```bash
$ sudo docker run hello-world
```
