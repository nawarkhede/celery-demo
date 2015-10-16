# celery-demo
How to get started with django and celery ? 

1. create virtualenv and activate it.
2. install the following stuff,
    ```
    pip install django
    
    pip install django-celery
  ```
3. create django project 
4. add following apps to INSTALLED_APPS
   ```
   'djcelery',
   
   'kombu.transport.django',
   ```
   
   <b>djcelery is always needed. kombu.transport.django is the Django-based broker, for use mainly during development.</b>
   
5. add followoing code to settings.py
  ```
  import djcelery
  
  djcelery.setup_loader()
  
  BROKER_URL = 'django://'
  ```
  and this line also, 
  
  ```
  CELERY_IMPORTS=("demo.tasks")
  ```
  
6. now create tables,

  ```
  python manage.py migrate
  ```
  
7. create new file in demo folder as ```task.py``` and add following code to it,

  ```
  from celery import task
  
  @task()
  
    def add(x, y):
    
    return x + y
  ```
    
8. open another terminal and execute following line (*) ,

    ```
    python manage.py celery worker --loglevel=info
    ```
    <br> keep it running </b>
    
9. open django shell and try the following code,
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
10. you can see task executed in first terminal (*) as, 
  ```
  [2015-10-16 08:17:54,261: INFO/MainProcess] Received task: demo.tasks.add[e9405e8a-66b5-4e5f-9662-38abb2118bd3]
  
  [2015-10-16 08:17:54,372: INFO/MainProcess] Task demo.tasks.add[e9405e8a-66b5-4e5f-9662-38abb2118bd3] succeeded in 0.109354613s: 5
  ```
11. Done (y)

  
  
