---
layout: post
title:  Job scheduler in python
date:   2016-02-29 18:45:00
categories: python scheduler
tags: cron scheduler APS celery job python
---

I want a job scheduler in python something like [Quartz-Scheduler](https://quartz-scheduler.org/) for java. 

## Options

1. [sched – Generic event scheduler](https://pymotw.com/2/sched/)
2. [Advanced Python Scheduler](https://apscheduler.readthedocs.org/en/latest/index.html)
3. [Celery: Distributed Task Queue](http://www.celeryproject.org/) - [Periodic tasks](http://celery.readthedocs.org/en/latest/userguide/periodic-tasks.html)
4. [dbader/schedule](https://github.com/dbader/schedule)
5. There are many crontab wrapper which I haven't included.

**References**:

1. [How do I get a Cron like scheduler in Python?](How do I get a Cron like scheduler in Python?)
2. [Schedule – Python job scheduling for humans](Schedule – Python job scheduling for humans)

**Comparison**:

1. [Consider switching from APScheduler to celery](https://github.com/ckan/ckan-service-provider/issues/8)
2. [APScheduler 3.0 released](http://alextechrants.blogspot.in/2014/08/apscheduler-30-released.html)
3. [Celery - schedule periodic tasks starting at a specific time](http://stackoverflow.com/questions/7848512/celery-schedule-periodic-tasks-starting-at-a-specific-time)

## APS

I choose PS finally as it is more powerful and celery requires setup time and is is not meant for task scheduling primarily.

**Extra resources**:

1. [Scheduled Jobs with Custom Clock Processes in Python with APScheduler](https://devcenter.heroku.com/articles/clock-processes-python)
2. [apscheduler in Flask executes twice](http://stackoverflow.com/questions/14874782/apscheduler-in-flask-executes-twice)
3. [How to run recurring task in the Python Flask framework?](http://stackoverflow.com/questions/25639221/how-to-run-recurring-task-in-the-python-flask-framework)


