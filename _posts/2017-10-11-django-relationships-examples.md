---
layout: post
title:  "Model relationships 예제"
categories: django
---

```python
from django.db import models


# Choices (2 tuples)
class Person(models.Model):
    SHIRT_SIZES = (
        ('S', 'Small'),
        ('M', 'Medium'),
        ('L', 'Large'),
    )
    name = models.CharField(max_length=30)
    shirt_sizes = models.CharField(
        max_length=1,
        choices=SHIRT_SIZES,
    )


# Primary Key
class Fruit(models.Model):
    name = models.CharField(
        max_length=30,
        primary_key=True,
    )


# Many-to-one (Foreign Key)
class Manufacturer(models.Model):
    name = models.CharField(max_length=30)


class Car(models.Model):
    manufacturer = models.ForeignKey(
        Manufacturer,
        on_delete=models.CASCADE,
    )
    name = models.CharField(max_length=30)


# Many-to-one recursive
class User(models.Model):
    name = models.CharField(max_length=30)
    spouse = models.ForeignKey(
        name,
        on_delete=models.SET_NULL,
        null=True,
        blank=True,
    )


# Many-to-many basic
class Topping(models.Model):
    name = models.CharField(max_length=30)


class Pizza(models.Model):
    toppings = models.ManyToManyField(
        Topping,
        blank=True,
    )


# Many-to-many recursive
class CyworldUser(models.Model):
    name = models.CharField(max_length=30)
    friends = models.ManyToManyField(
        'self',
        blank=True,
    )


# Many-to-many resursive symmetrical
class InstagramUser(models.Model):
    name = models.CharField(max_length=30)
    following = models.ManyToManyField(
        'self',
        symmetrical=False,
        blank=True,
    )


# Many-to-many intermediate
class Idol(models.Model):
    name = models.CharField(max_length=30)


class Group(models.Model):
    name = models.CharField(max_length=30)
    debut_date = models.DateField()
    members = models.ManyToManyField(
        Idol,
        through='Membership',
        through_fields=('group', 'idol'),
    )


class Membership(models.Model):
    idol = models.ForeignKey(
        Idol,
        on_delete=models.CASCADE,
        related_name='membership_set'
    )
    group = models.ForeignKey(
        Group,
        on_delete=models.CASCADE,
    )
    recommender = models.ForeignKey(
        Idol,
        on_delete=models.SET_NULL,
        related_name='recommend_membership_set'
    )
    joined_date = models.DateField()
    is_active = models.BooleanField()


# One-to-one (+ Foreign Key)
class Place(models.Model):
    name = models.CharField(max_length=30)
    address = models.CharField(max_length=50)


class Restaurant(models.Model):
    restaurant = models.OneToOneField(
        Place,
        on_delete=models.CASCADE,
        primary_key=True,
    )
    serves_pasta = models.BooleanField(default=False)
    serves_pizza = models.BooleanField(default=False)


class Waiter(models.Model):
    name = models.CharField(max_length=30)
    waiter = models.ForeignKey(
        Restaurant,
        on_delete=models.CASCADE,
    )


# Inheritance
# Abstract base class (related_name 쓸 때 주의)
class School(models.Model):
    name = models.CharField(max_length=30)


class CommonInfo(models.Model):
    school = models.ForeignKey(
        School,
        blank=True,
        null=True,
        related_name='%(app_label)s_%(class)s_set'
    )
    name = models.CharField(max_length=30)
    age = models.IntegerField()

    class Meta:
        abstract = True


class Student(CommonInfo):
    home_group = models.CharField(max_length=30)


class Teacher(CommonInfo):
    subject = models.CharField(max_length=30)


# Multi-table
class Place(models.Model):
    name = models.CharField(max_length=30)
    address = models.CharField(max_length=50)


class Restaurant(Place):
    serves_pasta = models.BooleanField(Default=False)
    serves_pizza = models.BooleanField(Default=False)


# Proxy
class Champion(models.Model):
    CHOICES_TYPE = (
        ('magician', '마법사'),
        ('supporter', '서포터'),
        ('ad', '원거리'),
    )
    champion_type = models.CharField(
        max_length=30,
        choices=CHOICES_TYPE,
    )
    rank = models.IntegerField(default=0)
    name = models.CharField(max_length=30)


# 쿼리셋 필터 매니저
class AdManager(models.Manager):
    def get_queryset(self):
        return super().get_queryset().filter(champion_type='ad')


class Ad(Champion):
    objects = AdManager()

    class Meta:
        ordering = ['name']
        proxy = True

    def buy_ad_item(self):
        print(f'{self.name}은/는 AD 아이템을 샀다')


class MidManager(models.Manager):
    def get_queryset(self):
        return super().get_queryset().filter(champion_type='magician')


class Mid(Champion):
    objects = MidManager()
    
    class Meta:
        proxy = True

    def go_to_mid(self):
        print(f'{self.name}은/는 미드에 도착했다')
```