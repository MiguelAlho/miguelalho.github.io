---
title: 'A Decade of DDD, CQRS, Event Sourcing (Greg Young)'
author: Miguel Alho
date: 2016-04-12T11:42:10+00:00
url: /a-decade-of-ddd-cqrs-event-sourcing-greg-young/

video:
  source: youtube
  id: LDW0QWie21s
  link: https://www.youtube.com/embed/LDW0QWie21s
  author:
    name: Greg Young
  context:
    event: DDD Europe
    
tags:
  - bookmark
  - talk


summary:
    I always enjoy watch Greg Young&#8217;s presentations. This one, from [DDD Europe 2016](http://dddeurope.com/2016/greg-young.html), is very interesting as a retrospect of the changes DDD, CQRS and Event Sourcing have permitted in how modeling problems are attacked and solved with these techniques, and what we can expect in the future.

notes:
  - type: note
    time: 05:50
    content: 
        Getting people onto CQRS / Event Sourcing can be hard based on people's experience and past knowledge. If all they've known is ORMs on a relation database, the leap to thinking in events can be hard. Same as the jump to functional for anyone who's only done OOP.  


        CQRS is really a "stepping stone" into Event Sourcing. It's a valuable pattern, but not the end goal.
    
    
        This implies that empathy with whom you collaborate is required. Junior eng. jumping into an existing codebase will be challenged without the needed hand-holding and guidance on how to get into the right mindset.  

  - type: quote
    time: 10:43
    content: 
        But something else has been happening that is really cool... ()...) they've gone through in many domains and actually had breakthroughs in their domains.  
    comment: 
        Changing your way of thinking (and in this case, applying a new pattern or style may be forcing you to change your thinking mode), can cause you to see things in different ways and get a better understand of how the domain really works. 

  - type: note
    time: 10:45
    content: 
        The warehouse system example has been stuck with me for a few years. I haven't successfully applied it (it's hard to change how people think). In any case, this example has so many points of interest it's eye opening.


        First, the idea of the "source of truth" - we get so stuck in defining the software as the source of truth, when in reality the source of truth is always "the real world". As a corollary, in some cases, the "proxy" source of truth is actually another piece of software, when all we can do is interact with a third party system. In many cases we cannot ensure good data, because the system cannot guarantee it.


        Second, the idea of an exception report. It can be used in so many realms for improving quality. The software of record may not be able to generate or ensure valid inputs due to the many uncontrolled aspects of it, but it can help users find the irregular situations and help "correct" that data (in the case of an event sourced system, with a new event ensuring the correction is an entry).


        "It's a different perspective, that also changes how a domain expert looks at a domain"

  - type: quote
    time: 12:45
    content: 
        (modeling events) ... Domain experts coming from a legacy system tend to think in terms of their legacy system as opposed to thinking about their domain problem. Once you start modeling events, it forces you to think about a behavioral version of that system as opposed to a structural version of that system and what the data that it stores is.in that system represents.  
        

        More importantly,... it absolutely forces you to have a temporal focus about what happens within the system. Time becomes a crucial factor of your system. (order) becomes a domain problem.

  - type: quote
    time: 17:00
    content: 
        Event sourcing is naturally functional

  - type: note
    time: 18:00
    content: 
        We're seeing the rise of Event sourcing at the same time as the rise of other ideas, somewhat in tandem, as there are aspects of each that are interrelated
        
        * functional programming (has all the functions needed for pattern matching and left folds, natively)

        * actor models 

        * immutable infrastructure 

        * microservices


        Even some ideas from event sourcing are applied in other technologies like Flux and Kafka
    
  - type: quote
    time: 22:00
    content: 
       Once you start dealing with immutable events, you need to start thinking about things like corrections
    
  - type: quote
    time: 22:00
    content: 
      (on applying event sourcing everywhere) ... This is a really really bad idea. You want to apply it selectively, only in a few places. ... As a rule., you really don't want to event source everything. Event sourcing and CQRS are not top-level architectures.
    
  - type: quote
    time: 28:15
    content: 
      There's no such thing as a one-way command
    
  - type: note
    content: 
      Some of the pitfalls many people fall into or bad things that have happened


      * Trying to apply ES everywhere / everything instead of selectively

      * Dogma 
      
        * mutable / immutable Value Objects, 
        
        * Process managers and Sagas 
        
        * Can write side read from read side? 
        
        * inputs = outputs i.e. it's not necessarily one to one,
        
        * one-way commands (i.e. it's not fire and forget)

        * not recognizing you may not be the "book of record" / source of truth

      * Frameworks (i.e. people still trying to build a ES framework, like every other one has become)

      * Use of naive / simple examples, when real business problems are way more complex and where the real value are.

      * Lack of process managers - systems are much harder understand when you have to look at all the systems to see how they are connected

  - type: note
    content: 
      Future focus -


      * More functional programming 
      
      * More push on Process managers

      * Actors / actor models
      
      * Lack of linearization (global ordering of events), applicable to occasionally connected systems. (though you should linearize if you can because it simplifies the system)

      * git style work models (to help offline access situations)

      * N-temporal models (there may be multiple timelines)

        * "as-at" vs. "as-of" querying
---