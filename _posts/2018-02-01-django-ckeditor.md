---
layout: post
comments: true
title: "Django에서 WYSIWYG 에디터 사용하기"
description: "Django admin에 WYSIWYG 에디터를 적용시켜 포스팅을 쉽게 해보자"
tags: [web, django]
---

웹 프로젝트를 진행하면서 어드민 페이지에 위지위그(WYSIWYG) 방식의 에디터가 필요로 해 리서치를 하던 중 손쉽게 세팅이 가능하고 django-storages, S3를 지원하는 라이브러리를 발견해 소개하고자 한다.

[위지위그란?](https://ko.wikipedia.org/wiki/%EC%9C%84%EC%A7%80%EC%9C%84%EA%B7%B8)

이번에 사용한 위지위그 방식 리치 텍스트 에디터는 CKSource에서 개발한 CKEditor이고 Django에서 쓸 수 있도록 배포한 [django-ckeditor](https://github.com/django-ckeditor/django-ckeditor) 라이브러리를 사용하였다.



###설치



##### 필수 요소

1. 라이브러리 설치

```bash
pip install django-ckeditor
```

2. INSTALLED_APPS에 `ckeditor` 추가
3. CKEditor가 필요로 하는 미디어 소스를 받아오기 위해 `collectstatic` 명령어 실행

```bash
./manage.py collectstatic
```



##### 파일 업로드 기능을 쓰기위한 필수 요소

1. INSTALLED_APPS에 `ckeditor_uploader` 추가 
2. 업로드 경로 설정 (예)

```python
# settings.py

CKEDITOR_UPLOAD_PATH = "uploads/"
```

3. 파일명이 생성될 때 메서드를 추가해주고자 할 때 (예)

```python
# utils.py

def get_filename(filename):
    return filename.upper()
```

```python
# settings.py

CKEDITOR_FILENAME_GENERATOR = 'utils.get_filename'
```

4. CKEditor URL 추가

```python
# urls.py

urlpatterns = [
    ...
    path('ckeditor/', include('ckeditor_uploader.urls')),
]
```



### 사용



##### 필드

필드는 라이브러리가 제공하는 RichTextField()를 사용하면 된다. 정말 쉽다.

```python
from django.db import models
from ckeditor.fields import RichTextField

class Event(models.Model):
    description = RichTextField()
```

_업로드 기능이 들어간 에디터를 사용하고 싶다면 `RichTextUploadingField()`를 사용하면 된다._



##### 위젯

필드를 사용하지 않고 Form 필드에 위젯을 적용시켜 사용할수도 있다. (예)

```python
# admin.py

from django import forms
from django.contrib import admin
from ckeditor.widgets import CKEditorWidget

from .models import Event

class EventAdminForm(forms.ModelForm):
    description = forms.CharField(widget=CKEditorWidget())
    class Meta:
        model = Event

class EventAdmin(admin.ModelAdmin):
    form = EventAdminForm
    
admin.site.register(Event, EventAdmin)
```



##### S3

아래 줄을 추가해주지 않으면 django-storages를 통한 S3 사용이 불가하다.

```python
# settings.py

AWS_QUERYSTRING_AUTH = False
```

**Query String Authenticaton**에 대해 간단히 설명하자면..

* Amazon S3에는 두 종류의 요청이 있다. (API Calls, URL Calls)
* API Call은 프로그램 된 것에 따라 요청을 보낼 때마다 항상 AWS 자격 인증을 같이 보낸다.
  * 고로 항상 인증된 상태의 요청이 가는 것이다.
* URL Call은 URL로 작동 되기 때문에 어느 브라우저에서나 작동할 수 있게 된다.
  * S3에선 Access Control List (ACL)을 통해 파일 별 퍼미션을 줄 수 있다.
  * Public Access 권한을 주지 않고 파일에 접근할 수 있게 하는 방법이 Query String Auth이다.
    * URL에 자신의 AWSAccessKey를 제공하여 유저들의 접근을 받지만,
    * 요청마다 URL이 바뀌며 누군가 이 URL을 제공받아 접근을 시도하면 막히게 되는 것이다.
* CKEditor에선 Query String Auth가 적용되면 작동이 하지 않는다고 한다..




{% include image.html path="documentation/django-ckeditor.png" path-detail="documentation/django-ckeditor.png" alt="CKEditor" %}

<p style="text-align: center; font-size: 8; font-style: italic;">간단하게 Django Admin에 적용된 모습</p>




### 기타

이 외에도 커스터마이징, `allowedContent` 사용, 특정 파일 업로드 제한 등 기능이 많다. 기능에 대한 상세한 설명은 [django-ckeditor](https://github.com/django-ckeditor/django-ckeditor#id12) 문서에서 확인할 수 있다.