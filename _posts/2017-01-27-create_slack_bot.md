---
layout: post
categories: dev
title:  "슬랙봇 만들기"
---


협업툴로 굉장히 명성높은 슬랙에 보면 봇이 존재한다. 기본적으로 협업 공간을 생성하면 SlackBot 이라는 놈이 있는데, 이 SlackBot은 슬랙을 사용하는데 미비한 도움을 주는 .. 그다지 스마트 하지 못한 헬퍼 봇이다. (하물며 영어로 말한다. 당연한건가?)

하지만 슬랙이 명성 높은데는 다 이유가 있다. 다른 APP들과 연동이 쉽고 많은 API를 제공하기 때문인데 그 중에 Bot을 생성할 수 있도록 도와주는 BOT API도 있다. 그렇다면 우리 기호에 맞도록 헬포봇을 만들어보자.

우선 아래 여기 [링크](https://api.slack.com/bot-users)를 통해 봇을 생성할 수 있는 페이지로 이동한다.


그리고 아래 화면에서 "creating a new bot user" 를 선택하도록 한다.
<img src="/image/20170127/이미지_2.jpg"  style="max-width:100%;max-height:100%;">


브라우저에 슬랙이 로그인되어 있었다면 자동으로 해당 슬랙쪽으로 이동이 될 것이고, 그렇지 않다면 봇을 생성할 슬랙에 로그인할 수 있는 페이지가 열릴 것이다. 

그리고나면 Bot의 이름을 정할 수 있다. 이 이름을 통해 채널에 초대하거나 1:1 대화를 진행할 수 있다. 봇이라는 특성을 생각해서 가급적 쉬운 이름으로 지정하고 다음 단계를 진행하도록 하자.
<img src="/image/20170127/이미지_3.jpg"  style="max-width:100%;max-height:100%;">

다음 단계는 Bot에 API Token을 얻고, 이미지를 입힐 수 있는 등의 Bot 설정 페이지다. 우선 언제든 수정 가능한 부분이므로 Token만 복사하고 넘어가도록 하자. (아래 save integration으로 저장해주면 된다.)
<img src="/image/20170127/이미지_5.jpg"  style="max-width:100%;max-height:100%;">


이제 준비는 끝났다. 본격적으로 Bot을 코딩하면 된다. 위 Token은 슬랙와 Bot이 프로세싱되는 환경과의 연결을 도와줄 것이다.

이제는 본격적인 Bot coding을 진행해야 한다. 일단 적당한 서버를 준비하고 slack api를 설치하도록 해야하는데, 이때는 python을 사용하도록 한다. python으로 프로세스를 동작시키는 방법은 여러가지가 있지만 여기서는 virtualenv라는 기법을 사용한다. virtualenv에 대해 간단히 소개하자면, 서버에 여러 종류의 python 설치와 라이브러리 호환등의 충돌에 대비해 프로세스를 동작시키는 독립적인 환경을 구축하는 것이다.

터미널에서 아래 명령어를 실행하도록 하자. 여기서는 testbot 이라는 이름으로 구축했다.
```bash
$ virtualenv testbot
```

구축된 환경으로 접속은 아래와 같이 실행한다.
```bash
$ source testbot/bin/activate
```

명령어를 실행하면 프롬프트 앞에 (testbot) 이 추가된 것을 확인할 수 있다.
```bash
(testbot) $
```

이제 가상환경에 접속되었으니 이곳에 slack api를 설치하도록 한다.
```bash
(testbot) $ pip install slackclient
```

slack bot과 통신하기 위해 slack bot의 ID를 얻어와야 한다. 아래와 같은 방식으로 얻어올 수 있다. 빨간색으로 마킹된 부분은 각자의 서버에 맞도록 설정해야 한다. token은 위에서 얻은 slack bot의 token을 사용하면 된다.
```bash
(testbot) $ curl https://NAME.slack.com/api/auth.test?token=xoxb-120374760084-IoTPIndlbOw8EpTy0JR
```

결과는 json 형태로 출력되는데 "user_id":"U3JBON2.." 로 되어 있는 부분에서 U3JBON2... 를 복사해서 앞으로 사용하게 되겠다. 아래와 같이 환경변수를 설정해주도록 한다.
```bash
(testbot) $ export SLACK_BOT_TOKEN=''xoxb-120374760084-IoTPIndlbOw8EpTy0JR"
(testbot) $ export BOT_ID='U3JBON2'
```

이제 모든 준비가 끝났다. 아래 github 에 누군가 작성해놓은 slack bot 스크립트를 기반으로 입맛에 맞는 봇을 만들면 된다 ! 여기를 참고하도록 하자.
[https://github.com/mattmakai/slack-starterbot/blob/master/starterbot.py](https://github.com/mattmakai/slack-starterbot/blob/master/starterbot.py)

예제를 참고해서 스크립트를 만들었으면 아래처럼 실행하도록 하자.
```bash
(testbot) $ python testbot.py
```

프로세스가 실행될 것이고 이제 슬랙에서 @testbot 에게 말을 걸면 된다. github에서 받은 소스를 토대로 좋은 봇을 만들 수 있기를 기대하며 원작자에게 감사한 마음을 항상 갖도록 하자.


덧. 위 내용은 아래 링크를 기반으로 재구성되었습니다.

[https://www.fullstackpython.com/blog/build-first-slack-bot-python.html](https://www.fullstackpython.com/blog/build-first-slack-bot-python.html)


