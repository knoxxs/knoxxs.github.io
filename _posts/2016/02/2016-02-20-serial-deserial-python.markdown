---
layout: post
title:  Python [de]serialisation {draft}"
date:   2016-02-20 16:59:00
categories: programming python json
tags: json pickle type metaclass jsonconstruct simplejson demjson mongoengine
---

This post is about json serialisation and de-serialisation in python

1. Issue in python as compared with java
    1. Python vars can be referring to function, how to convert that into json. I know it can be just like any other object, but its a difference
    2. Python is duck-typed so in de-serialisation how you will know to what type this json should match.
     
To solve de-serialisation problem you need type info. It can be provided in following manner:

1. In the json itself using the keyword `type`
2. While defining the class you specify the type. Like you do in mongoengine
3. While de-serialisation you pass the class name, but how de-serialiser will know the type of the nested object. To solve for nested objects there are multiple solutions:
    1. Manually write decoder and encoder of every class. In the decoder/encoder function you hard code the type of nested objects. Its like the JsonSerialisation interface we used in Java.
    2. By first two solutions

All this also popped one more question in my mind what is the benefit of duck-typing. The answer is to make it generic. So in python you have to do extra work in ser/deser and in java you have to do extra work in generics. Which is better than?


`json` doesn't ahndle complex objects
`pickle` adds metadata, which UI have to send back for proper de-serialisation. Why json pickle need these many tags: https://github.com/jsonpickle/jsonpickle/blob/master/jsonpickle/tags.py
`jsonconstruct` doesn't handle nested classes

How jsonpickle filters the extra data in the object like __name__, __doc__. Is json pickle better than using obj.__dict__ and then filtering extra data (like http://stackoverflow.com/a/35483750/742173). Currently jsonpickle uses inspector.py.

Solutions in my mind (If all the tags jsonpickle add, those are needed)

1. For internal data use json pickle, also for db
2. For DTO and rest layer should not use jsonpickle. Should define any way to define the mapping between the incoming json data and type. Ideal way would be like mongoengine way. Temp way would be like JsonSerialisation interface way with jsonconstruct way to specify the type.
3. Any way to define custome type adaptor like we do in java for data
4. any way to define serialisation names



References:

http://stackoverflow.com/questions/6578986/how-to-convert-json-data-into-a-python-object
http://jsonpickle.github.io/api.html#jsonpickle-high-level-api, http://jsonpickle.github.io/, https://groups.google.com/forum/#!forum/jsonpickle, http://jsonpickle.github.io/contrib.html, https://github.com/jsonpickle/jsonpickle
https://github.com/initialxy/jsonstruct/network
https://eev.ee/blog/2015/10/15/dont-use-pickle-use-camel/ - Any issues in using pickle
https://github.com/Toubs/PyJSONSerialization/blob/master/PyJSONSerialization.py - using only type not many tags like json pickle
http://www.jsonweb.info/en/latest/index.html - using only type not many tags like json pickle and also many other features, some good use of decorators
http://stackoverflow.com/questions/3768895/python-how-to-make-a-class-json-serializable
http://stackoverflow.com/a/18561055/742173 - consideration of mixing these libs
http://stackoverflow.com/questions/10252010/serializing-python-object-instance-to-json
http://stackoverflow.com/questions/3768895/python-how-to-make-a-class-json-serializable
https://github.com/mattupstate/overholt/blob/master/overholt/helpers.py#L35-L42


Need to consider:

1. https://pypi.python.org/pypi/pyson/0.2
2. https://github.com/esnme/ultrajson/tree/master/python
3. https://pypi.python.org/pypi/metamagic.json/
4. http://pykler.github.io/yajl-py/
5. https://pypi.python.org/pypi/demjson/2.2.4
6. https://pypi.python.org/pypi/jsonlib


Ideal solution:

1. Initial like jsonconstruct , gson or jackson where you pass a class.
2. Then each class should define its own type info, by either defining types like mongoengine or toJson/fromJson method by extending JsonSerializable class.

This way there will be no need of storing the type info in json object itself. Still the idea solution should support metadata in json too. For smooth working with other options.

