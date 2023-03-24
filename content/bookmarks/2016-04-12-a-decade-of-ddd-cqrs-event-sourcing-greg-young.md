---
title: A Decade of DDD, CQRS, Event Sourcing (Greg Young)
date: 2023-03-24T18:07:44.2311369Z
author: Miguel Alho
draft: false
url: /a-decade-of-ddd-cqrs-event-sourcing-greg-young/
imageFolderId: decade-of-ddd
article:
  links: []
video:
  source: youtube
  id: LDW0QWie21s
  link: https://www.youtube.com/embed/LDW0QWie21s
  author:
    name: Greg Young
    twitter: ''
    linkedIn: ''
  context:
    name: ''
    link: ''
tags:
- bookmark
- talk
summary: I always enjoy watch Greg Young&#8217;s presentations. This one, from [DDD Europe 2016](http://dddeurope.com/2016/greg-young.html), is very interesting as a retrospect of the changes DDD, CQRS and Event Sourcing have permitted in how modeling problems are attacked and solved with these techniques, and what we can expect in the future.
notes:
- id: 0
  type: Note
  time: 05:50
  image: ''
  title: ''
  content: >
    Getting people onto CQRS / Event Sourcing can be hard based on people's experience and past knowledge. If all they've known is ORMs on a relation database, the leap to thinking in events can be hard. Same as the jump to functional for anyone who's only done OOP.  


    CQRS is really a "stepping stone" into Event Sourcing. It's a valuable pattern, but not the end goal.


    This implies that empathy with whom you collaborate is required. Junior eng. jumping into an existing codebase will be challenged without the needed hand-holding and guidance on how to get into the right mindset.  
  comment: ''
- id: 1
  type: Quote
  time: 10:43
  image: ''
  title: ''
  content: But something else has been happening that is really cool... ()...) they've gone through in many domains and actually had breakthroughs in their domains.
  comment: Changing your way of thinking (and in this case, applying a new pattern or style may be forcing you to change your thinking mode), can cause you to see things in different ways and get a better understand of how the domain really works.
- id: 2
  type: Note
  time: 10:45
  image: ''
  title: ''
  content: >-
    The warehouse system example has been stuck with me for a few years. I haven't successfully applied it (it's hard to change how people think). In any case, this example has so many points of interest it's eye opening.


    First, the idea of the "source of truth" - we get so stuck in defining the software as the source of truth, when in reality the source of truth is always "the real world". As a corollary, in some cases, the "proxy" source of truth is actually another piece of software, when all we can do is interact with a third party system. In many cases we cannot ensure good data, because the system cannot guarantee it.


    Second, the idea of an exception report. It can be used in so many realms for improving quality. The software of record may not be able to generate or ensure valid inputs due to the many uncontrolled aspects of it, but it can help users find the irregular situations and help "correct" that data (in the case of an event sourced system, with a new event ensuring the correction is an entry).


    "It's a different perspective, that also changes how a domain expert looks at a domain"
  comment: ''
- id: 3
  type: Quote
  time: 12:45
  image: ''
  title: ''
  content: >-
    (modeling events) ... Domain experts coming from a legacy system tend to think in terms of their legacy system as opposed to thinking about their domain problem. Once you start modeling events, it forces you to think about a behavioral version of that system as opposed to a structural version of that system and what the data that it stores is.in that system represents.


    More importantly,... it absolutely forces you to have a temporal focus about what happens within the system. Time becomes a crucial factor of your system. (order) becomes a domain problem.
  comment: ''
- id: 4
  type: Quote
  time: 17:00
  image: ''
  title: ''
  content: Event sourcing is naturally functional
  comment: ''
- id: 5
  type: Note
  time: 18:00
  image: ''
  title: ''
  content: >
    We're seeing the rise of Event sourcing at the same time as the rise of other ideas, somewhat in tandem, as there are aspects of each that are interrelated

    * functional programming (has all the functions needed for pattern matching and left folds, natively)

    * actor models 

    * immutable infrastructure 

    * microservices


    Even some ideas from event sourcing are applied in other technologies like Flux and Kafka
  comment: ''
- id: 6
  type: Quote
  time: 22:00
  image: ''
  title: ''
  content: Once you start dealing with immutable events, you need to start thinking about things like corrections
  comment: ''
- id: 7
  type: Quote
  time: 22:00
  image: ''
  title: ''
  content: (on applying event sourcing everywhere) ... This is a really really bad idea. You want to apply it selectively, only in a few places. ... As a rule., you really don't want to event source everything. Event sourcing and CQRS are not top-level architectures.
  comment: ''
- id: 8
  type: Quote
  time: 28:15
  image: ''
  title: ''
  content: There's no such thing as a one-way command
  comment: ''
- id: 9
  type: Note
  time: ''
  image: ''
  title: ''
  content: "Some of the pitfalls many people fall into or bad things that have happened\n\n* Trying to apply ES everywhere / everything instead of selectively\n* Dogma \n\n  * mutable / immutable Value Objects, \n  \n  * Process managers and Sagas \n  \n  * Can write side read from read side? \n  \n  * inputs = outputs i.e. it's not necessarily one to one,\n  \n  * one-way commands (i.e. it's not fire and forget)\n\n  * not recognizing you may not be the \"book of record\" / source of truth\n\n* Frameworks (i.e. people still trying to build a ES framework, like every other one has become)\n* Use of naive / simple examples, when real business problems are way more complex and where the real value are.\n* Lack of process managers - systems are much harder understand when you have to look at all the systems to see how they are connected\n"
  comment: ''
- id: 10
  type: Note
  time: ''
  image: ''
  title: ''
  content: >
    Future focus -


    * More functional programming 

    * More push on Process managers

    * Actors / actor models

    * Lack of linearization (global ordering of events), applicable to occasionally connected systems. (though you should linearize if you can because it simplifies the system)

    * git style work models (to help offline access situations)

    * N-temporal models (there may be multiple timelines)

      * "as-at" vs. "as-of" querying
  comment: ''
content: ''

---
