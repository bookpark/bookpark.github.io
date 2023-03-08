---
layout: post
comments: false
title: "Pillow를 사용한 썸네일 만들기"
author: "Booki"
tags: "Django"
description: "업로드 된 이미지를 썸네일 크기로 리사이징해 사용해보자"
---

웹에 업로드 되는 이미지의 리사이징 그리고 썸네일 생성을 위해 Pillow 패키지를 활용해보았다. 



## Pillow 활용

### 설치

- `pillow` 를 설치해준다.

```bash
pip install pillow
```



### 기본 활용

- `Pillow` 패키지에는 **Image** 모듈 외에도 **ImageColor**, **ImageDraw** 등등 모듈이 많지만 썸네일 만들기에선 기본적인 **Image** 모듈만 사용해보았다.

```python
from PIL import Image
import os

# 이미지 불러오기
image = Image.open('bo_oki.jpg')

# 썸네일 만들기
size = 128, 128

file, ext = os.path.splitext(infile)
image = Image.open(infile)
image.thumbnail(size)
image.save(file + '_thumbnail', "JPEG")
```



### Django 모델에 적용

- Django 모델에 적용 시 save() 메서드를 오버라이드 한 후 썸네일 함수를 적용시키면 된다.

```python
class Profile(models.Model):
    # 로고 이미지 받을 필드
    logo = models.ImageField(
        upload_to='logo',
    )
    # 썸네일 처리 된 이미지 저장 필드
    thumbnail = models.ImageField(
        upload_to='logo_thumbs',
    )
    
    # save() 메서드 재정의
    def save(self, *args, **kwargs):
        if not self.create_thumbnail():
            raise Exception('Failed creating thumbnails')
        super(Profile, self).save(*args, **kwargs)
    
    # 썸네일 처리
    def create_thumbnail(self):
        size = 128, 128
        file, ext = os.path.splitext(infile)
        image = Image.open(infile)
        image.thumbnail(size)
        ...
        image.save(file + '_thumbnail', "JPEG")
        # thumbnail 필드에 저장
        self.thumbnail.save()
```

