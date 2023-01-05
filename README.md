### Create a Virtual environment

```
virtualenv venv
source venv/bin/activate
```

### install django

```
 pip install django
 pip list
 python -m django --version
```

### create a project

```
 django-admin startproject mysite
 ls
 cd mysite/
```

### Run server in different ports

```
python manage.py runserver
python manage.py runserver 8080
python manage.py runserver 0.0.0.0:8000
```

### Create an app

```
python manage.py startapp polls
```

when we create an app we also want to custom url for that to do so
update the `urls.py` file of that app so create file like `polls/urls.py`.
also add that urls file to the urls file of the project like `mystie/urls.py`
to import that file use `include()` function inside the function pass the url path
`polls.urls`

### View

For basic http response from view use `HttpResponse`

### Model

In Django, a model is a class that represents a table in your database. Each model maps to a single database table, and
the fields of the model correspond to the columns of the table. For example, if you had a model called Person, it might
have fields like first_name, last_name, and age.

Models are used to interact with the database in Django. You can use them to create, retrieve, update, and delete
records in the database. They are an important part of Django's ORM (Object-Relational Mapper), which is responsible for
translating between the database and the Django models in your application.

Here is an example of a simple model in Django:

```python
from django.db import models


class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)
    age = models.IntegerField()

```

This model defines a Person class with three fields: first_name, last_name, and age. These fields are represented as
instances of the CharField and IntegerField classes, which are provided by Django's ORM.

To use this model, you would need to create a database table that matches the fields of the model. Django provides tools
to automate this process, so you don't have to write raw SQL statements.

To activate model we need to update settings of the project, we need to add the app in `INSTALLED_APPS` section

```python
INSTALLED_APPS = [
    'polls.apps.PollsConfig',
    # ...
    'django.contrib.staticfiles',
]

```

### Migration

`$ python manage.py makemigrations polls`

`$ python manage.py migrate`

### Check what makemigrations does

`$python manage.py sqlmigrate polls 0001`

### __str()__

In Python, the __str__ method is used to define how an object should be represented as a string. This method is called
when you use the str() function to convert an object to a string, or when you use the print() function to print an
object.

In Django, it is common to define the __str__ method on your models to return a human-readable representation of the
object. This can be useful when working with Django's admin site, or when you want to display a list of objects in your
templates.

Here is an example of a simple model that defines the __str__ method:

```python
from django.db import models


class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)
    age = models.IntegerField()

    def __str__(self):
        return f'{self.first_name} {self.last_name} ({self.age})'

```

In this example, the __str__ method returns a string in the format 'first_name last_name (age)'. This is the string that
will be used when you call str() on an instance of the Person model, or when you print an instance of the model.

For example:

```text
>>> p = Person(first_name='John', last_name='Doe', age=30)
>>> str(p)
'John Doe (30)'
>>> print(p)
John Doe (30)

```

### To make the poll app modifiable in the admin

update `polls/admin.py`

```python
from django.contrib import admin
from .models import Question

admin.site.register(Question)
```