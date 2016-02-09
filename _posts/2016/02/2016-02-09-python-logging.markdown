---
layout: post
title:  Python logging
date:   2016-02-09 21:43:00
categories: programming python
tags: python logging setup handler logger filter 
---

Setting up logging in python application with features like rotating logger.

## Introduction

1. Read [The Hitchhiker’s Guide to Python - Logging](http://docs.python-guide.org/en/latest/writing/logging/).
2. Read [Logging HOWTO](https://docs.python.org/2/howto/logging.html#logging-basic-tutorial) complete.

## Setting up logging:

I decided to use `dict` based config.

1. [An example dictionary-based configuration](https://docs.python.org/2/howto/logging-cookbook.html#an-example-dictionary-based-configuration)
2. [Elegant setup of Python logging in Django](https://stackoverflow.com/questions/1598823/elegant-setup-of-python-logging-in-django#)
3. [logging.config — Logging configuration](https://docs.python.org/2/library/logging.config.html#configuration-dictionary-schema)
4. [LogRecord attributes](https://docs.python.org/2/library/logging.html#logrecord-attributes)

I used the following configuration:

```python
LOGGING = {
    'version': 1,
    'disable_existing_loggers': True,
    'formatters': {
        'verbose': {
            'format': '[%(asctime)s] [%(levelname)s] [%(name)s:%(funcName)s:%(lineno)d] [%(process)d:%(thread)d] %(message)s'
        },
        'simple': {
            'format': '%(levelname)s %(message)s'
        },
    },
    'filters': {

    },
    'handlers': {
        'console': {
            'level': 'DEBUG',
            'class': 'logging.StreamHandler',
            'formatter': 'verbose'
        },
        'file': {
            'level': 'DEBUG',
            'class': 'logging.handlers.RotatingFileHandler',
            'filename': 'altin.log',
            'maxBytes': '16777216',  # 16megabytes
            'formatter': 'verbose',
            'backupCount': 10
        },
    },
    'loggers': {

    },
    'root': {
        'handlers': ['file'],
        'propagate': True,
        'level': 'DEBUG'
    }
}
```

And used it as following:

```python
import logging
import logging.config


logging.config.dictConfig(LOGGING)

logger = logging.getLogger(__name__)
logger.debug("Sample msg")
```

## Compression

Haven't implemented it but the process is very simple as mentioned in [Python, want logging with log rotation and compression](http://stackoverflow.com/questions/8467978/python-want-logging-with-log-rotation-and-compression).