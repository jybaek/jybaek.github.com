---
layout: post
categories: dev 
title:  "[php] crul을 활용한 버스 도착 정보 크롤링"
---


회사 바로 앞에 버스 정류장이 있는데, 퇴근해서 내려가 보면 눈앞에서 떠나는 버스..  
요즘은 버스가 정류장을 떠나면 잘 세워주지도 않는다.. *(안전을 위해 당연한 거겠지만.)*

그래서 이제는 버스의 도착 시간을 미리 알아야겠다는 생각이 들었다.  
사실 버스 홈페이지나 국토부 등에서 정보를 쉽게 구할 수는 있는데, 로그인이나 웹페이지 여는 것조차 우리에겐 귀찮다..  *(검은 바탕에 흰 글씨)터미널이 눈에 더 익는 것도 사실이고 ^^;;*  

일단 웹에서 제공하는 서비스를 웹페이지 개발자 모드로 분석하고 PHP에서 curl 하기로 했다.   
결과는 XML이기 때문에 그에 맞게 parsing 했다.  

### source code
----

```php
<?php
/* FIXME. change strBusNumber !! */
$url = "http://bus.go.kr/xmlRequest/getStationByUid.jsp?strBusNumber=23248";
$ref_url = "";
$data = array();
function curl($url, $ref_url, $data)
{
	$ch = curl_init();
	curl_setopt($ch, CURLOPT_USERAGENT, "Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/50.0.2661.94 Safari/537.36");
	curl_setopt($ch, CURLOPT_TIMEOUT, 30);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);
	curl_setopt($ch, CURLOPT_URL, $url);
	//curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
	curl_setopt($ch, CURLOPT_REFERER, $ref_url);
	curl_setopt($ch, CURLOPT_HEADER, TRUE);
	curl_setopt($ch, CURLOPT_USERAGENT, $_SERVER['HTTP_USER_AGENT']);
	curl_setopt($ch, CURLOPT_FOLLOWLOCATION, TRUE);
	curl_setopt($ch, CURLOPT_POST, TRUE);
	curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
	ob_start();
	$res = curl_exec ($ch);
	$result = explode("\n", $res); // 7
	echo " ---------------------------------\n";
	for ($i = 0; $i < count($result); $i++) {
		if (strstr($result[$i], "rtNm")) {
			sscanf($result[$i], "\t\t<rtNm>%d</rtNm>", $bus);
			//echo " | Bus : $bus \n";
			printf("| %s \n", " Bus : $bus ");
		} else if (strstr($result[$i], "arrmsg1")) {
			sscanf($result[$i], "\t\t<arrmsg1>%[^<]</arrmsg1>", $msg1);
			printf("|  %s \n", " 1) $msg1 ");
		} else if (strstr($result[$i], "arrmsg2")) {
			sscanf($result[$i], "\t\t<arrmsg2>%[^<]</arrmsg2>", $msg2);
			printf("|  %s \n", " 2) $msg2 ");
		} else if (strstr($result[$i], "gpsX")) {
			echo " ---------------------------------\n";
		}
	}
	system("date");
	return 1;
	ob_end_clean();
	curl_close($ch);
	unset($ch);
}
$result = curl($url, $ref_url, $data);
?>
```

리눅스 터미널에서 실행은.. 대략 이렇게 하면 된다.  

```bash
$ while [ 1 ] ;do clear; php bus.php; sleep 5; done
```

### 출력 결과
----

```bash
 ---------------------------------
|  Bus : 143  
|   1) 4분32초후[2번째 전]  
|   2) 4분58초후[2번째 전]  
 ---------------------------------
|  Bus : 350  
|   1) 7분39초후[3번째 전]  
|   2) 21분33초후[7번째 전]  
 ---------------------------------
|  Bus : 2413  
|   1) 11분57초후[6번째 전]  
|   2) 27분31초후[16번째 전]  
 ---------------------------------
|  Bus : 3422  
|   1) 6분25초후[3번째 전]  
|   2) 18분26초후[7번째 전]  
 ---------------------------------
Wed May 25 16:44:49 KST 2016
```


이제는 버스를 눈앞에서 놓치지 말고 잘 타서 즐거운 퇴근 길이 되어야겠다. 
혹시 코드 사용하실 분들은 코드 앞 부분 **$url**에서 **strBusNumber**의 값만 **버스 정류소 번호**로 변경해주면 되겠다.

