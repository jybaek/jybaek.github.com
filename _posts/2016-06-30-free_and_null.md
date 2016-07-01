---
layout: post
categories: dev 
title:  "동적 메모리를 free하고 NULL로 세팅하는 이유"
---

OpenSource를 보거나 동적 메모리 관련 교육을 살펴보면 아래와 같은 코드를 종종 볼 수 있다.  
아래 코드는 사용한 메모리를 해제하는 구문이다.
{% highlight c %}
free(pointer);
pointer = NULL;
{% endhighlight %}

사실 free만으로 메모리는 해제된다.  

하지만 문제가 되는 부분은 **해제된 메모리 영역이 우연찮게 다른 곳에서 사용되는 경우** 프로그램의 오동작을 피하기 어렵다는 것. 또한 디버깅에도 쉽지 않다.  

아래 코드를 통해 극단적인 예시를 살펴본다. 
{% highlight c %}
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(void)
{
    char *str = NULL;
    int *addr = 0;

    str = (char *)malloc(10);
    strncpy(str, "test", strlen("test"));
    printf("str(%s)(%x) \n", str); // pointer use 0x1562010 address

    /* 억지스럽게 같은 영역을 사용하기 위해 str의 주소를 가져온다. */
    addr = (int *)str;

    free(str);

    strncpy((char *)addr, "good afternoon", strlen("good afternoon"));
    printf("str(%s)(%x) \n", str);

    return 0;
}
{% endhighlight %}

두 곳에서 printf를 통해 출력했는데 결과는 어떨까? 
>**root@localhost#** ./a.out   
str(test)(1562010)   
str(good afternoon)(1562010)

예상처럼 *str*에는 값이 들어 있다. 이 경우는 간단한 코드이기 때문에  오염(?)이 안되서 그나마 양반이다.  
*str*변수를 사용하기 전에 *if (str != NULL)* 과 같은 체크를 하더라도  *str*은 *free()*만 되었을 뿐 여전히 메모리 주소를 갖고 있기 때문에 조건문에 매치되지 않는다.

*free()*후 *NULL*로 포인터를 세팅하는 것은 **defensive style**이라고도 표현하지만, 이러한 코딩은 선택이 아닌 필수가 되어야 겠다.
