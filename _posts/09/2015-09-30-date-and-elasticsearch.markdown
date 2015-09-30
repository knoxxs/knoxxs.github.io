---
layout: post
title:  "Date and elasticsearch"
date:   2015-09-30 12:42:00
categories: programming elasticsearch
tags: date time-parsing es range-query
---

So yesterday I was trying to find the best way to store the opening hours for any vendor in the db(elasticsearch). I found out about the really flexible support given by elasticsearch.


## Storing
Elastic search has a special type `data` which can help es to know that its a date field and to handle it differently. Also the `date` type has one more attribute called `format` which help us to specify format of the date. There are many [formats supported](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-date-format.html#built-in) by the es, about 43 different formats. This is just the part of storing data. 
 
## Searching
As the main job of ES is to search, it doesn't stop here, it provides special [data math](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-date-format.html#date-math) capability to use in range queries.

As stated: 

> The expression starts with an `anchor` date, which can be either now or a date string (in the applicable format) ending with `||`. It can then follow by a math expression, supporting `+`, `-` and `/` (rounding). The units supported are `y` (year), `M` (month), `w` (week), `d` (day), `h` (hour), `m` (minute), and `s` (second). Here are some samples: `now+1h`, `now+1h+1m`, `now+1h/d`, `2012-01-01||+1M/d`.

To know more about it see [Ranges on date fields](https://www.elastic.co/guide/en/elasticsearch/reference/2.0/query-dsl-range-query.html#ranges-on-dates)

## Opening hours

[This SO question](http://stackoverflow.com/questions/25671150/filter-range-date-elasticsearch) gives the perfect solution for this.

The mappings will be stored like this:

```json
/PUT http://localhost:9200/demo/test/_mapping
{
  "test": {
    "properties": {
      "openingTimes": {
        "type": "object",
        "properties": {
          "monday": {
            "type": "nested",
            "properties": {
              "start": {
                "type": "date",
                "format": "hour_minute"
              },
              "end": {
                "type": "date",
                "format": "hour_minute"
              }
            }
          }
        }
      }
    }
  }
}
```

Then we can do the nested query:

```json
/POST http://localhost:9200/demo/test/_search
{
  "query": {
    "filtered": {
      "query": {
        "match_all": {}
      },
      "filter": {
        "nested": {
          "path": "openingTimes.monday",
          "filter": {
            "bool": {
              "must": [
                {
                  "range": {
                    "openingTimes.monday.start": {
                      "lte": "13:00"
                    }
                  }
                },
                {
                  "range": {
                    "openingTimes.monday.end": {
                      "gte": "14:00"
                    }
                  }
                }
              ]
            }
          }
        }
      }
    }
  }
}
```


### Using Java

If you are using Java then you might need [Joda-LocalTime](http://stackoverflow.com/questions/15573226/how-to-represent-time-of-day-ie-1900-in-java) to store time in Java and [SimpleDateFormat](http://docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html) for parsing. Also when using `SimpleDateFormat` don't forget to set the timeZone [`setTimeZone`](http://docs.oracle.com/javase/7/docs/api/java/text/DateFormat.html#setTimeZone(java.util.TimeZone))

## Reference

1. [Built In Formats](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-date-format.html#built-in)
2. [Date Math](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-date-format.html#date-math)
3. [Ranges on date fields](https://www.elastic.co/guide/en/elasticsearch/reference/2.0/query-dsl-range-query.html#ranges-on-dates)
4. [filter range date elasticsearch](http://stackoverflow.com/questions/25671150/filter-range-date-elasticsearch)
5. [How to represent time of day (ie. 19:00) in Java?](http://stackoverflow.com/questions/15573226/how-to-represent-time-of-day-ie-1900-in-java)
6. [SimpleDateFormat](http://docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html)