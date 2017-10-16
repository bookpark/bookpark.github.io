---
layout: post
title:  "Model Relationships"
categories: django
---

### Many-to-one relationships

> 상대가 없어지면 자기 자신도 사라짐 `on_delete=models.CASCADE`

```python
# 만약 아직 정의되지 않은 ForeignKey를 정의하고 싶다면 참조하는 클래스를 문자열로 넣어준다 (문자열이기 때문에 rename 시킬 때는 같이 바뀌지 않는다
class Car(models.Model):
    # 제조사가 사라지면 차종도 사라진다
    manufacturer = models.ForeignKey('Manufacturer', on_delete=models.CASCADE)
    name = models.CharField(max_length=50, blank=True)
    
class Manufacturer(models.Model):
    name = models.CharField(max_length=50)
```

**Recursive Relationships (재귀 관계)**

* `자기 자신`과 `다대일 관계` (many-to-one relationship)을 갖는 것을 `재귀 관계`라 부른다

> 상대가 없어져도 자기 자신은 살아있음 `on_delete=models.SET_NULL`

```python
# 동등한 유저일 때 관계를 가지는 경우
class User(models.Model):
    name = models.CharField(max_length=30)
    # 배우자가 없어져도 자신은 없어지지 않는다 (on_delete=models.SET_NULL)
    spouse = models.ForeignKey('self', on_delete=models.SET_NULL, null=True, blank=True)
```

**The possible values for `on_delete` are found in django.db.models:**

* `CASCADE`  
  Cascade deletes. Django emulates the behavior of the SQL constraint ON DELETE CASCADE and also deletes the object containing the ForeignKey.

* `PROTECT`  
  Prevent deletion of the referenced object by raising ProtectedError, a subclass of django.db.IntegrityError.

* `SET_NULL`  
  Set the ForeignKey null; this is only possible if null is True.

* `SET_DEFAULT`  
  Set the ForeignKey to its default value; a default for the ForeignKey must be set.

* `SET()`  
  Set the ForeignKey to the value passed to SET(), or if a callable is passed in, the result of calling it. In most cases, passing a callable will be necessary to avoid executing queries at the time your models.py is imported:



### Many-to-many Symmetrical

* 보통 many-to-many는 대칭관계를 갖는다 (내가 너의 친구라면 너도 나의 친구)
* `symmetrical=False` 값을 줬을 때는 대칭이 아닌 관계가 됨

```python
class InstagramUser(models.Model):
    name = models.CharField(max_length=30)
    following = models.ManyToManyField('self', symmetrical=False, blank=True)
```

### Intermediate Model

* 중간자 모델을 사용하여 관계를 갖는다
* `through`를 사용하여 중계자 모델을 선택한다
* `Foreign Key`를 두개 이상 참조해야 할 시 `through_fields`를 사용
* `related_name`을 이용해 역참조 개체 이름 지정

```python
# through_fields 사용
class Group(models.Model):
    name = models.CharField(max_length=30)
    debut_date = models.DateField()
    members = models.ManyToManyField(
        Idol,
        through='Membership',
        # 순서 중요 (source 다음 target)
        # source의 의미 (group의 idol (O), idol의 group (X))
        through_fields=('group', 'idol'),
    )


class Membership(models.Model):
    idol = models.ForeignKey(
        Idol,
        on_delete=models.CASCADE,
        related_name='membership_set',
    )
    group = models.ForeignKey(
        Group,
        on_delete=models.CASCADE,
    )
    recommender = models.ForeignKey(
        Idol,
        null=True,
        on_delete=models.SET_NULL,
        related_name='recommend_membership_set',
    )
    joined_date = models.DateField()
    is_active = models.BooleanField()
```

**with MTM Field**

> `recommender` -> `recommenders`로 MTM 변경

```python
recommenders = models.ManyToManyField(
        Idol,
        blank=True,
        related_name='recommend_membership_set',
    )
```

