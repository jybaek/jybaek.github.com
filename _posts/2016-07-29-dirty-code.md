---
layout: post
categories: dev 
title:  "아무리 코딩 스타일이 개인취향이라지만..."
---


은근히 혐오스러운 코드를 만날때가 있다. 토발즈씨가 그랬나? ***if문 중첩이 3개 이상이면 그 코드는 문제가 있는 코드다.*** 무슨말인지 이해할 필요도 없다. 내가 바로 예제를 보여주지.  
아래는 short 변수의 16개 bit가 모두 1인지 검사하는 코드다. 잘 봐라.  

{% highlight c %}
#include <stdio.h>

int arrow_func(void)
{
	short val = 0xFFFF;

	if (val & (1<<15)) {
		if (val & (1<<14)) {
			if (val & (1<<13)) {
				if (val & (1<<12)) {
					if (val & (1<<11)) {
						if (val & (1<<10)) {
							if (val & (1<<9)) {
								if (val & (1<<8)) {
									if (val & (1<<7)) {
										if (val & (1<<6)) {
											if (val & (1<<5)) {
												if (val & (1<<4)) {
													if (val & (1<<3)) {
														if (val & (1<<2)) {
															if (val & (1<<1)) {
																if (val & (1<<0)) {
																	return 1;
																}
															}
														}
													}
												}
											}
										}
									}
								}
							}
						}
					}
				}
			}
		}
	}

	return 0;
}

int main(void)
{
	int ret = 0;

	ret = arrow_func();

	if (ret) {
		printf("same 0xFFFF\n");
	} else {
		printf("not same 0xFFFF\n");
	}

	return 0;
}
{% endhighlight %}

어때보이는가? 혹시 ***말도 안되는 소리 하지마!! 이런 코드는 존재할리가 없어!!***라고 말하고 싶은가? 사실 이 코드는 극단적인 상황을 보이는 것 뿐이다.  
실제 필드에서도 코드가 복잡해지고 조건이 많아지다 보면 이런 유사한 코드가 심심치 않게 보인다. 왠지 모르게 바꿔주고 싶은 그런 마음... 하지만 누구 말대로 코딩 스타일은 개인의 취향이기 때문에 터치하면 안된다..  
`코드리뷰`에서도 이 부분은 굉장히 민감한 부분이다. (***하지만 너무하잖아!!!***)  
이대로 끝내면 안될것 같다. 그래서 내가 선호하는 방식을 잠깐 기술해본다.  

{% highlight c %}
/* 일단 극단적인 예시의 회피용 이므로 for문은 사용하지 않음. 
 * 단순히 arrow를 없애는데 초점을 맞춘 코드 */
int arrow_func(void)
{
	short val = 0xFFFF;
	if (!(val & (1<<15))) return 0;
	if (!(val & (1<<14))) return 0;
	if (!(val & (1<<13))) return 0;
	if (!(val & (1<<12))) return 0;
	if (!(val & (1<<11))) return 0;
	if (!(val & (1<<10))) return 0;
	if (!(val & (1<<8))) return 0;
	if (!(val & (1<<7))) return 0;
	if (!(val & (1<<6))) return 0;
	if (!(val & (1<<5))) return 0;
	if (!(val & (1<<4))) return 0;
	if (!(val & (1<<3))) return 0;
	if (!(val & (1<<2))) return 0;
	if (!(val & (1<<1))) return 0;
	if (!(val & (1<<0))) return 0;

	return 1;
}
{% endhighlight %}

좋은 코딩이란 가독성과 떼려야 뗄 수 없는 관계다. 가독성을 해치는 ***arrow***는 우리 왠만하면 지양하자...
