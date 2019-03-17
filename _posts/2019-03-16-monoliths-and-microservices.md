---
layout: post
title: "Monoliths and Microservices, Oh My!"
date: 2019-03-16
tags: ["development", "architecture", "microservices"]
---

Wherein I'll talk about monoliths vs. microservices, where to start, and if you decide to migrate from a monolith, how to do so sanely.

*Note:* I make no value judgement between them. Both approaches have advantages, and you should make the best decision based on the application in question (including the environment in which it resides).

### Start with a monolith!

If you are running a new or high-risk project,  I would not start with microservices at all.  The most important thing you should be doing is managing the risks associated with where you are at.  Tackling the complexity that comes with microservices is a huge distraction you probably can't afford.

Build your MVP as a monolith and focus on getting happy, paying customers.

### When should you split the monolith?

As I mentioned before, microservices bring a lot of baggage with them, when it comes to technical complexity.  So you should make sure the value you get from the move is worth the debts you'll be paying to get there.

Here are some example situations that could force you to consider doing a split:

#### Organizational Pressures

When an organization gets to be a certain size, it becomes very hard for a single team of developers to keep up-to-date context of what is going on in every facet of the business.  This will usually shows up as externally created `Bounded Contexts` popping up all over the place.  

The nasty bit about multiple `Bounded Contexts` existing in the same codebase is that they muddy the waters as to what the internal model the code was expressing even means.  These clarity issues create bugs and bad experiences.  

`Bounded Contexts` are natural cleaving places for separate services.  Doing so allows businesses to execute well in each of those contexts.

#### Scaling Concerns

As a business grows, and a product gets more traffic, it is natural for parts of the monolith to need different deployment profiles to meet the needs of the customers.  Deploying the whole monolith to service each of those profiles becomes more wasteful the bigger the monolith is.

Splitting a monolith based on deployment profiles allows the business to use infrastructure resource more efficiently, which adds natural value to each customer using those resources.

### How should the monolith be split?

So, after all of that, you've decided that you *need* to spit your monolith, here's a suggested path forward for you:

#### Define your new `Bounded Context`

Define your `Bounded Context` and the use-cases that will be that context's responsibilities.

One of the worst downsides to microservices are the need for distributed transactions, so do your very best to keep them to a minimum.  Consider the common use-cases that dip into your context, and make sure entities that are close collaborators in fulfilling those use-cases live together.  

#### Create a shim facade

In the monolith, create a facade that the rest of the monolith will use to interact with your newly formed `Bounded Context`.  This will act as a natural barrier between your context and the rest of the system, and will allow you to finally split the monolith later.

#### Refactor the code

Refactor all of the code outside your `Bounded Context` to talk to it via the shim facade.

When I do this refactor, I try to stay away from passing domain model entities through the facade.  Better to translate things to plain data beforehand.  This extra step means that it will be easier later to actually split, since there's little chance to cheat in the mean-time.

#### Build your microservice

Build your micro-service to the specifications defined by the shim facade.  

When you do this, it is really tempting to cheat and have a shared database or something.  Do not give in!  Discipline here is vial to the success of the project.

#### Re-implement the facade using the microservice

Re-implement the facade to use the external microservice, paying mind to handle all of the fun complications that come with calling external services.

#### Remove old code from monolith

At this point, you get to delete a bunch of code from the old monolith.  It'll be fun, trust me.

### Further Reading

- [Bounded Context](https://martinfowler.com/bliki/BoundedContext.html)
- [Microservice Architecture: Aligning Principles, Practices, and Culture](https://www.ca.com/content/dam/ca/us/files/ebook/microservice-architecture-aligning-principles-practices-and-culture.pdf)
