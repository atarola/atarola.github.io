---
layout: post
title: "Thing a Week Progress, Pt. 1"
date: 2019-04-22
tags: ["development", "thing_a_week"]
---

Wherein I'll describe my first weeks' progress.

## Done

I finished the basic framework for the app and completed login via Google's OpenID API.  Not as much as I hoped, but with a new language and framework, I guess I shouldn't expect the world yet.

The frameworks I've landed on (so far) for the backend are:

* rust
* rocket
* reqwest
* serde
* mongodb

I'm going to implement an application layout I've done at least twice before, it looks something like:

![Architectural Diagram](/public/img/thing-a-week-pt1/arch-diagram.png)

### Handlers

Rocket marked-up fn's, responsible for translating to and from html, and security / CSRF handling.

### Commands

Implementation of all of the work necessary to do a transaction in the API.  Having one file per allows us to keep all of the business logic here in one place.

See: [Transaction Script](https://www.martinfowler.com/eaaCatalog/transactionScript.html)

### Services

Process requests to other APIs (eg: Google's OpenAPI implementation).  They should abstract away any authn / authz and protocol handling to the external services.

See: [Anti-Corruption Layer](https://markhneedham.com/blog/2009/07/07/domain-driven-design-anti-corruption-layer/)

### DAO

Handle talking from and to the database.  Should abstract away the database interals.

See: [Data Access Object](https://en.wikipedia.org/wiki/Data_access_object)

### Data Structs

Plain old rust structs (PORS?) that represent data in the system.

## Doing

With the layout and working code done, the project should transition from being more exploratory to being normal web-development.  In that vein, I'll be doing some user flows to figure out the UX and IA, then implementing the backends to support my findings.

I usually think about a webapp as a set of overlapping state-machines, and I'll model this one as such.  Since I'll be doing the work anyways, I'll show off my output next week.  

I guess this means that I've bitten off more than I can chew in a week.  Thats ok, struggling means learning.  
