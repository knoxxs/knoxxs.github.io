---
layout: post
title:  Backing up Es using S3
date:   2015-12-01 11:24:00
categories: programming database
tags: es s3 cron backup snapshot
---

In this post I am going to cover the steps to schedule regular your ES data backup to S3.

## Introduction to S3
Amazon Simple Storage Service (Amazon S3), provides developers and IT teams with secure, durable, highly-scalable object storage. Amazon S3 offers a range of storage classes designed for different use cases including Amazon S3 Standard for general-purpose storage of frequently accessed data, Amazon S3 Standard - Infrequent Access (Standard - IA) for long-lived, but less frequently accessed data, and Amazon Glacier for long-term archive. To know more follow [this](https://aws.amazon.com/s3/).

## Setting up S3
You need to create a new bucket and create a user with full access permission to this bucket. Follow [this](https://serversforhackers.com/video/backup-to-s3) for complete steps. Unlike as it explained in the tutorial you have to give the full access of the bucket to user. Es backup process (called [Snapshot & Restore](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-snapshots.html)) also need to read/delete from ES, not only add.

## Setting up awscli

```bash
1. sudo apt-get install python-pip
2. sudo pip install awscli
3. aws configure
    1. Default region name [None]: us-east-1
    2. Default output format [None]: json
4. Test it: `aws s3 cp <test-file> s3://<bucket-name>/test`
```

Follow [this](https://serversforhackers.com/video/backup-to-s3) for complete steps.

## Setting up ES for backup

### Install elasticsearch-cloud-aws

```bash
1. `/bin/plugin -install elasticsearch/elasticsearch-cloud-aws/2.7.1`
2. Restart ES
3. Check if the plugin is installed: `curl http://localhost:9200/_cat/plugins`
```

To determine exact version of `elasticsearch-cloud-aws` follow [this](https://github.com/elastic/elasticsearch-cloud-aws)

### Setup snapshot config

```bash
curl -XPUT 'http://localhost:9200/_snapshot/<snapshot_config_name>' -d '{ "type": "s3", "settings": { "bucket": "<bucket-name>", "region": "us-east-1", "base_path": "es/local", "access_key": "<key>", "secret_key": "<secret>" } }'

You will get following response: `{"acknowledged":true}`
```

### Testing Backup

```bash
Run `curl -XPUT localhost:9200/_snapshot/<snapshot_config_name>/<snapshot_name>?wait_for_completion=true`.

You will get following response:
{"snapshot":{"snapshot":"sample2","version_id":1070199,"version":"1.7.1","indices":["agent","customer","quickresponse","ticket","task","comment","customfield","message","conversationcontext","roleandpermissions","taskcategoryspecificdata","vendor"],"state":"SUCCESS","start_time":"2015-11-30T05:50:03.749Z","start_time_in_millis":1448862603749,"end_time":"2015-11-30T05:53:21.786Z","end_time_in_millis":1448862801786,"duration_in_millis":198037,"failures":[],"shards":{"total":60,"failed":0,"successful":60}}}
```

### Creating a script to backup

```bash
#!/usr/bin/env bash

BACKUP_CONFIG_NAME="s3-backup"
BACKUP_NAME="`date +%Y-%m-%d-%H-%M`-daily_complete_backup"

# Print meta data
echo "################################################################################"
echo "######################### Starting es data backup ##############################"
echo "################################################################################"
echo "########################## `date +%Y-%m-%d-%H-%M` ##############################"
echo "################################################################################"

# call for backup
BACKUP_OUTPUT=$(curl -XPUT localhost:9200/_snapshot/${BACKUP_CONFIG_NAME}/${BACKUP_NAME}?wait_for_completion=true)

# Test result of last command run
if [ "$?" -ne "0" ]; then
    echo "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    echo "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX ES backup failed XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    echo "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    exit 1
fi

# If success, remove backup file
echo $BACKUP_OUTPUT

echo "################################################################################"
echo "######################### Done es data backup ##############################"
echo "################################################################################"

exit 0
```

Make it executable: `chmod +x /home/aapa/scripts/es_backup.sh`

### Scheduling regular backup

```bash
To schedule we are going to use `cron`. First to test we are going to schedule it to run every minute.

1. Create a file in `/etc/cron.d/`.
2. Write `* * * * * aapa bash /home/aapa/scripts/es_backup.sh &>> /var/log/super/es_backup.log`
3. Check if it ran in `cat /var/log/syslog`. You will see a line in the output like: `Dec  1 11:00:01 dexter CRON[31335]: (aapa) CMD (bash /home/aapa/scripts/es_backup.sh &>> /var/log/super/es_backup.log)`
4. Reschedule it to the time suitable to you. I want it to run at every 10 hour, so I used: `0 */10 * * * aapa bash /home/aapa/scripts/es_backup.sh &>> /var/log/super/es_backup.log`
```

Follow [this](http://www.cyberciti.biz/faq/how-do-i-add-jobs-to-cron-under-linux-or-unix-oses/) to learn cron syntax.

## Other considerations

The Snapshot & Restore API by ES is too powerful. There are many things you can do with it. On thing I like to mention is:

> The index snapshot process is incremental. In the process of making the index snapshot Elasticsearch analyses the list of the index files that are already stored in the repository and copies only files that were created or changed since the last snapshot. That allows multiple snapshots to be preserved in the repository in a compact form. Snapshotting process is executed in non-blocking fashion. All indexing and searching operation can continue to be executed against the index that is being snapshotted. However, a snapshot represents the point-in-time view of the index at the moment when snapshot was created, so no records that were added to the index after the snapshot process was started will be present in the snapshot. The snapshot process starts immediately for the primary shards that has been started and are not relocating at the moment.

To know more about it follow [this](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-snapshots.html) and [this](https://www.elastic.co/guide/en/elasticsearch/guide/current/backing-up-your-cluster.html).
## References

1. [Backup to S3](https://serversforhackers.com/video/backup-to-s3)
2. [Snapshot And Restore](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-snapshots.html)
3. [elastic/elasticsearch-cloud-aws](https://github.com/elastic/elasticsearch-cloud-aws)
4. [HowTo: Add Jobs To cron Under Linux or UNIX?](http://www.cyberciti.biz/faq/how-do-i-add-jobs-to-cron-under-linux-or-unix-oses/)
5. [Elasticsearch backup to s3](http://consciou.us/index.php/2014/07/elasticsearch-backup-to-s3/)
6. [Backing Up Your Cluster](https://www.elastic.co/guide/en/elasticsearch/guide/current/backing-up-your-cluster.html)
7. [Amazon S3](https://aws.amazon.com/s3/)