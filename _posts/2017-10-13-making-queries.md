---
layout: post
title:  "Making queries"
categories: django
---

# Making queries

**Filters can reference fields on the model**

- Django provides [`F expressions`](https://docs.djangoproject.com/en/1.11/ref/models/expressions/#django.db.models.F) to allow such comparisons. Instances of `F()` act as a reference to a model field within a query. These references can then be used in query filters to compare the values of two different fields on the same model instance.

```python
>>> from django.db.models import F
>>> Entry.objects.filter(n_comments__gt=F('n_pingbacks'))
```

- For date and date/time fields, you can add or subtract a [`timedelta`](https://docs.python.org/3/library/datetime.html#datetime.timedelta) object. The following would return all entries that were modified more than 3 days after they were published:

```python
>>> from datetime import timedelta
>>> Entry.objects.filter(mod_date__gt=F('pub_date') + timedelta(days=3))
```

### Q objects를 이용한 complex lookups

* For example, this statement yields a single `Q` object that represents the “OR” of two `"question__startswith"` queries:

```python
Q(question__startswith='Who') | Q(question__startswith='What')
```

* This is equivalent to the following SQL `WHERE` clause:

```sql
WHERE question LIKE 'Who%' OR question LIKE 'What%'
```

* If you provide multiple `Q` object arguments to a lookup function, the arguments will be “AND”ed together. For example:

```python
Poll.objects.get(
    Q(question__startswith='Who'),
    Q(pub_date=date(2005, 5, 2)) | Q(pub_date=date(2005, 5, 6))
)
```

* SQL:

```Sql
SELECT * from polls WHERE question LIKE 'Who%'
    AND (pub_date = '2005-05-02' OR pub_date = '2005-05-06')
```

### Model instances 복제

* In the simplest case, you can just set `pk` to `None`. Using our blog example:

```python
blog = Blog(name='My blog', tagline='Blogging is easy')
blog.save() # blog.pk == 1

blog.pk = None
blog.save() # blog.pk == 2
```

* Things get more complicated if you use inheritance. Consider a subclass of `Blog`:

```python
class ThemeBlog(Blog):
    theme = models.CharField(max_length=200)

django_blog = ThemeBlog(name='Django', tagline='Django is easy', theme='python')
django_blog.save() # django_blog.pk == 3
```

* Due to how inheritance works, you have to set both `pk` and `id` to `None`:

```python
django_blog.pk = None
django_blog.id = None
django_blog.save() # django_blog.pk == 4
```

### Updating multiple objects at once

* To update [`ForeignKey`](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.ForeignKey) fields, set the new value to be the new model instance you want to point to. For example:

```python
>>> b = Blog.objects.get(pk=1)

# Change every Entry so that it belongs to this Blog.
>>> Entry.objects.all().update(blog=b)
```

> `update()`는 `save()`를 호출하지 않는다

* Make sure that the [`save()`](https://docs.djangoproject.com/en/1.11/ref/models/instances/#django.db.models.Model.save) method is called on each instance, you don’t need any special function to handle that. Just loop over them and call [`save()`](https://docs.djangoproject.com/en/1.11/ref/models/instances/#django.db.models.Model.save):

```python
for item in my_queryset:
    item.save()
```

### Related objects

For example, a `Blog` object `b` has access to a list of all related `Entry`objects via the `entry_set` attribute: 

```python
b.entry_set.all()
```

**One-to-many relationships**

Forward



Backward



Using a custom reverse manager



Additional methods



**Many-to-many relationships**



**One-to-one relationships**



**Queries over related objects**

* For example, if you have a Blog object `b` with `id=5`, the following three queries would be identical:

```python
Entry.objects.filter(blog=b) # Query using object instance
Entry.objects.filter(blog=b.id) # Query using id from instance
Entry.objects.filter(blog=5) # Query using id directly
```

