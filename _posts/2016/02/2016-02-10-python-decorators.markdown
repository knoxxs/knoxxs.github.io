---
layout: post
title:  Python decorators
date:   2016-02-10 13:59:00
categories: programming python
tags: python decorator annotation profiler data-capture data-generation
---

This is the post about python decorators. I wanted to know what they are and I also wanted to write some decorators to profile my code and to record data.

## Introduction

1. [Primer on Python Decorators](https://realpython.com/blog/python/primer-on-python-decorators/)
2. [What does functools.wraps do?](http://stackoverflow.com/questions/308999/what-does-functools-wraps-do)
3. [Decorators and Wrappers in Python](http://hangar.runway7.net/python/decorators-and-wrappers)
    - Common Gotchas
    - Usage Ideas
    - Multiple decorators
    - Meta decorators
    
## Some usages

### Profiler

```python
import logging
import time
from functools import wraps


def profiler(f):
    """
    Logs the time a function takes to execute.
    :param f: function
    """

    @wraps(f)
    def wrapper():
        t1 = time.time()
        f()
        t2 = time.time()

        time_taken = t2 - t1

        logger = logging.getLogger(f.__module__)

        log_message = f.__name__ + " took " + str(time_taken) + " secs"
        if time_taken >= 3:
            logger.warn(log_message)
        else:
            logger.debug(log_message)

    return wrapper

```

### Data recorder

```python
import json
import logging
from functools import wraps
from logging.handlers import RotatingFileHandler

from config.logging import DATA_LOG_BASE_PATH


def record_data(f):
    """
    Records input and output of an function.

    :param f: function IO to record
    """

    @wraps(f)
    def wrapper(*args, **kwargs):
        result = f(*args, **kwargs)

        data = {'input': (args, kwargs), 'output': result}
        _get_logger(f).info(json.dumps(data))

        return result

    return wrapper


def _get_logger(f):
    # trying to get a logger with unique name so that no one else uses this loggger in normal code
    logger = logging.getLogger(f.__module__ + "_" + f.__name__ + "_data")

    # stopping propagation to parent logger so that only we should log it
    logger.propagate = False

    # 10 file of 10 mb
    handler = RotatingFileHandler(DATA_LOG_BASE_PATH + "_" + f.__module__ + "_" + f.__name__ + "_data.log",
                                  maxBytes=10485760, backupCount=10)

    formatter = logging.Formatter(fmt='[%(asctime)s] [%(message)s]')

    handler.setFormatter(formatter)
    handler.setLevel(logging.INFO)

    logger.addHandler(handler)
    logger.setLevel(logging.INFO)

    return logger
```
## Other examples
 
There are many more use cases of decorators. May can be found here:

1. [What are some common uses for Python decorators? [closed]](http://stackoverflow.com/questions/489720/what-are-some-common-uses-for-python-decorators)
2. [PythonDecoratorLibrary](https://wiki.python.org/moin/PythonDecoratorLibrary)
3. [What are common uses of Python decorators?](https://www.quora.com/What-are-common-uses-of-Python-decorators)