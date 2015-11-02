# celery-demo

source : https://www.caktusgroup.com/blog/2014/06/23/scheduling-tasks-celery/

How to get started with django and celery ? 

# create virtualenv and activate it.
# install the following stuff,
```
pip install django
pip install django-celery
```

# create django project 
# add following apps to INSTALLED_APPS
```
'djcelery',
'kombu.transport.django',
```
   
   <b>djcelery is always needed. kombu.transport.django is the Django-based broker, for use mainly during development.</b>
   
# add followoing code to settings.py
```
import djcelery
djcelery.setup_loader()
BROKER_URL = 'django://'
```
  <b>The first two lines are always needed. Line 3 configures Celery to use its Django broker.
  Important: Never use the Django broker in production. We are only using it here to save time in this tutorial.     In production you'll want to use RabbitMQ, or maybe Redis.</b>
  
  and also add this line to settings.py, 
  
```
CELERY_IMPORTS=("demo.tasks")
```
  
# now create tables,

```
python manage.py migrate
```
  
# create new file in demo folder as ```task.py``` and add following code to it,

```
from celery import task
@task()
  def add(x, y):
  return x + y
```
    
# open another terminal and execute following line (*) ,

```
python manage.py celery worker --loglevel=info
```

 **keep it running.**
    
# open django shell and try the following code,
```
(env1)➜  demo  ./manage.py shell   
Python 2.7.6 (default, Jun 22 2015, 17:58:13) 
[GCC 4.8.2] on linux2
Type "help", "copyright", "credits" or "license" for more information.
(InteractiveConsole)
>>> from demo.tasks import add
>>> add.delay(1,4)
<AsyncResult: 8e1bb123-d7bf-4b57-9974-00f9fb5c444e>
>>> add.delay(1,4)
<AsyncResult: e9405e8a-66b5-4e5f-9662-38abb2118bd3>
>>> 
```
# you can see task executed in first terminal (*) as, 
```
[2015-10-16 08:17:54,261: INFO/MainProcess] Received task: demo.tasks.add[e9405e8a-66b5-4e5f-9662-38abb2118bd3]

[2015-10-16 08:17:54,372: INFO/MainProcess] Task demo.tasks.add[e9405e8a-66b5-4e5f-9662-38abb2118bd3] succeeded in 0.109354613s: 5
```
# Done.

Page formatted using : https://stackedit.io/

  
  
