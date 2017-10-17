---
layout: post
comments: true
title: "인스타그램 프로젝트"
categories: Django
---

## 새롭게 배운 내용들

**Pillow**

- 파이썬 image 처리 라이브러리

**MEDIA_URL이 MEDIA_ROOT의 파일을 리턴하도록 하는 URL**

```python
urlpatterns += static(
    settings.MEDIA_URL,
    document_root=settings.MEDIA_ROOT,
)
```

**URL Mapping**

한번만 사용 될 admin이나 MEDIA 설정 url은 그대로 유지하고 앱 단위로 url 관리

- 기존 프로젝트 urls.py

```python
from django.conf import settings
from django.conf.urls import url, include
from django.conf.urls.static import static
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^posts/$', post_list, name='post_list'),
	url(r'^posts/upload/$', post_upload, name='post_upload'),
]
urlpatterns += static(
    settings.MEDIA_URL,
    document_root=settings.MEDIA_ROOT,
)
```

- 애플리케이션 안에 새로운 urls.py 생성 후 해당 애플리케이션에서 사용하는 url 이동

```python
from django.conf.urls import url

from post.views import post_list, post_upload

urlpatterns = [
    url(r'^posts/$', post_list, name='post_list'),
    url(r'^posts/upload/$', post_upload, name='post_upload'),
]
```

- 기존 프로젝트 urls.py에서 앱에 사용되는 url들을 inlcude로 가져옴

```python
from django.conf import settings
from django.conf.urls import url, include
from django.conf.urls.static import static
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^', include('post.urls')),
]
urlpatterns += static(
    settings.MEDIA_URL,
    document_root=settings.MEDIA_ROOT,
)
```

**Form**