---
layout: post
title: "Vision & RFC Based Workflow"
date: 2021-09-25
tags: ["development", "SDLC"]
---

Wherein I'll talk about my preferred method of turning product requirements into working code.  

A common theme I've seen in my days is the ability for a dev shop to be able to handle both the high level (quarter to quarter) view of what needs to be done along with handling the sprint level blocking and tackling well, while having problems bridging the gap between those different realms.  Below I'm gonna sketch out how I've had success doing things in the past to glue those two realms together.

The process here revolves around one Vision document, and a set of RFCs, used together to define the product (or section of product), and how that will be implemented in code.

## Firstly, Don't Sweat the Small Stuff

> Plans are worthless, but planning is everything.
>
> -- Gen Dwight D. Eisenhower

The point of any planning process is that the process of planning is more important than the actual documents.  Writing things down, especially in a collaborative editing system (wiki or google docs) will help center the teams thoughts and dig out any known unknowns, and unknown knowns.

Seriously though, don't sweat the small stuff here, you won't know everything till everything is done.

## The Process

To help illustrate matters, we'll be using a made-up scenario as examples for each step.

First start with a business problem and a goal metric to target.  If your organization does OKRs, these would be those. Example:

<p class="message">
We are a consumer facing e-commerce website.  Looking at user metrics and such, we see lots of customers try many times to login, and eventually give up after a while.  Seeing as how they had items in their cart when they gave up, we want to help matters and get those people buying our widgets.  Our login on first try rate: ~60%.  We would like to get that number up to at least 70%
</p>

A project idea to tackle the OKR is ideated upon by the team, and buy-in is gotten.

<p class="message">
The product manager, along with a UX expert, thinks that maybe allowing other login method other than passwords, the company could get those users logged in without them having to remember a password.  They pitch the idea to the rest of the team and get buy-in.
</p>

With the team in agreement that this project is a good one, a Vision document for the project is spun-up by the Project Manager:

<p class="message">
The project is defined as allowing users to have multiple ways to login, and preferring non-password login methods.  Since this is a big project in the whole, with many login types available, the minimum viable experience is defined as the ability to register and login via webauthn.  
</p>

The Vision document is then handed over to engineering, who start an investigation and come up with a candidate solution.  An RFC (or several, depending on the complexity of the solution) is then written up.

The dev team as a whole should get together into a meeting to discuss the RFC (preferably called a `Thunderdome`) and give feedback on the doc.

Once the RFC gets buy-in from the devs, Tasks (or stories, or whatever) are created to be scheduled according to priorities.  

<p class="message">
One of the devs is tasked with fleshing out a candidate solution.  They write up an RFC.  The dev team as a whole reviews the RFC, and after a few tweaks, the RFC is deemed good.  Stories are created in the work tracker, and are scheduled for a sprint based on priorities.
</p>

The project gets shipped, and based on feedback to the project, the vision should be updated and more RFCs created, etc, etc.

## The Documents

Below is the outline of both of the documents used in the process.  The Vision document should be treated as a "living" document and updated as needed.  The RFCs are more of a work product, and will necessarily get out-of-sync with the code as the code is updated.  

### Vision Document

The Vision Document is used to define goals from the view of the project.  This will help the whole team to understand the scope of project, what success will be measured against, and how the end-user is expected to interact with the product.

A good Vision Document should include:

- An overview of the project in total.  What is the expected end-state.
- What does the minimum viable experience looks like.
- How will you measure success.

In a bigger organization, adding a list of who is ultimately responsible for the project from a technical and product management perspective is helpful too.

Once the vision document has buy-in from the team and organization, there should be an investigation spun off by the developers around the architecture of a candidate solution.  I call these RFCs, as is tradition.

### RFC Document(s)

RFCs are used by the engineering folks to come up with a candidate solution to fulfill the project's goals.

If the project as a whole is too big to reasonably be done in one RFC, create an RFC for the minimum viable experience, and then backlog tasks to iterate to the full solution.

A good RFC should include:

- A quick overview of the project / subset of the project it covers
- A link to the project vision
- An overview of the candidate solution, in reasonable detail.  Sequence Diagrams are highly encouraged.
- A list of steps that would need to be taken to implement the solution (stories / tasks will be generated based on these steps)
- A list of open questions for discussion
- A list of links to references used during the writing of the RFC

Since RFCs will be reviewed by the team as part of the process, these are great opportunities for more junior developers to exercise their design muscles and get feedback on their ideas.  The best way to learn to design systems, is to practice doing it.
