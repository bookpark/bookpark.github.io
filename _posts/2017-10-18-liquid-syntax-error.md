---
layout: post
title: "Jekyll에서 Liquid syntax error 해결하기"
---

개발 블로깅을 하다보면 `Liquid syntax error` 메시지를 은근 자주 보게 된다. 이 에러 메시지가 뜨면 힘들게 작성한 포스트는 보여지지 않게되고… 이 에러를 간단하게 해결한 방법을 소개하고자 한다. 

### 템플릿 언어

이 에러 메시지는 보통 템플릿 언어를 사용했을 때 뜨는 것을 확인했다.

예)

![예](http://ohmywork.co.kr/wp-content/uploads/2017/10/liquid_syntax_error_1.png)

이렇게 단독으로 사용되는 템플릿 언어 같은 경우에 아래 내용을 참고해 수정해보자.

해결)

![해결](http://ohmywork.co.kr/wp-content/uploads/2017/10/liquid_syntax_error_2.png)

위와 같이 에러가 나는 부분을 위 코드로 감싸주기만 하면 에러는 해결!

빈 칸이 생기는 부분은 감수해야한다.. 더 좋은 방법이 있다면 알려주세요!