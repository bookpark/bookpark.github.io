---
layout: post
title:  "Model relationships / Making queries"
categories: django
---

## Model inheritance

### Multiple inheritance

**mix-in**

* class에 속성이나 메서드를 추가


### Field name "hiding" is not permitted

* 보통 파이썬 클래스에서는 child class가 parent class 속성을 변경할 수 있음
* 장고에서는 불가능
* `abstract model`에서는 가능
* `field_name = None`을 통해 제거할수도 있음

* **_django documentation중 읽어야 할 reference_**
  * Model field reference
  * Field attribute reference
  * QuerySet API reference
  * Lookup API reference

## Making queries

### Creating objects

```python
>>> b = Blog(name='Beatles Blog', tagline='All the latest Beatles news.')
>>> b.save()
```

### Save changes to objects

```python
>>> b5.name = 'New name'
>>> b5.save()
```

### Saving ForeignKey and ManyToManyField fields

* Updating a ForeignKey field works exactly the same way as saving a normal field – simply assign an object of the right type to the field in question.

```python
>>> e = Entry.objects.create(blog=b5)
```

* Updating a ManyToManyField works a little differently – use the add() method on the field to add a record to the relation. This example adds the Author instance joe to the entry object:
* save() 호출 필요 없음

```python
>>> joe = Author.objects.create(name="Joe")
>>> entry.authors.add(joe)
```

* To add multiple records to a ManyToManyField in one go, include multiple arguments in the call to add(), like this:

```python
>>> john = Author.objects.create(name="John")
>>> paul = Author.objects.create(name="Paul")
>>> george = Author.objects.create(name="George")
>>> ringo = Author.objects.create(name="Ringo")
>>> entry.authors.add(john, paul, george, ringo)
```

### Retrieving objects

```python
>>> Blog.objects
<django.db.models.manager.Manager object at ...>
>>> b = Blog(name='Foo', tagline='Bar')
>>> b.objects
Traceback:
    ...
AttributeError: "Manager isn't accessible via Blog instances."
```

### Retrieving all objects

```python
>>> all_entries = Entry.objects.all()
```

To create such a subset, you refine the initial QuerySet, adding filter conditions. The two most common ways to refine a QuerySet are:

* filter(\*\*kwargs)
  * Returns a new QuerySet containing objects that match the given lookup parameters.

* exclude(**kwargs)
  * Returns a new QuerySet containing objects that do not match the given lookup parameters.

### Retrieving specific objects with filters

For example, to get a QuerySet of blog entries from the year 2006, use filter() like so:

```python
>>> Entry.objects.filter(pub_date__year=2006)
```

With the default manager class, it is the same as:

```python
>>> Entry.objects.all().filter(pub_date__year=2006)
```

**Chaining filters**

* The result of refining a QuerySet is itself a QuerySet, so it’s possible to chain refinements together. For example:

```python
>>> From datetime import datetime
>>> Entry.objects.filter(headline__icontains='First').exclude(pub_date__gte=datetime(2017, 5, 1))
```

**Filtered QuerySets are unique**

* Each time you refine a QuerySet, you get a brand-new QuerySet that is in no way bound to the previous QuerySet. Each refinement creates a separate and distinct QuerySet that can be stored, used and reused.

```python
>>> q1 = Entry.objects.all()
>>> q2 = q1.filter(headline__contains='first')
>>> q3 = q2.filter(pub_date__year=2006)
>>> q1.__str__()
```

> q1.\_\_str__()은 q1을 호출하는 것과 같다

**Querysets are lazy**

* the act of creating a QuerySet doesn’t involve any database activity. You can stack filters together all day long, and Django won’t actually run the query until the QuerySet is evaluated. Take a look at this example:

```python
>>> q = Entry.objects.filter(headline__startswith="What")
>>> q = q.filter(pub_date__lte=datetime.date.today())
>>> q = q.exclude(body_text__icontains="food")
>>> print(q)
```

**Field lookups**

```python
>>> Entry.objects.filter(pub_date__lte='2006-01-01')
```

SQL에선:

```sql
SELECT * FROM blog_entry WHERE pub_date <= '2006-01-01';
```

**Lookups that span relationships**

* Django offers a powerful and intuitive way to “follow” relationships in lookups, taking care of the SQL JOINs for you automatically, behind the scenes. To span a relationship, just use the field name of related fields across models, separated by double underscores, until you get to the field you want.

예)

```python
>>> Blog.objects.filter(entry__headline__contains='Lennon')
```

> entry의 headline이 'Lennon'을 포함하는 Blog의 쿼리셋을 리턴

* If you are filtering across multiple relationships and one of the intermediate models doesn’t have a value that meets the filter condition, Django will treat it as if there is an empty (all values are NULL), but valid, object there. All this means is that no error will be raised. For example, in this filter:


**Spanning multi-valued relationships**

*  To select all blogs that contain entries with both “Lennon” in the headline and that were published in 2008 (the same entry satisfying both conditions), we would write:

_동시에 두가지 조건 다 만족하는 Blog 반환_

```python
Blog.objects.filter(entry__headline__contains='Lennon', entry__pub_date__year=2008)
```

To select all blogs that contain an entry with “Lennon” in the headline as well as an entry that was published in 2008, we would write:

* 필터를 하고 쿼리셋 리턴, 다시 필터를 하고 쿼리셋에 추가
* 필터 할 때마다 추가되는 값이 있으면 추가
* 중복값이 생길수밖에 없음
* 중복값을 제거하고 싶으면 `.distinct()` 추가

```python
Blog.objects.filter(entry__headline__contains='Lennon').filter(entry__pub_date__year=2008)
```

> filter() 와 exclude()는 다르다

```python
Blog.objects.exclude(
	entry__headline__contains='Lennon',
	entry__pub_date__year=2008
)
```

위 예제는 두 개의 조건을 만족시키지 못한다. 다음 예를 써보자.

```python
Blog.objects.exclude(
    entry__in=Entry.objects.filter(
        headline__contains='Lennon',
        pub_date__year=2008,
    ),
)
```

