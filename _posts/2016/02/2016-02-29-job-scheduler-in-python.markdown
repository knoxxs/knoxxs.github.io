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

1. [APScheduler Job not running after restart](http://stackoverflow.com/questions/35707630/apscheduler-job-not-running-after-restart/35708579#35708579)
2. My chat on IRC understanding the differences between Quartz and APS:
    
    ```
    knoxxs
    Hey guys can someone help me with http://stackoverflow.com/questions/35707630/apscheduler-job-not-running-after-restart
    
    agronholm
    knoxxs: have you read the docs?
    
    knoxxs
    Ya, I read them today only. Am I missing something basic? :/
    
    agronholm
    yeah, the description of how misfire grace time works
    
    knoxxs
    let me check
    
    knoxxs
    thanks
    
    agronholm
    if this was a recurring job it'd simply calculate its next run time
    
    knoxxs
    Ya after defining `misfire_grace_time` it worked. Actuallly, when I debugged the code, I saw it submitting the job to the threadpool.
    
    knoxxs
    One more thing, the benefit of storing the sate in persistent store is only beneficial in case of error. Am I right?
    
    knoxxs
    I am coming from Java background and used Quartz scheduler. It didn't had any such functionality. So I am having a hard time in digesting it.
    
    knoxxs
    @agronholm
    
    agronholm
    knoxxs: storing the state is beneficial if you need a job to persist over restarts
    
    agronholm
    in my case I needed persistence so that if the app was down when it needed to generate reports, it wouldn't know to do that otherwise after restart
    
    agronholm
    or worse, it would do it again and overwrite the reports
    
    agronholm
    for that I'd have to set a misfire grace time of several hours at least
    
    knoxxs
    ok
    
    knoxxs
    one more thing...after reading the docs, I understood that in APS we need the register the job after creating the scheduler instance.
    
    knoxxs
    Because of this the if someone has to write a new job they have to register it manually. Is there any way to automate this.
    
    knoxxs
    IMHO Quartz, the java scheduler, used to store the registry in db
    
    knoxxs
    I think I can do the same with APS too.
    
    knoxxs
    Am i thinking in right direction? Also why no one else needed it before?
    
    agronholm
    knoxxs: what registry are you talking about?
    
    agronholm
    if you add a job to the job store, it will stay there if the job store is persistent
    
    knoxxs
    Initially I have to add the job to scheduler instance.
    
    agronholm
    yes, as you would in quartz too
    
    knoxxs
    Ohhh I got it ... In quartz I used to extend a class. That must be informing quartz
    
    knoxxs
    base job class of quartz
    
    knoxxs
    thanks... Yeah The standard base class was doing everything. I checked it.
    
    knoxxs
    Thanks for all the help. :)
    ```
**Extra resources**:

1. [Scheduled Jobs with Custom Clock Processes in Python with APScheduler](https://devcenter.heroku.com/articles/clock-processes-python)
2. [apscheduler in Flask executes twice](http://stackoverflow.com/questions/14874782/apscheduler-in-flask-executes-twice)
3. [How to run recurring task in the Python Flask framework?](http://stackoverflow.com/questions/25639221/how-to-run-recurring-task-in-the-python-flask-framework)


