---
layout: post
title:  Which DB To Choose
date:   2015-07-19 15:06:00
categories: programming database comparison
tags: database programming nosql sql mongodb couchdb elasticsearch redis
---

Till now I have only used sql, mongodb, redis (a bit) and elasticsearch. But I always have one question in my mind about the existence of so many dbs. Though I don't know all dbs but I want to know a bit about all the different techniques available in the db world. So that when time comes I can choose the best one. It is not possible to get a hands on every possible db but I can use others knowledge.  Though I have one benefits as the three dbs I know cover three different technique: sql (mysql), nosql (mongodb), search engine (elasticsearch), in-memory & key-based (redis). This gives me a advantage to compare other techniques. I will keep updating this post whenever I will learn anything new about dbs.

## NOSQL

It can imply to:

1. Not using the relational model
2. Running well on clusters
3. Mostly open-source
4. Built for the 21st century web estates
5. Schema-less

## Why NOSQL

### Aggregate Modeling
Application developers have been frustrated with the impedance mismatch between the relational data structures and the in-memory data structures of the application. This started a movement towards aggregate models, most of this is driven by [Domain Driven Design](http://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215), a book by Eric Evans.

![In memory data structure]({{ site.url }}assets/whichDbToChoose/in-memory-data-structure.png)

Aggregate-oriented databases make inter-aggregate relationships more difficult to handle than intra-aggregate relationships.Aggregate-oriented databases often compute materialized views to provide data organized differently from their primary aggregates. This is often done with map-reduce computations, such as a map-reduce job to get items sold per day.

### Data Scale
The rise of the web as a platform also created a vital factor change in data storage as the need to support large volumes of data by running on clusters. 

### Distribution Models
Aggregate oriented databases make distribution of data easier, since the distribution mechanism has to move the aggregate and not have to worry about related data, as all the related data is contained in the aggregate.

**Sharding**: Sharding distributes different data across multiple servers, so each server acts as the single source for a subset of data.
**Replication**: Replication copies data across multiple servers, so each bit of data can be found in multiple places. Replication comes in two forms:

1. Master-slave replication makes one node the authoritative copy that handles writes while slaves synchronize with the master and may handle reads.
2. Peer-to-peer replication allows writes to any node; the nodes coordinate to synchronize their copies of the data.

Master-slave replication reduces the chance of update conflicts but peer-to-peer replication avoids loading all writes onto a single server creating a single point of failure. 

## The CAP Theorem:
[Visual Guide to NoSQL Systems](http://blog.nahurst.com/visual-guide-to-nosql-systems) is an awesome post written by Nathan Hurst. No one could have explained it better. Just follow this.
 
Here's the summary:
![Visual Guide to NoSQL Systems]({{ site.url }}assets/whichDbToChoose/nosql-visual-guide.png)

As you can see, there are three primary concerns you must balance when choosing a data management system: consistency, availability, and partition tolerance.

1. Consistency means that each client always has the same view of the data.
2. Availability means that all clients can always read and write.
3. Partition tolerance means that the system works well across physical network partitions.

>Eric Brewer put forth the CAP theorem which states that in any distributed system we can choose only two of consistency, availability or partition tolerance. 

Many NoSQL databases try to provide options where the developer has choices where they can tune the database as per their needs. For example if you consider [Riak](http://basho.com/riak) a distributed key-value database. There are essentially three variables r, w, n where:

- r=number of nodes that should respond to a read request before its considered successful.
- w=number of nodes that should respond to a write request before its considered successful.
- n=number of nodes where the data is replicated aka replication factor.

In a Riak cluster with 5 nodes, we can tweak the r,w,n values to make the system very consistent by setting r=5 and w=5 but now we have made the cluster susceptible to network partitions since any write will not be considered successful when any node is not responding.

> NoSQL databases provide developers lot of options to choose from and fine tune the system to their specific requirements. Not as RDMS
In addition to CAP configurations, another significant way data management systems vary is by the data model (there are others, but these are the main ones):

## Types of NoSQL Databases:

### Relational systems
These are the databases we've been using for a while now. RDBMSs and systems that support ACIDity and joins are considered relational.

### Key-value

1. Key-value stores are the simplest NoSQL data stores to use from an API perspective.
2. The client can either get the value for the key, put a value for a key, or delete a key from the data store.
3. The value is a blob that the data store just stores, without caring or knowing what's inside.
4. Since key-value stores always use primary-key access, they generally have great performance and can be easily scaled.

### Document-oriented

1. The database stores and retrieves documents, which can be XML, JSON, BSON etc.
2. These documents are self-describing, hierarchical tree data structures which can consist of maps, collections, and scalar values.
3. The documents stored are similar to each other but do not have to be exactly the same.
4. They provides a rich query language and constructs such as database, indexes like RDMS.

### Column-oriented

1. Column-family databases store data in column families as rows that have many columns associated with a row key.
2. Each column family can be compared to a container of rows in an RDBMS table where the key identifies the row and the row consists of multiple columns. The difference is that various rows do not have to have the same columns, and columns can be added to any row at any time without having to add it to other rows.
3. When a column consists of a map of columns, then we have a super column. A super column consists of a name and a value which is a map of columns. Think of a super column as a container of columns.

### Graph Databases

1. Graph databases allow you to store entities and relationships between these entities.
2. Entities are also known as nodes, which have properties.
3. Relations are known as edges that can have properties.
4. Edges have directional significance.
5. The organization of the graph lets the data to be stored once and then interpreted in different ways based on relationships.
6. Traversing the joins or relationships is very fast. The relationship between nodes is not calculated at query time but is actually persisted as a relationship. Traversing persisted relationships is faster than calculating them for every query.
7. Nodes can have different types of relationships between them.
8. Since most of the power from the graph databases comes from the relationships and their properties, a lot of thought and design work is needed to model the relationships.

**Relationships** are first-class citizens in graph databases; most of the value of graph databases is derived from the relationships. Relationships don't only have a type, a start node, and an end node, but can have properties of their own. Using these properties on the relationships, we can add intelligence to the relationship—for example, since when did they become friends, what is the distance between the nodes, or what aspects are shared between the nodes. These properties on the relationships can be used to query the graph.

## Available DB options:

I am creating a table comparing all the dbs. Will update soon.

# Resources

1. [Visual Guide to NoSQL Systems](http://blog.nahurst.com/visual-guide-to-nosql-systems)
2. [NoSQL Databases: An Overview](http://www.thoughtworks.com/insights/blog/nosql-databases-overview)
3. [MONGODB VS COUCHDB](http://blog.scottlogic.com/2014/08/04/mongodb-vs-couchdb.html)