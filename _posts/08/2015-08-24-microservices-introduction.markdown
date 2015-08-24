---
layout: post
title:  "Microservices Introduction"
date:   2015-08-24 23:27:00
categories: programming architecture
tags: microservices "software design"
---

I saw this huge post on MicroService Architecture written by Martin Fowler. Though I am very bad at reading but as this post is from Martin Fowler, I know it will be worth the time I will spend on it.

## Introduction
Microservice Architecture: suites of independently deployable services. There are certain common characteristics of this approach: automated deployment, intelligence in the endpoints, and decentralized control of languages and data.

In short, the microservice architectural style is an approach to developing a single application as a suite of small services, each running in its own process and communicating with lightweight mechanisms, often an HTTP resource API. These services are built around business capabilities and independently deployable by fully automated deployment machinery. There is a bare minimum of centralized management of these services, which may be written in different programming languages and use different data storage technologies. Also differnt services can scale differently. To understand it in more better way we have to look at the opposite of this approach.

#### The Monolithic Style:
A monolithic application built as a single unit. Enterprise Applications are often built in three main parts: a client-side user interface, a database and a server-side application. This server-side application is a monolith - a single logical executable. Any changes to the system involve building and deploying a new version of the server-side application.

All your logic for handling a request runs in a single process, allowing you to use the basic features of your language to divide up the application into classes, functions, and namespaces. With some care, you can run and test the application on a developer's laptop, and use a deployment pipeline to ensure that changes are properly tested and deployed into production. You can horizontally scale the monolith by running many instances behind a load-balancer.

**Issues**:

Change cycles are tied together - a change made to a small part of the application, requires the entire monolith to be rebuilt and deployed. Scaling requires scaling of the entire application rather than parts of it that require greater resource.

A pictorial summary:

![Monolithic Vs MicroServices]({{ site.url }}assets/2015/08/microservices-introduction/monolithic_vs_microservices.png)

## Characteristics of a Microservice Architecture
Its not the formal definition but the emerged characteristics.

### Componentization via Services
To build systems by plugging together components is always desired. But what makes a component. Our definition is that a component is a unit of software that is independently replaceable and upgradeable.

Microservice architectures will use libraries, but their primary way of componentizing their own software is by breaking down into services. We define libraries as components that are linked into a program and called using in-memory function calls, while services are out-of-process components who communicate with a mechanism such as a web service request, or remote procedure call. (This is a different concept to that of a service object in many OO programs .)

**Benefits**:

1. Services are independently deployable.
2. More explicit component interface. Most languages do not have a good mechanism for defining an explicit Published Interface. Often it's only documentation and discipline that prevents clients breaking a component's encapsulation, leading to overly-tight coupling between components.

**Issues**:

1. Remote calls are more expensive than in-process calls, and thus remote APIs need to be coarser-grained.

### Organized around Business Capabilities
When looking to split a large application into parts, often management focuses on the technology layer, leading to UI teams, server-side logic teams, and database teams. When teams are separated along these lines, even simple changes can lead to a cross-team project taking time and budgetary approval. A smart team will optimise around this and plump for the lesser of two evils - just force the logic into whichever application they have access to. Logic everywhere in other words. This is an example of Conway's Law in action.

>Any organization that designs a system (defined broadly) will produce a design whose structure is a copy of the organization's communication structure.
>
>-- Melvyn Conway, 1967

The microservice approach to division is different, splitting up into services organized around business capability. Such services take a broad-stack implementation of software for that business area, including user-interface, persistant storage, and any external collaborations.

>How Big is a microservice?
>
>Although microservice has become a popular name for this architectural style, its name does lead to an unfortunate focus on the size of service, and arguments about what constitutes “micro”. In our conversations with microservice practitioners, we see a range of sizes of services. The largest sizes reported follow Amazon's notion of the Two Pizza Team (i.e. the whole team can be fed by two pizzas), meaning no more than a dozen people. On the smaller size scale we've seen setups where a team of half-a-dozen would support half-a-dozen services.

### Products not Projects