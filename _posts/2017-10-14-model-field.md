---
layout: post
comments: true
title: "Model Field"
categories: Django
---

## Field options

* null

* blank

* choices

* db_column

  * The name of the database column to use for this field.

* db_index

  * If `True`, a database index will be created for this field.

* db_tablespace

  * The name of the [database tablespace](https://docs.djangoproject.com/en/1.11/topics/db/tablespaces/) to use for this field’s index, if this field is indexed.

* default

  * The default value for the field

* editable

  * Default is `True`
  * If `False`, the field will not be displayed in the admin or any other [`ModelForm`](https://docs.djangoproject.com/en/1.11/topics/forms/modelforms/#django.forms.ModelForm).

* error_messages

  * The `error_messages` argument lets you override the default messages that the field will raise.

* help_text

  * Extra “help” text to be displayed with the form widget.

* primary_key

  * If `True`, this field is the primary key for the model.
  * `primary_key=True` implies [`null=False`](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.Field.null) and [`unique=True`](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.Field.unique).

* unique

  * If `True`, this field must be unique throughout the table.
  * This option is valid on all field types except [`ManyToManyField`](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.ManyToManyField) and [`OneToOneField`](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.OneToOneField).

* unique_for_date

* unique_for_month

* unique_for_year

* verbose_name

  * A human-readable name for the field. If the verbose name isn’t given, Django will automatically create it using the field’s attribute name, converting underscores to spaces. See [Verbose field names](https://docs.djangoproject.com/en/1.11/topics/db/models/#verbose-field-names).

* validators

  * Registering and fetching lookups

    `Field` implements the [lookup registration API](https://docs.djangoproject.com/en/1.11/ref/models/lookups/#lookup-registration-api). The API can be used to customize which lookups are available for a field class, and how lookups are fetched from a field.

## Field types

* AutoField
  * An [`IntegerField`](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.IntegerField) that automatically increments according to available IDs. 
* BigAutoField (New in Django 1.10)
  * A 64-bit integer, much like an [`AutoField`](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.AutoField) except that it is guaranteed to fit numbers from `1` to `9223372036854775807`. 
* BigIntegerField
  * A 64-bit integer, much like an [`IntegerField`](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.IntegerField) except that it is guaranteed to fit numbers from `-9223372036854775808` to`9223372036854775807`. The default form widget for this field is a [`TextInput`](https://docs.djangoproject.com/en/1.11/ref/forms/widgets/#django.forms.TextInput).
* BinaryField
  * A field to store raw binary data. It only supports `bytes` assignment. Be aware that this field has limited functionality. For example, it is not possible to filter a queryset on a `BinaryField` value. It is also not possible to include a `BinaryField` in a [`ModelForm`](https://docs.djangoproject.com/en/1.11/topics/forms/modelforms/#django.forms.ModelForm).
  * 이 필드는 정적 파일 처리를 대체하지 못한다.
* BooleanField
  * If needs `null` values, use **NullBooleanField**.
* CharField
* CommaSeperatedIntegerField
* DateField
  * auto_now
    * Automatically set the field to now every time the object is saved.
    * The field is only automatically updated when calling [`Model.save()`](https://docs.djangoproject.com/en/1.11/ref/models/instances/#django.db.models.Model.save).
  * auto_now_add
    * Automatically set the field to now when the object is first created.
    * So even if you set a value for this field when creating the object, it will be ignored. If you want to be able to modify this field, set the following instead of `auto_now_add=True`:
      * For [`DateField`](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.DateField): `default=date.today` - from [`datetime.date.today()`](https://docs.python.org/3/library/datetime.html#datetime.date.today)
      * For [`DateTimeField`](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.DateTimeField): `default=timezone.now` - from [`django.utils.timezone.now()`](https://docs.djangoproject.com/en/1.11/ref/utils/#django.utils.timezone.now)
* DateTimeField
  * A date and time, represented in Python by a `datetime.datetime` instance.
* DecimalField
  * A fixed-precision decimal number, represented in Python by a [`Decimal`](https://docs.python.org/3/library/decimal.html#decimal.Decimal) instance. Has two **required** arguments.
    * max_digits
    * decimal_places
* DurationalField
  * A field for storing periods of time - modeled in Python by [`timedelta`](https://docs.python.org/3/library/datetime.html#datetime.timedelta).
* EmailField
* FieldField
  * FileField.upload_to
  * FileField.Storage
    1. In your settings file, you’ll need to define [`MEDIA_ROOT`](https://docs.djangoproject.com/en/1.11/ref/settings/#std:setting-MEDIA_ROOT) as the full path to a directory where you’d like Django to store uploaded files. (For performance, these files are not stored in the database.) Define [`MEDIA_URL`](https://docs.djangoproject.com/en/1.11/ref/settings/#std:setting-MEDIA_URL) as the base public URL of that directory. Make sure that this directory is writable by the Web server’s user account.
    2. Add the [`FileField`](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.FileField) or [`ImageField`](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.ImageField) to your model, defining the [`upload_to`](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.FileField.upload_to) option to specify a subdirectory of[`MEDIA_ROOT`](https://docs.djangoproject.com/en/1.11/ref/settings/#std:setting-MEDIA_ROOT) to use for uploaded files.
* When you access a [`FileField`](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.FileField) on a model, you are given an instance of [`FieldFile`](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.fields.files.FieldFile) as a proxy for accessing the underlying file.
  * FieldFile.name
  * FieldFile.size
  * FieldFile.url
  * FieldFile.open
  * FieldFile.close
  * FieldFile.save
  * FieldFile.delete
* FilePathField
  * path
  * match
  * resursive
  * allow_files
  * allow_folders
* FloatField
  * A floating-point number represented in Python by a `float` instance.
  * `FloatField` vs. `DecimalField`
    * The [`FloatField`](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.FloatField) class is sometimes mixed up with the [`DecimalField`](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.DecimalField) class. Although they both represent real numbers, they represent those numbers differently. `FloatField` uses Python’s `float` type internally, while `DecimalField` uses Python’s `Decimal` type.
* ImageField
  * height_field
  * width_field
* IntegerField
* GenericIPAddressField
  * protocol
  * unpack_ipv4
* NullBooleanField
* PositiveIntegerField
* SlugField
  * [Slug](https://docs.djangoproject.com/en/1.11/glossary/#term-slug) is a newspaper term. A slug is a short label for something, containing only letters, numbers, underscores or hyphens. They’re generally used in URLs.
* SmallIntegerField
* TextField
  * A large text field. The default form widget for this field is a [`Textarea`](https://docs.djangoproject.com/en/1.11/ref/forms/widgets/#django.forms.Textarea).
* TimeField
* URLField
  * A [`CharField`](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.CharField) for a URL.
* UUIDField
  * A field for storing universally unique identifiers. Uses Python’s [`UUID`](https://docs.python.org/3/library/uuid.html#uuid.UUID) class. When used on PostgreSQL, this stores in a `uuid`datatype, otherwise in a `char(32)`.
  * Universally unique identifiers are a good alternative to [`AutoField`](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.AutoField) for [`primary_key`](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.Field.primary_key). The database will not generate the UUID for you, so it is recommended to use [`default`](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.Field.default):

```python
import uuid
from django.db import models

class MyUUIDModel(models.Model):
    id = models.UUIDField(primary_key=True, default=uuid.uuid4, editable=False)
    # other fields
```

# Field attribute

## Attributes for fields

* auto_created
* concrete
* hidden
* is_relation
* model

## Attributes for fields with relations

* many_to_many
* many_to_one
* one_to_many
* one_to_one
* related_model