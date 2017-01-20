---
layout: post
categories: dev
title:  "오픈소스 컨트리뷰트 하기"
---


아마도 나와 유사한 업무를 하는 시스템 프로그래머는 대부분 회사 업무에 치여 다른 무언가를 한다는 것은 상상하기 힘들 것이다. 간혹 그렇지 않은 곳도 많지만 ...

여하튼 시간이 종종 생겨도 다른 무언가를 뚝딱뚝딱 만들거나 미뤘던 공부를 하면서 시간을 보냈는데 우연찮게 GitHub 프로젝트에 발을 담그게 됐다. 나의 [첫번째 컨트리뷰트](https://github.com/tensorflowkorea/tensorflow-kr/pull/115)는 [TensorFlow-kr 문서 번역](https://github.com/tensorflowkorea/tensorflow-kr). 영어가 능통해서 문장을 술술 풀어 쓰지는 못하고. 또한 TensorFlow의 구조에 대한 이해가 부족하여 일단 다른 사람이 번역한 내용의 소소한 오타 수정이나 마크다운 문법에 대한 오류를 정정해서 컨트리뷰트 하고 있다.

컨트리뷰트에 대한 벙법은 홍콩과기대의 [김성훈](https://github.com/hunkim) 교수님이 어떤 절차를 수행하면 되는지 너무나 상세히 동영상([Github Flow 이론](https://www.youtube.com/watch?v=x-b_ij22vWg ), [Github Flow 데모](https://www.youtube.com/watch?v=GeFkVB8w7uM ))으로 설명을 해주시기 때문에 다른 어려움은 없다. 다만 나도 무언가 흔적을 남기기 위해 컨트리뷰트 하는 방법에 대해 몇 가지 기록하고자 한다.

## **오픈소스 찾기**

제일 중요한 부분이다. 평소 관심이 있거나 참여하고 싶은 프로젝트를 GitHub 에서 찾는 것으로 부터 오픈소스 컨트리뷰트는 시작된다. `Search GitHub` 이라는 상단 텍스트박스에 대략적인 검색어를 입력하면 쉽게 찾을 수 있다. 

## **참여를 위해 repository fork**
프로젝트 참여를 위해 오픈소스 저장소를 내 계정으로 fork 하도록 한다. 보통 오픈소스 저장소는 일반 유저에게 push 권한을 주지 않기 때문에 fork한 이후 내 저장소에서 수정을 해야 한다. fork를 했다면 일단 작업을 하고자 하는 PC에 clone을 진행하도록 한다.

## **clone 이후 단계**
일단 해당 프로젝트에 한번 PR하고 끝이 아니라 꾸준한 기여를 할 생각이라면 *branch*를 관리하는 것이 좋다. 이유는 간단한데 약간 설명하자면 A에 대한 수정을 서버에 PR하고 나서 (관리자에 의해) merge가 되기 전에 B에 대한 수정을 하고 다시 PR하게 되면 A,B 두가지가 1개의 PR로 관리된다. 서로 다른 이슈이지만 그렇게 된다. 이건 GitHub의 장점이다. 왜냐하면 PR하고 feedback을 받아 내용을 수정해서 다시 PR을 하는 등의 과정이 1개로 관리되기 때문이다. A, B 이슈를 branch로 관리하게 되면 별개의 이슈로 등록되기 때문에 branch로 PR하는 습관이 좋겠다.

## **push**
branch에 대한 언급을 위한 글이 아니므로.. 우선 branch를 생성하는 방법은 다음으로 대체한다.

```
% git checkout -b [브랜치 이름] :
% git checkout -b fixing_type_error
```

branch가 되고 소스 수정까지 되었다면 이제 생성한 branch의 변경이력을 나의 저장소에 push하도록 한다.  

```
% git push -f origin fixing_type_error
```
저장소에 branch가 없다면 생성까지 자동으로 된다.

## **PR (Pull Request)**
이제 GitHub에서 확인하면 친절하게 PR을 안내하는 버튼이 생겨있다. (Compare & pull request)
<img src="/image/20161014/PR2.png"  style="max-width:100%;max-height:100%;">
버튼을 누르면 PR의 다음단계로 진입할 수 있다. 다음 단계는 PR에 대한 설명을 작성하는 너무나도 간단한 화면이라 생략한다 (...)

## **결과 확인**
PR을 마쳤으면 제대로 되었는지 확인하도록 한다. 
아래는 김성훈 교수님의 저장소에 PR 해본 것...
<img src="/image/20161014/PR.png"  style="max-width:100%;max-height:100%;">

페이스북 그룹 `개발자 영어`에서 교수님의 그룹내 스팸 글 방지 관련 내용이 잠깐 언급되었는데 그때 기억이 나서 원리가 궁금해 저장소까지 훔쳐보게 되었다. 그 내용중에 API를 참조할 수 있는 링크 주소가 변경되었길래 PR을 던졌는데 감사하게도 받아 주셨다. GitHub 에서 Open과 Closed로 구분해서 내가 그간 컨트리뷰트 한 내용에 대한 목록이 확인 가능하다.

>Open은 PR되어 대기중인 상태, Closed 는 PR이 원본 저장소에 merge된 상태다.  
- 기각되도 Closed에 나오려나?


## **결론**
간단하게 오픈소스 컨트리뷰트에 대해 확인해봤는데 꽤 재미있다. `오픈소스`라고 불리는 순간 이미 공개 프로젝트이고 다른 사람의 참여가 가능하다는 의미로 생각된다. GitHub은 이러한 측면에서 상당한 생태계와 프로세스를 갖춘 엄청난 커뮤니티를 형성해주었다. 지금 이 시대에 누릴 수 있는 즐거움은 열심히 누리도록 하자 :)

