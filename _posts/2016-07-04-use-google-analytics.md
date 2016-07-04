---
layout: post
categories: dev 
title:  "github에서 Google analytics 사용하기"
---

github으로 블로그를 이전하면서 고려 대상이었던 통계에 대한 처리를  
Google에서 제공하는 웹로그 분석 [analytics](https://www.google.com/analytics/)를 이용하기로 했다.  

일단 Google에서 제공하는 웹로그 분석은 무료인것을 감안했을때  
엄청나게 많은 정보를 제공해준다. 이것에 대한 이야기는 다음을 기약하기로 하고...  
github에서 사용하기 위한 절차를 살펴보도록 한다.  

## [analytics](https://www.google.com/analytics/)에 접속해서 계정을 생성한다
----
<img src="/image/20160704/이미지_2016-07-04_09_11_15.png"  style="max-width:100%;max-height:100%;">

## 우측에 **가입** 버튼을 선택한다.
----
<img src="/image/20160704/이미지_2016-07-04_09_12_38.png" style="max-width:100%;max-height:100%;">

## 필요한 정보를 입력하고 **추적 ID가져오기**를 선택한다.
----
<img src="/image/20160704/이미지_2016-07-04_09_14_59.png" style="max-width:100%;max-height:100%;">
<img src="/image/20160704/이미지_2016-07-04_09_15_28.png" style="max-width:100%;max-height:100%;">

## 약관에 동의하도록 한다.
----
<img src="/image/20160704/이미지_2016-07-04_09_15_56.png" style="max-width:100%;max-height:100%;">

## **추적 ID**와 **script**를 저장하도록 한다. 
----
<img src="/image/20160704/이미지_2016-07-04_09_16_59.png" style="max-width:100%;max-height:100%;">

## 이제 **_config.yml**에 **추적ID**를 추가하자.
----
{% highlight c %}
google_analytics_id: UA-80265650-1
{% endhighlight %}

## google_analytics_id를 활용하는 analytics.html을 생성한다.
----
	{% raw %}
	{% if site.google_analytics_id %}
	<script>
	  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
	  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
	  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
	  })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

	  ga('create', 'UA-80265', 'auto');
	  ga('send', 'pageview');

	</script>
	{% endif %}
	{% endraw %}

## 생성한 analytics.html을 사용한다.
----
	{% raw %}
	<!DOCTYPE html>
	<html>

		{% include analytics.html %} <!-- Here -->
		{% include head.html %}

		<body>

		{% include header.html %}

		<div class="page-content">
		  <div class="wrap">
		  {{ content }}
		  </div>
		</div>

		{% include footer.html %}

		</body>
	</html>
	{% endraw %}
본인의 모든 포스트 layout은 *_layouts/default.html*을 참조하므로 관련 코드 `{%raw%}{% include analytics.html %}{%endraw%}`를 그쪽에 삽입하였다. 각자의 환경에 맞도록 구성해보자.  

이제 모든 과정이 끝났다.  [구글 웹로그 분석 사이트](https://www.google.com/analytics/)에 접속해서 로그인하고 결과를 확인하면 되겠다.
