---
title: Introduction to Context Mapping - Michael Plöd - DDD Europe 2022
date: 2023-03-24T15:56:04.0544277Z
author: Miguel Alho
draft: false
url: /bookmarks/Introduction-to-Context-Mapping
imageFolderId: intro-context-mapping
article:
  links: []
video:
  source: youtube
  id: k5i4sP9q2Lk
  link: https://www.youtube.com/embed/k5i4sP9q2Lk
  author:
    name: Michael Plod
    twitter: bitboss
    linkedIn: ''
  context:
    name: DDD Europe 2022
    link: ''
tags: []
summary: This is a really awesome intro into Context Mapping, if your getting started or even if you need a refresher.
notes:
- id: 0
  type: Slide
  time: 3:10
  image: intro-context-mapping/1088838045194518528.png
  title: ''
  content: These are things we are looking to get our teams to achieve. The Bounded Context helps achieve that.
  comment: ''
- id: 1
  type: Slide
  time: 00:00
  image: intro-context-mapping/1088838246357532672.png
  title: ''
  content: The ddd-crew github repo has a lot of tools, like the DDD starter Modelling Process to get you started
  comment: '[github link](https://github.com/ddd-crew/ddd-starter-modelling-process)'
- id: 2
  type: Quote
  time: 4:28
  image: ''
  title: ''
  content: A Bounded Context is a boundary for a model expressed in a consistent (ubiquitous) language tailored around a specific purpose.
  comment: ''
- id: 3
  type: Note
  time: 5:10
  image: ''
  title: ''
  content: >-
    A Bounded Context is a boundary. It has it's own language within the boundary. There is a purpose to it, too - Reusability is not key concern. Models are specific.


    It can also be seen as team first boundary (Team Topologies space).


    The bounded context does not show the dynamics between contexts (nor between teams).
  comment: ''
- id: 4
  type: Quote
  time: 8:40
  image: ''
  title: On Conway's Law
  content: Conway's law is not about the org structure on the chart - it's about the dynamics in that org chart. It's about the communication structure.
  comment: ''
- id: 5
  type: Slide
  time: 9:20
  image: intro-context-mapping/1088840506160447488.png
  title: ''
  content: ''
  comment: ''
- id: 6
  type: Note
  time: 10:15
  image: ''
  title: ''
  content: Context Maps are a bit of a hidden gem in the DDD space, since most look at the Tactical patterns immediaely.
  comment: ''
- id: 7
  type: Slide
  time: 12:09
  image: intro-context-mapping/1088841483970150400.png
  title: ''
  content: >+
    We're used to communicating between services using REST API synchronous call or consuming events. But we can see other types of communication. For instance, the propagation of models based on the messages passed back and forth is an effect. There is always some type of coupling.

  comment: ''
- id: 8
  type: Slide
  time: 15:10
  image: intro-context-mapping/1088842798355972096.png
  title: ''
  content: "Context Maps address a few concerns:\n\n* Dependencies between components and teams\n  * Mutually Dependent\n  * Free\n  * Upstream / Downstream (reflects on power dynamics: Upstream team normally has power over downstream teams)\n\n  "
  comment: ''
- id: 9
  type: Slide
  time: 19:10
  image: intro-context-mapping/1088843287634116608.png
  title: ''
  content: There are 9 patterns.
  comment: ''
- id: 10
  type: Quote
  time: 19:45
  image: intro-context-mapping/1088843451019034624.png
  title: Open-host Service
  content: One API for several consumers. API provider (Open host provider) is generally Upstream. Rare cases where the Open-host service (power control) is downstream is in regulator related scenarios.
  comment: ''
- id: 11
  type: Quote
  time: 22:15
  image: intro-context-mapping/1088844323962748928.png
  title: Anti-Corruption Layer (ACL)
  content: >-
    ACL is basically a model-to-model transformation. Downstream team uses ACL to transform external model into it's own.


    Be aware that is costs time and money.
  comment: ''
- id: 12
  type: Slide
  time: 24:40
  image: intro-context-mapping/1088844699818524672.png
  title: Conformist (CF)
  content: Conformist just takes and consumes upstream model
  comment: ''
- id: 13
  type: Slide
  time: 25:15
  image: intro-context-mapping/1088845221506056192.png
  title: On Coupling
  content: >
    ACL is not about decoupling (because you can't) - it's about loose-coupling.

    Conformist is deep coupling
  comment: ''
- id: 14
  type: Quote
  time: 27:00
  image: intro-context-mapping/1088845342260068352.png
  title: ''
  content: There are heuristics towards choosing Conformist. It does reduce effort when choosen.
  comment: ''
- id: 15
  type: Slide
  time: 31:00
  image: intro-context-mapping/1088846309927944192.png
  title: Shared Kernel
  content: >-
    Shared Kernel physically shares an artifact. JARs, DLLs, DBs. It's considered an Anti-Pattern.


    If you want to autonomy, you want to avoid Shared Kernel.
  comment: ''
- id: 16
  type: Slide
  time: 33:10
  image: intro-context-mapping/1088846743912579072.png
  title: ''
  content: There's heuristics for it too. It can be a trade-off. Avoid it, for sure, if teams are in a competitive nature - it becomes toxic (politics).
  comment: ''
- id: 17
  type: Slide
  time: 34:35
  image: intro-context-mapping/1088847237653463040.png
  title: Partnership
  content: Needed for shared kernels - establishes cooperative relationships.
  comment: ''
- id: 18
  type: Slide
  time: 37:30
  image: intro-context-mapping/1088847903998345216.png
  title: Customer - Supplier
  content: "Upstream - downstream relationship. Customer (downstream team) has **some** influence granted on the Supplier (upstream team) planning. \n\nIt's organizational - doesn't show up in code. "
  comment: ''
- id: 19
  type: Slide
  time: 44:00
  image: intro-context-mapping/1088849564695265280.png
  title: Seperate Ways
  content: It's a Free-style relationship. Very expensive.
  comment: ''
- id: 20
  type: Slide
  time: 45:35
  image: intro-context-mapping/1088849969093279744.png
  title: Published Language
  content: >-
    Standardized language in most cases. Creates independence between teams. Often combined with OHS.


    We'd expect that an OHS have a well documented interface and a well documented / defined language.
  comment: ''
- id: 21
  type: Slide
  time: 49:45
  image: intro-context-mapping/1088851036447178752.png
  title: Big Ball of Mud (BBoM)
  content: it's a warning sign; You want ACLs against these.
  comment: ''
- id: 22
  type: Slide
  time: 51:30
  image: intro-context-mapping/1088851475838271488.png
  title: ''
  content: ''
  comment: ''
- id: 23
  type: Slide
  time: 52:00
  image: intro-context-mapping/1088851546474545152.png
  title: ''
  content: Describes communication patterns too
  comment: ''
- id: 24
  type: Quote
  time: 54:40
  image: intro-context-mapping/1088852243328794624.png
  title: ''
  content: Affects domain types, too
  comment: ''
content: ''

---
