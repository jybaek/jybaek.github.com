---
layout: post
categories: dev 
title:  "warning: deprecated conversion from string constant to 'char*'"
---


C/C++ 컴파일을 진행할 때 아래와 같은 ***warning***를 접할 때가 있다.  

>warning: deprecated conversion from string constant to 'char*'

오류는 대략 아래와 같은 구문에서 발생한다.  

```c
void foo(char *msg) 
foo("hello world")
```

warning이 발생하는 이유는 함수 호출 시에 인자로 `const char *` 가 지정되었는데 함수 원형은 `char *`로 받고 있기 때문이다. 무척 간단하지만 우리는 ***warnning*** 하나도 무시하고 지나가면 안되기 때문에 제거 하도록 하자.

>가끔 컴파일러는 warning이라고 하지만 시스템에 치명적인 문제를 일으키는 경우가 있다.  

제거하는 방법은 여러가지 있겠지만 아래처럼 두 가지 방식을 제안하도록 한다.  

### **함수 원형 수정 (추천)**
----
함수의 원형을 형식에 맞도록 `const char *`로 지정한다.  

>void foo(char *msg)  ==> void foo(**const** char *msg)

### **gcc 옵션**
----
gcc 옵션으로 해당 warning을 제거 하도록 한다.

>-Wwrite-strings
When compiling C, give string constants the type const char[length] so that copying the address of one into a non-const char * pointer produces a warning. These warnings help you find at compile time code that can try to write into a string constant, but only if you have been very careful about using const in declarations and prototypes. Otherwise, it is just a nuisance. This is why we did not make -Wall request these warnings.  

>When compiling C++, warn about the deprecated conversion from string literals to char *. This warning is enabled by default for C++ programs. 

위를 활용해서 gcc 옵션에 `-Wno-write-strings` 를 주도록 한다.  


 
