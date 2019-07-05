## Deploying Django Channels v2 / Django 2.X on Heroku

### Terms:
+ Heroku: Platform as a Service a.k.a. PaaS
+ Python: Programming language of our choice
+ Django: Web Framework
+ Django Channels: Addition to Django, extending protocol support
+ Daphne: Django HTTP/WebSocket server

So let's say you already build your consumers, logic and channel layers and running manage.py gives you:
```python
Django version 2.1.9, using settings 'my_config.my_settings.dev'
Starting ASGI/Channels version 2.2.0 development server at http://127.0.0.1:8000/
```
You feel GREAT, everything is running fine locally, but now is time that you deploy your work in Heroku, so Front-End can start integrating with your code.
What needs to happen is Daphne to substitute your Gunicorn/Waitress or whatever WSGI server you are running at the moment, because Daphne will route based on the protocol to your WS or HTTP app/s. This will happen in your ```Procfile ```

#### FROM (in my test case I use Waitress for WSGI and Celery as task manager)
```
web: waitress-serve --port=$PORT core.wsgi:application
worker: celery worker --app=core.celery.app -l DEBUG
```

#### TO (Daphne setup substitutes Waitress setup and leave Celery as is)
```
web: daphne -p $PORT -b 0.0.0.0 core.asgi:application
worker: celery worker --app=core.celery.app -l DEBUG
```

There are few components here that you should know what they do:
+ $PORT is valid referral: $Port or $port are not valid , because PORT is auto set by Heroku. Do NOT set --port to static value.
+ Daphne needs to get your asgi application instance: here is how

You need to give Daphne argument of where you so-called asgi.py file is. Here is how mine looks like:
```python
 1 import os
 2 import django
 3 from channels.routing import get_default_application
 4
 5 os.environ.setdefault("DJANGO_SETTINGS_MODULE", "core.settings.local")
 6 django.setup()
 7 application = get_default_application()
```

If we go line by line we can see that on line 5 we get the settings our Django will run with or get by default - in my example our local settings.

#### Please note that the settings path is unique and in your project will be different!

On line 6 we tell Django to load its settings, because later in ```get_default_application()``` we have to have them in memory.
``` get_default_application()``` is dead simple function that:
+ Looks in your Django settings for ```ASGI_APPLICATION = "core.routing.application"``` definition.
+ Strips down the path ```core.routing``` and the name of the application ```application```.
+ Tries to import the path ```core.routing```

My ```core/routing.py``` looks like this:

```python
  1 from my_websocket_django_app.my_custom_auth import MyCustomAuthMiddleware
  2 from channels.routing import ProtocolTypeRouter, URLRouter
  3 import my_websocket_django_app.routing
  4 
  5 application = ProtocolTypeRouter({
  6     'websocket': MyCustomAuthMiddleware(
  7         URLRouter(
  8             my_wensocket_django_app.routing.websocket_urlpatterns
  9         )
 10     ),
 11 })
```

If ``` get_default_application()``` succeeds it provides Daphne with channels application instance to work with.

Thats all it takes.
Enjoy!
