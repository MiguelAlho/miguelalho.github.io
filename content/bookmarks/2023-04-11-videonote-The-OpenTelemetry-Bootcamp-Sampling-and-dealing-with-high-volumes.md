---
title: 'The OpenTelemetry Bootcamp: Sampling and dealing with high volumes'
date: 2023-04-11T14:37:08.5582249Z
author: Miguel Alho
draft: false
url: /bookmarks/The-OpenTelemetry-Bootcamp-Sampling-and-dealing-with-high-volumes
imageFolderId: 1095281665414004736
article:
  author:
    name: ''
    twitter: ''
    linkedIn: ''
    website: ''
  links: []
video:
  source: youtube
  id: tb6VHrihPZI
  link: https://www.youtube.com/embed/tb6VHrihPZI
  author:
    name: Michael Haberman
    twitter: ''
    linkedIn: ''
    website: ''
  context:
    name: The OpenTelemetry Bootcamp Episode 4
    link: ''
tags: []
summary: ''
notes:
- id: 0
  type: Slide
  time: 05:55
  image: 1095281665414004736/1095282975286755328.png
  title: ''
  content: Kind of worried, as I heard "three pillars" being mentioned... but still, it's one of the common models used.
  comment: ''
- id: 1
  type: Quote
  time: 07:36
  image: ''
  title: ''
  content: 'Traces are something that are mostly automatic. '
  comment: ''
- id: 2
  type: Note
  time: 8:52
  image: ''
  title: ''
  content: >-
    Traces are expensive : there's a cost to CPU / Memory used, cost in data transfer to cloud provider, and cost in storage used to save them.


    The cost of the tooling (Jaeger) is not as relevant when compared to the above costs.
  comment: ''
- id: 3
  type: Note
  time: 11:50
  image: 1095281665414004736/1095312782225571840.png
  title: ''
  content: 'Trace sample percentage can / should be chosen based on use case. Cost analysis can help define percentage ranges. Different percentages can be used within the use case (example: 100% of errors but 10 of the rest)'
  comment: ''
- id: 4
  type: Note
  time: 16:49
  image: 1095281665414004736/1095315728531193856.png
  title: ''
  content: >-
    Head sampling : decide to keep sample when the span starts. SDK makes the decision, normally.


    Tail sampling: decide to keep sample when trace completes. Collector can make this choice.
  comment: ''
- id: 5
  type: Note
  time: 19:07
  image: ''
  title: ''
  content: >-
    Tail sampling is a part of `otel-collector-contrib_tail_sampling` package / plugin, added to the collector.


    When using tail sampling in the collector, samples are collected during a period of time (`decision_wait`), though that means it is subject to failure/trace loss if the collector fails (out of memory, etc)


    Interesting conditions to choose on:

    * latency

    * numeric attribute

    * probability

    * status code

    * string attributes

    * rate limiting
  comment: ''
- id: 6
  type: Slide
  time: 26:20
  image: 1095281665414004736/1095318127127822336.png
  title: ''
  content: >-
    Load balancer helps shred load, based on trace rules (think sticky session - same traceId to same collector)


    Then 2 layers of collectors:

    * first layer load balances and makes decisions on what to collect;

    * second layer serves as transport to final destination
  comment: ''
- id: 7
  type: Note
  time: 32:41
  image: 1095281665414004736/1095349023918784512.png
  title: ''
  content: Parent Based sampler - Service B respects Service A's decision of sampling or not.
  comment: ''
- id: 8
  type: Slide
  time: 49:04
  image: 1095281665414004736/1095353139919323136.png
  title: Optimizations
  content: ''
  comment: ''
- id: 9
  type: Slide
  time: 51:04
  image: 1095281665414004736/1095353645186154496.png
  title: Benchmarks
  content: Tail based requires more memory since it needs to buffer and decide late.
  comment: ''
- id: 10
  type: Slide
  time: 53:58
  image: 1095281665414004736/1095354376723103744.png
  title: ''
  content: You may need an extra layer if your target can't handle the load.
  comment: ''
- id: 11
  type: Slide
  time: 55:07
  image: 1095281665414004736/1095354662443286528.png
  title: ''
  content: 3 metrics to calculate cost.
  comment: ''
- id: 12
  type: Slide
  time: 56:02
  image: 1095281665414004736/1095354895071969280.png
  title: Cost Calculation
  content: >-
    730 = aprox. hours in a month


    Storage is probably the most expensive bit on the list.
  comment: ''
- id: 13
  type: Slide
  time: 59:49
  image: 1095281665414004736/1095355842435219456.png
  title: Prod tips
  content: ''
  comment: ''
content: ''

---
