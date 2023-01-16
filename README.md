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

### Create Superuser

`python manage.py createsuperuser`

### DoesNotExist exception

In Django, DoesNotExist is an exception that is raised by a model's objects manager when you try to retrieve an object
from the database and it doesn't exist.

For example, suppose you have a model called Person with a field called name, and you want to retrieve a person with a
specific name. You can use the get() method of the model's objects manager to do this. However, if no person with the
specified name exists in the database, the get() method will raise a Person.DoesNotExist exception.

Here is an example of how you might use the get() method and handle the DoesNotExist exception:

```python
from django.shortcuts import render
from .models import Person


def view_person(request, name):
    try:
        person = Person.objects.get(name=name)
    except Person.DoesNotExist:
        # Handle the case where the person does not exist
        pass
    else:
        # Render the person's profile
        return render(request, 'person.html', {'person': person})
```

In this example, the view_person function tries to retrieve a Person object with the specified name from the database.
If no such person exists, the DoesNotExist exception is caught and the function does nothing. If the person does exist,
their profile is rendered using the render() function.

You can also use the get_or_create() method of the objects manager to retrieve an object from the database or create it
if it doesn't exist. This can be a convenient way to avoid having to write exception handling code to deal with the
DoesNotExist exception.

### how modelA.modelB_set.all works ?

In Django, the ModelA.modelB_set.all() syntax is used to retrieve all the ModelB objects that are related to a given
ModelA object. This is called a reverse relationship, because it is the opposite of the ModelB objects' forward
relationship with ModelA.

For example, suppose you have two models called Author and Book, where an Author has many Books. You could define the
models like this:

```python
class Author(models.Model):
    name = models.CharField(max_length=50)


class Book(models.Model):
    author = models.ForeignKey(Author, on_delete=models.CASCADE)
    title = models.CharField(max_length=100)

```

In this example, the Book model has a foreign key field called author that is used to relate each Book object to an
Author object. This creates a forward relationship from Book to Author.

To retrieve all the Book objects that are related to a given Author object, you can use the Author.book_set.all()
syntax. For example:

```python
author = Author.objects.get(name='John Smith')
books = author.book_set.all()
```

This will retrieve all the Book objects that have author set to the Author object with the name 'John Smith', and store
them in the books queryset.

The ModelA.modelB_set.all() syntax is provided by Django's ORM as a convenient way to access the related objects in a
reverse relationship. It is equivalent to using the ModelB.objects.filter(modelA_field=modelA_object) syntax to filter
the ModelB objects by the field that defines the forward relationship.

- question.choice_set.all is interpreted as the Python code question.choice_set.all()

### what is {% url %} template tag

In Django templates, the `{% url %}` template tag is used to generate a URL for a named URL pattern. A named URL pattern
is a URL pattern that is given a name, using the name argument of the path() function in Django's urls module.

Here is an example of how you might define a named URL pattern in your Django project:

```python
# myapp/urls.py
from django.urls import path

from . import views

urlpatterns = [
    path('articles/', views.article_list, name='article_list'),
    path('articles/<int:pk>/', views.article_detail, name='article_detail'),
]
```

In this example, the URL patterns are given the names 'article_list' and 'article_detail'. You can then use the {% url
%} template tag in your templates to generate the URLs for these patterns.

For example:

```html
< !-- template.html -->
{ % url 'article_list' %}
{ % url 'article_detail' pk = article.pk %}
```

The first {% url %} tag will generate the URL for the 'article_list' pattern, which is '/articles/'. The second {% url
%} tag will generate the URL for the 'article_detail' pattern, passing the value of article.pk as the pk parameter.

Using named URL patterns and the {% url %} template tag can make it easier to manage your URLs in a Django project, as
you don't have to hardcode the URLs in your templates. If you need to change the URL patterns in your project, you only
have to update the urlpatterns list in your urls module, and the corresponding URLs in your templates will be updated
automatically.

### what is {% csrf_token %}

In Django, the {% csrf_token %} template tag is used to protect your website against Cross-Site Request Forgery (CSRF)
attacks. CSRF attacks are a type of malicious exploit that target a website by tricking a user's browser into making
unintended requests to the website.

The {% csrf_token %} template tag is used in Django templates to generate a CSRF token that is included in the HTML of a
rendered template. The token is sent back to the server when the user submits a form, and the server checks the token to
verify that the form submission is legitimate.

To use the {% csrf_token %} template tag, you need to have 'django.middleware.csrf.CsrfViewMiddleware' in your
MIDDLEWARE setting and the csrf_token template tag in your template.

Here is an example of how you might use the {% csrf_token %} template tag in a Django template:

```html

<form method="post">
    {% csrf_token %}
    <!-- form fields go here -->
    <button type="submit">Save</button>
</form>
```

When this template is rendered, the {% csrf_token %} template tag will be replaced with an HTML input element containing
the CSRF token. When the form is submitted, the CSRF token will be sent back to the server and checked to make sure the
submission is legitimate.

Using the {% csrf_token %} template tag is an important security measure to protect your Django website from CSRF
attacks. It is recommended to use it whenever you have a form that accepts POST, PUT, or DELETE requests in your Django
templates.

### what is pluralize

In Django templates, the pluralize template filter is used to pluralize a word depending on the value of a variable. It
is a built-in template filter that is part of Django's template language.

The pluralize filter takes two arguments: the word to be pluralized and the number that determines whether the word
should be pluralized. If the number is greater than 1, the pluralize filter will return the plural form of the word. If
the number is 0 or 1, it will return the singular form of the word.

Here is an example of how you might use the pluralize filter in a Django template:

```html
{% for object in object_list %}
{{ object.name }}
{% endfor %}

{% if object_list|length > 1 %}
<p>There are {{ object_list|length }} {{ object_list.model|lower }}s.</p>
{% else %}
<p>There is {{ object_list|length }} {{ object_list.model|lower|pluralize }}.</p>
{% endif %}

```

In this example, the pluralize filter is used to pluralize the word object_list.model depending on the number of objects
in the object_list. If there is more than one object, the pluralize filter will return the plural form of the word (
e.g. 'books' if object_list.model is 'book'). If there is only one object, it will return the singular form of the
word (e.g. 'book').

The pluralize filter is a convenient way to handle pluralization in Django templates. It is especially useful when you
are working with languages that have complex rules for pluralization.

### Generic view

For common uses like details view and list view we can use generic view

### Test

For run test case use command like this
`python manage.py test polls` where polls is app name

### Admin page Customization

We can customize admin page by customizing `app/admin.py` file like `polls/admin.py`
We can register multiple model to the admin page
We can add multiple value at a same time using `class ChoiceInline(admin.TabularInline):`
We can update the admin page for list display use`list_display` for filtering `list_filter` for search `search_fields`
