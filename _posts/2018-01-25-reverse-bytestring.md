---
layout: post
comments: true
title: "Django 2.0 UnicodeDecodeError 해결법"
description: "bytestring의 reverse(), decode() 관련 UnicodeDecodeError를 해결해보자"
tags: [web, django]
---


최근 이메일 인증을 다시 구현하기 위해 전에 쓰던 코드를 적용하고 테스트를 하는데...

{% include image.html path="documentation/django_unicode_decode_error.png" path-detail="documentation/django_unicode_decode_error.png" alt="Decode Error" %}

<p style="text-align: center; font-size: 8; font-style: italic;">아니 이게 왠 처음보는 에러메시지인가..</p>

정신 차리고 언제나 해답이 있는 장고 공식문서를 찾아가보았다.
아니나 다를까 그 곳엔 역시 해답이 있었다.

{% include image.html path="documentation/django_2.0_bytestrings.png" path-detail="documentation/django_2.0_bytestrings.png" alt="Bytestrings" %}

Django는 1.11 버전까지 Python 2를 지원하기 위해 bytestring과 unicode string을 모두 지원했었지만 Django가 2.0으로 버전 업이 되면서 Python 2는 더 이상 지원하지 않게 되면서 bytestring 사용을 제한한다는 얘기이다.

그리고 친절하게 reverse() 전에 force_text() 대신 먼저 decode()를 호출하라는 우리의 공식문서.

```python
# 회원가입 api 쪽 encode 부분
...

html_message = render_to_string('user_activate.html', {
    'domain': current_site.domain,
    'uid': urlsafe_base64_encode(force_bytes(user.pk)),
    'token': urlsafe_base64_encode(force_bytes(user.token)),
})

...
```
```python
# 이메일 인증 메서드 decode 부분
def activate(request, uidb64=None, token=None):
    uid = force_text(urlsafe_base64_decode(uidb64))
    token = force_text(urlsafe_base64_decode(token))

    ...
```

이러했던 코드를...

```python
# 회원가입 api 쪽 encode 부분
...

html_message = render_to_string('user_activate.html', {
    'domain': current_site.domain,
    'uid': urlsafe_base64_encode(force_bytes(user.pk)).decode(),
    'token': urlsafe_base64_encode(force_bytes(user.token)).decode(),
})

...
```
```python
# 이메일 인증 메서드 decode 부분
def activate(request, uidb64=None, token=None):
    uid = urlsafe_base64_decode(uidb64).decode()
    token = urlsafe_base64_decode(token).decode()

    ...
```

이렇게 바꾸어주니!

{% include image.html path="documentation/gaviscon.jpg" path-detail="documentation/gaviscon.jpg" alt="Gaviscon" %}
