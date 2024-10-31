# local settings
/local/local_settings.py

## manage.py

Find the django settings
```
python manage.py diffsettings --all
```

## Console

Find Setting from the console
```
manage.py shell
```
Find Setting by name
```
from django.conf import settings
print(settings.DATABASES)
print(settings.SESSION_ENGINE)
print(settings.CACHE)
```
List Installed Apss
```
from django.conf import settings
for i in settings.INSTALLED_APPS:      print(i)
```
