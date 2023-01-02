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


 