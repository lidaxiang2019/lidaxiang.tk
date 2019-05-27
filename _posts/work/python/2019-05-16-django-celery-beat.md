---
layout: article
title:  "Django celery beat"
rootCate: "work"
categories:
- Python
tags:
- work
- Python3
- Django
- celery
- beat
---

总结 Django Framework 可以利用的第三方库，以备后用。

<!---more--->

## Install apps
1. Use pip to install the package:
```
pip3 install django-celery-beat==1.4.0
```

2. Add the django_celery_beat module to INSTALLED_APPS in your Django project’ settings.py:
```
INSTALLED_APPS = (
    ...,
    'django_celery_beat',
)
```

3. Apply Django database migrations so that the necessary tables are created:
```
python manage.py migrate
```

4. 启动服务,本来应该启动 `beat` service 和 `celery` service，但是也可以通过下面的一行命令一起启动。 Start the celery beat service using the django_celery_beat.schedulers:DatabaseScheduler scheduler:

```
celery -A [project-name] worker --beat --scheduler django --loglevel=info
```

5. 通过代码(下一部分)或者 Visit the Django-Admin interface to set up some periodic tasks.


## 代码
```
from django_celery_beat.models import CrontabSchedule, PeriodicTask
schedule, _ = CrontabSchedule.objects.get_or_create(
    					minute='48',hour='09',day_of_week='*',day_of_month='*', month_of_year='*')
PeriodicTask.objects.create(crontab=schedule,
                            name='any_name',
                            task='proj.tasks.import_contacts',)
```


## 参考文档
[解决nginx+uwsgi部署Django的所有问题](https://django-celery-beat.readthedocs.io/en/latest/)

[](http://docs.celeryproject.org/en/latest/userguide/periodic-tasks.html#using-custom-scheduler-classes)

