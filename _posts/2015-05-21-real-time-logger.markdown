---
layout: post
title:  "Monitoring Logger - A real-time analysable logger"
date:   2015-05-21 5:32:09
categories: java programming logging sprinklr
tags: java logger log real-time alert
---

This is my first post in the the series to write about programming patterns, frameworks, designs, tips I learned in Sprinklr.

Logger we all know. How important it is and how many times it saves our ass while debugging. Logging is really helpful with tools like sumo-logic, gray-logs etch. These service collect all the logs and make them searchable. You can also set automatic alerts on them. I also heard of a tool called Takipi which runs as a agent with your jar and automatically logs every exception with the complete context info. Haven't explored it completely yet. But we can do something extra with logging. Things like:

## Analysis
To do analysis we have to start logging in a particular format. That particular format can also include application specific details. Some of the things we can include are: ip, current login context, class name, module name, server types, hostname, calleeClass, calleeMethod, threadName etc.. Then if we save them in any tool with analysis capability (say ES), we are good to go.

## Automatic monitoring
There should a customizable way to monitor exceptions based on exception level, rate of change of exceptions, max exception per time period and then alerting system on it. Sor alerting of course we have emails, but we need to monitor exceptions and as we are doing it over time we have to be stateful.

## Sprinklr, Monitoring Logger Architecture:

#### Components:

1. MonitoringLogger: Logs
2. LogLevelController: Control enabled log level, level details, log rate change details per module type
3. ExceptionProcessor:
    - Process error and fatal logs
    - batch them
    - sends email alerts
4. Exception Email Alert sender
5. Exception configuration manager
6. Rolling Es client to log on new index everyday
7. RateOfChangeDetector to measure and detect rate of change error

#### Logging flow (All this is done asynchronously):

1. Log is called
2. Process and send alerts using ExceptionProcessor with email preferences from exception manager
3. Log in Es
4. Check rate of change, if threshold reached: log the same thing again as fatal and step 2 will handle email alert