### Create a Virtual environment
virtualenv venv

source venv/bin/activate

### install django 
 pip install django

 pip list

 python -m django --version

### create a project 
 django-admin startproject mysite
 ls
 cd mysite/
 
### Run server in different ports 
python manage.py runserver

python manage.py runserver 8080

python manage.py runserver 0.0.0.0:8000

### Create an app
python manage.py startapp polls

 