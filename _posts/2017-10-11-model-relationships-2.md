---
layout: post
title:  "Model relationships 2"
categories: django

---

# Models

## Relationships

### One-to-one relationships

* This is most useful on the primary key of an object when that object “extends” another object in some way.

* Place <-> Restaurant <- Waiter 
  * Place와 Restaurant은 `One-to-one`, Waiter는 Restaurant을 `Foreign Key`로 사용

```python
class Place(models.Model):
    name = models.CharField(max_length=50)
    address = models.CharField(max_length=80)

    def __str__(self):
        return f'{self.name} the place'


# 혼자 존재할 이유가 없기 때문에 Primary Key를 설정
class Restaurant(models.Model):
    place = models.OneToOneField(
        Place,
        on_delete=models.CASCADE,
        primary_key=True
    )
    serves_hot_dogs = models.BooleanField(default=False)
    serves_pizza = models.BooleanField(default=False)

    def __str__(self):
        return f'{self.place.name} the restaurant'


class Waiter(models.Model):
    restaurant = models.ForeignKey(
        Restaurant,
        on_delete=models.CASCADE,
    )
    name = models.CharField(max_length=50)

    def __str__(self):
        return f'{self.name} the waiter at {self.restaurant}'
```

> creating a Restaurant with unsaved Place raises ValueError

```python
>>> p3 = Place(name='Demon Dogs', address='944 W. Fullerton')
>>> Restaurant.objects.create(place=p3, serves_hot_dogs=True, serves_pizza=False)
Traceback (most recent call last):
...
ValueError: save() prohibited to prevent data loss due to unsaved related object 'place'.
```

## Model methods

* Define custom methods on a model to add custom “row-level” functionality to your objects. Whereas Manager methods are intended to do “table-wide” things, model methods should act on a particular model instance.

* This is a valuable technique for keeping business logic in one place – the model.



> 예)

```python
class Person(models.Model):
    first_name = models.CharField(max_length=50)
    last_name = models.CharField(max_length=50)
    birth_date = models.DateField()

    def baby_boomer_status(self):
        "Returns the person's baby-boomer status."
        import datetime
        if self.birth_date < datetime.date(1945, 8, 1):
            return "Pre-boomer"
        elif self.birth_date < datetime.date(1965, 1, 1):
            return "Baby boomer"
        else:
            return "Post-boomer"

    @property
    def full_name(self):
        "Returns the person's full name."
        return '%s %s' % (self.first_name, self.last_name)
```

**Overriding predefined model methods**

* There’s another set of model methods that encapsulate a bunch of database behavior that you’ll want to customize. In particular you’ll often want to change the way save() and delete() work.

```python
class User(models.Model):
    name = models.CharField(max_length=30)
    spouse = models.ForeignKey('self', on_delete=models.SET_NULL, null=True, blank=True)

    def __str__(self):
        return self.name

    def save(self, *args, **kwargs):
        if self.spouse and self.spouse.pk == self.pk:
            self.spouse = None
        super(User, self).save(*args, **kwargs)
```

## Model inheritance

* 세 가지 유형의 모델 상속
  * Abstract base classes (가장 많이 쓰는 형태)
  * Multi-table inheritance (추천하지 않음)
  * Proxy models

### Abstract base classes

* You write your base class and put `abstract=True` in the `Meta` class.
  * `abstract=True` 옵션을 주면 `Abstract base class`가 된다.

```python
class CommonInfo(models.Model):
    name = models.CharField(max_length=100)
    age = models.PositiveIntegerField()

    class Meta:
        abstract = True


class Student(CommonInfo):
    home_group = models.CharField(max_length=5)


class Teacher(CommonInfo):
    subject = models.CharField(max_length=30)
```

* Meta inheritance
  * Meta를 상속 받아와 필요한 부분만 추가하거나 변경할 수 있다.

```python
class CommonInfo(models.Model):
    # ...
    class Meta:
        abstract = True
        ordering = ['name']

class Student(CommonInfo):
    # ...
    class Meta(CommonInfo.Meta):
```

**Be careful with related\_name and related\_query_name**

```python
class Base(models.Model):
    m2m = models.ManyToManyField(
        OtherModel,
        related_name="%(app_label)s_%(class)s_related",
        related_query_name="%(app_label)s_%(class)ss",
    )

    class Meta:
        abstract = True

class ChildA(Base):
    pass

class ChildB(Base):
    pass
```

### Multi-table inheritance

* Each model corresponds to its own database table and can be queried and created individually.

```python
class Place(models.Model):
    name = models.CharField(max_length=50)
    address = models.CharField(max_length=80)

    def __str__(self):
        return self.name


class Restaurant(Place):
    serves_hot_dogs = models.BooleanField(default=False)
    serves_pizza = models.BooleanField(default=False)

    def __str__(self):
        return f'{self.name} Restaurant'
```

* The inheritance relationship introduces links between the child model and each of its parents (via an automatically-created OneToOneField).

```python
place_ptr = models.OneToOneField(
    Place, on_delete=models.CASCADE,
    parent_link=True,
)
```

**Inheritance and reverse relations**

* Because multi-table inheritance uses an implicit OneToOneField to link the child and the parent, it’s possible to move from the parent down to the child, as in the above example. However, this uses up the name that is the default related\_name value for ForeignKey and ManyToManyField relations. If you are putting those types of relations on a subclass of the parent model, you must specify the `related_name` attribute on each such field. If you forget, Django will raise a validation error.

**Specifying the parent link field**

* As mentioned, Django will automatically create a OneToOneField linking your child class back to any non-abstract parent models. If you want to control the name of the attribute linking back to the parent, you can create your own OneToOneField and set `parent_link=True` to indicate that your field is the link back to the parent class.

### Proxy models

* Sometimes, you only want to change the Python behavior of a model – perhaps to change the default manager, or add a new method. (동작 구분)

* You can create, delete and update instances of the proxy model and all the data will be saved as if you were using the original (non-proxied) model.

* The difference is that you can change things like the default model ordering or the default manager in the proxy, without having to alter the original.

> 핵심: 원본을 바꾸지 않고 default model ordering 또는 default manager를 바꿀 수 있음

```python
class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)

class MyPerson(Person):
    class Meta:
        proxy = True

    def do_something(self):
        # ...
        pass
```

> `proxy = True` 일 경우에는 테이블이 추가적으로 생기지 않는다.

예)

```python
class Champion(models.Model):
    CHOICES_TYPE = (
        ('magician', '마법사'),
        ('supporter', '서포터'),
        ('ad', '원거리 딜러'),
    )
    champion_type = models.CharField(max_length=20, choices=CHOICES_TYPE)
    rank = models.PositiveIntegerField(default=0)
    name = models.CharField(max_length=30)

    class Meta:
        ordering = ['rank']

    def __str__(self):
        return f'{self.name} ({self.get_champion_type_display()})'


# 커스텀 쿼리셋 매니저
class SupporterManager(models.Manager):
    def get_queryset(self):
        return super().get_queryset().filter(champion_type='supporter')


class Supporter(Champion):
    objects = SupporterManager()

    class Meta:
        ordering = ['name']
        proxy = True

    def buy_supporter_item(self):
        print(f'{self.name}은/는 서포터 아이템을 샀다')


class Midliner(Champion):
    class Meta:
        proxy = True

    def go_to_mid(self):
        print(f'{self.name}은/는 미드에 도착했다')
```

