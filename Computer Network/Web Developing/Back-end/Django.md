# Start
```Shell
# Project & Apps
pip install django
django-admin startproject <project_name>
# inside the project folder
python manage.py startapp <app_name> # create apps
python manage.py runserver 8080 # 8000 by default
# Database: how to change models
# 1. change models.py
# 2. after changing models, store the migration to a file
python manage.py makemigrations <app_name | all>
# 3. build/rebuild database tables based on INSTALLED_APPS in settings.py and the migration files
# `migrate` runs only when there are migrations not in django_migrations
python manage.py migrate <app_name | all>
# interactive Python shell within Django project
python manage.py shell
```
WSGI (Web Server Gateway Interface)
ASGI (Asynchronous Server Gateway Interface)
# **Related objects reference**
[https://docs.djangoproject.com/en/4.1/ref/models/relations/](https://docs.djangoproject.com/en/4.1/ref/models/relations/)
```Python
# Create/Delete
q = Question(question_text="What's new?", pub_date=timezone.now())
q.save()
# or
Question.objects.create(question_text="What's new?", pub_date=timezone.now())
q.delete()
q.<field_name>
# Read
# query set
Question.objects.all()
Question.objects.filter(id=1)
Question.objects.filter(question_text__startswith='What')
Question.objects.get(pub_date__year=timezone.now().year)
# query item
Question.objects.get(pk=1) # primary key
Question.objects.get(id=1) 
# q.choice_set: choices holding the primary key of q 
c = q.choice_set.create(choice_text='Not much', votes=0)
q.choice_set.all()
q.choice_set.count()
```
Avoid db racing condition: [https://docs.djangoproject.com/en/4.1/ref/models/expressions/#avoiding-race-conditions-using-f](https://docs.djangoproject.com/en/4.1/ref/models/expressions/#avoiding-race-conditions-using-f)
# **Django Admin**
1. create admin account: `python manage.py createsuperuser`
2. register models in <app_name>/admin.py: `admin.site.register(<Model_Name>)`
3. visit website /admin
# URLconfs

> maps URL patterns to views, `urls.py` and `views.py`
```Python
# project level: ROOT_URLCONF
path('<app_name>/', include('<app_name>.urls')),
# app level
url_pattern = '<int:question_id>/vote/'
path(url_pattern, views.vote, name=<view_name>),
```
  
from polls.models import Question
from django.utils import timezone
q = Question(question_text="What's new?", pub_date=timezone.now())
q.save()
c = q.choice_set.create(choice_text='A', votes=0)
c = q.choice_set.create(choice_text='B', votes=0)
c = q.choice_set.create(choice_text='C', votes=0)