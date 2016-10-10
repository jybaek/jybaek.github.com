---
layout: post
categories: dev
title:  "github 블로그 코드블럭에 스크롤 제거"
---


깃헙 블로그로 오면서 가장 신경쓰였던 부분중에 하나가 바로 아래처럼 코드블럭에 생기는 스크롤이었다. 예제는 예전에 포스팅 했던 내용의 일부 내용 발췌.
<img src="/image/20161010/이미지_2016-10-10_12_09_56.png"  style="max-width:100%;max-height:100%;">

스크롤을 제거하는 방법은 매우 간단하게도 css에 단어 한개만 바꾸면 된다.
<img src="/image/20161010/이미지_2016-10-10_12_10_14.png"  style="max-width:100%;max-height:100%;">

`ovjerflow:scroll` 이라고 되어 있는 부분을 `overflow:hidden` 로 변경해주자.  
그리고 확인해보면...

```
docker run -d -it --name test_os centos:6.7 /bin/bash
```

깔끔해진 것을 확인할 수 있다..
