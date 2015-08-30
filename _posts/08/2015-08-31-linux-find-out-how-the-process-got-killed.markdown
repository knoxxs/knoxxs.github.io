---
layout: post
title:  "Linux find out how the process got killed"
date:   2015-08-31 02:21:00
categories: programming linux ops
tags: programming linux monitoring logs kill process ops
---

Two days before I deployed a server on `gcloud` and started a python process as daemon. Today I saw this proces was not there. That means it got killed. How I don't know. So I started enquiring about it. There can be only two reasons because of which the process can be killed:

1. kernel killed it
2. exception occurred in process itself.

## Kernel killing
Kernel usually kills a process only when the process exceeds the max resources limit or availability be it the memory or open files etc. But before killing kernel logs it in the logs.

Some ways to know whether this is happening or not:

#### 1. Logs
There are many ways it can be logged but mainly all the kernel level logs are present in `/var/logs`. To find if the process is killed by kernel, the simplest thing I found on SO is `egrep -i -r "processname" /var/log`. Though I have some doubts whether the process name is printed in logs or not but till now I can work with it. Will update it in case I found the actual error.

#### 2. Process monitoring
Another way is to restart the process and start monitoring its resource usage over time. For this I used a script shared in a [SO answer](http://superuser.com/a/620051/116631). This script captures process information using `/proc/$PID/stam` in a loop. I modified the script to sleep in between. Here is the final version:

```bash
#!/usr/bin/env bash

## Get the PID of the process name given as argument 1
pidno=$1
sleeptime=$2
while [ 1 ]; do
    ## If the process is running, print the memory usage
     if [ -e /proc/$pidno/statm ]; then
     ## Get the memory info
      m=`awk '{OFS="\t";print $1,$2,$3,$6}' /proc/$pidno/statm`
     ## Get the memory percentage
      perc=`top -bd .10 -p $pidno -n 1  | grep $pidno | gawk '{print \$10}'`
     ## print the results
     ## Print header
      echo -e "Size\tResid.\tShared\tData\t%"
      echo -e "$m\t$perc";
      ## Sleeping
      sleep $sleeptime;
    ## If the process is not running
     else
      echo "$1 is not running";
     fi
done
```

The output of this contains following info:

```
size       total program size (same as VmSize in /proc/[pid]/status)
resident   resident set size (same as VmRSS in /proc/[pid]/status)
share      shared pages (from shared mappings)
data       data + stack
```

Then I ran the script for the process I want to monitor: `./mProcess.sh 15507 1m > ~/projects/supertext/supertext/logs/yowsup_statm.log &`.

## Self Killing
A process can kill itself in case of any unhandled exception or error. To know whether this is happening the only thing came to my mind is to redirect the process output and error to some files.

```
python ~/projects/supertext/yowsup/sample/run.py >>~/projects/supertext/supertext/logs/yowsup_out.log 2>>~/projects/supertext/supertext/logs/yowsup_error.log &
```

## Conclusion
There should be many tools out there which can help me monitor the process but I started with the basics and will upgrade my monitoring skills if this won't work out.

## Issues Faced
While running the `Process monitoring` script I have to install the `gawk` to my `gcloud centos` instance. While installing this I faced the following error:

```
Preconfiguring packages ...
dpkg: warning: 'ldconfig' not found in PATH or not executable.
dpkg: warning: 'start-stop-daemon' not found in PATH or not executable.
dpkg: error: 2 expected programs not found in PATH or not executable.
Note: root's PATH should usually contain /usr/local/sbin, /usr/sbin and /sbin.
E: Sub-process /usr/bin/dpkg returned an error code (2)
```

The [solution](http://unix.stackexchange.com/questions/160019/dpkg-cannot-find-ldconfig-start-stop-daemon-in-the-path-variable) to this I found on SE.

## References

1. [SO - Invoke and track memory usage of one process in linux](http://superuser.com/questions/620004/invoke-and-track-memory-usage-of-one-process-in-linux)
2. [SO - Redirect all output to file](http://stackoverflow.com/questions/6674327/redirect-all-output-to-file)
3. [SE - dpkg cannot find ldconfig/start-stop-daemon in the PATH variable](http://unix.stackexchange.com/questions/160019/dpkg-cannot-find-ldconfig-start-stop-daemon-in-the-path-variable)