---
layout: post
comments: true
title: "장고 인스타그램 프로젝트"
categories: Django
---

Instagram을 구현해보는 프로젝트를 진행하며 알게 된 부분을 정리해보았다.

---

### PostgreSQL

RDMBS의 한 종류이며 주로 django web 개발을 진행할 때 사용된다.

sqlite3에서 postgresql로 변경하는 방법

-   psycopg2 라이브러리 설치

```
pip install psycopg2
```

-   settings.py 안에 설정되어 있는 DATABASES 변경

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': '<database_name>',
        'HOST': 'localhost',
        'PORT': '5432',
        'USER': '<username>'
        'PASSWORD': '<password>'
    }
}
```

-   프로젝트 도중 바꾸게 될 시 application 데이터를 `dump.json`에 가져온 후,

```python
>>> ./manage.py dumpdata <app_name> <app_name2> <app_name3> --indent=4 > dump.json
```

-   데이터베이스 변경 후 다시 가져온다.

```python
>>> ./manage.py loaddata dump.json
```

>   이 후 migrate 만 실행시켜주면 잘 동작한다

### 공개 저장소에 올릴 때 주의 사항

위 상태로 공개 저장소에 올리게 되면 SECRET_KEY 또는 데이터베이스의 비밀번호 등 민감한 정보가 올라갈 수 있다. SECRET_KEY를 분리하는 방법에는 여러가지 있겠지만 이 프로젝트를 진행하며 쓴 방법을 정리해보았다.

*   프로젝트 폴더 내에 .config_secret 폴더 생성
*   폴더 내에 settings_common.json 파일을 새로 생성 후 SECRET_KEY와 데이터베이스 정보 작성 

```json
{
  "django": {
    "secret_key": "<secret_key>",
    "databases": {
      "default": {
        "ENGINE": "django.db.backends.postgresql",
        "HOST": "localhost",
        "PORT": "5432",
        "NAME": "<database_name>",
        "USER": "<username>",
        "PASSWORD": "<password>"
      }
    }
  }
}
```

-   settings.py 에서.config_secret의 폴더 경로 추가
-   폴더 안에 있는 파일 내용 읽어오기
-   json.loads 함수 호출로 python dict 형태로 변환하기
-   할당 된 변수를 가지고 SECRET_KEY와 DATABASES 정보 가져오기

```python
CONFIG_SECRET_DIR = os.path.join(os.path.dirname(BASE_DIR), '.config_secret')

f = open(os.path.join(CONFIG_SECRET_DIR, 'settings_common.json'))
config_secret_common_str = f.read()
f.close()

config_secret_common = json.loads(config_secret_common_str)

SECRET_KEY = config_secret_common['django']['secret_key']
DATABASES = config_secret_common['django']['databases']
```

>   .gitignore에 .config_secret/ 폴더만 추가 하면 성공적으로 분리 끝!
>
>   README 작성 시, .config_secret/settings_common.json 경로에 SECRET_KEY를 저장했다는 내용만 보여주면 누구든지 가져다 쓸 수 있다.

### Pillow

- 파이썬 image 처리 라이브러리

```
>>> pip install pillow
```

### MEDIA_URL이 MEDIA_ROOT의 파일을 리턴하도록 하는 URL

-   urls.py에 아래 내용 추가

```python
urlpatterns += static(
    settings.MEDIA_URL,
    document_root=settings.MEDIA_ROOT,
)
```

### URL Mapping

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

### Form

Form을 사용해 image upload 기능 구현하기

* 'Post' application 안에 forms.py 생성

```python
from django import forms

class PostForm(forms.Form):
    title = forms.CharField()
    image = forms.ImageField()
```

*   views.py

```python
from post.forms import PostForm

def image_upload(request):
    if request.method == 'POST':
        form = PostForm(request.POST, request.FILES)
        if form.is_valid():
            title = form.cleaned_data['title']
            image = form.cleaned_data['image']
            Post.objects.create(
            	'title'=title,
              	'image'=image,
            )
        return redirect('<url_name>')
    else:
        form = PostForm()
        context = {
            'form': form,
        }
    return render(request, '<template_path>', context)
```

-   _template_name_.html

```html
<form action="" method="post" enctype="multipart/form-data">
  	{% raw %}
    {% csrf_token %}
  	{% endraw %}
    {{ form }}
    <button type="submit">업로드</button>
</form>
```

-   urls.py

```python
from django.conf.urls import url

from post.views import image_upload

urlpattern = [
    url(r'^posts/upload/$', image_upload, name='<url_name>'
]
```

