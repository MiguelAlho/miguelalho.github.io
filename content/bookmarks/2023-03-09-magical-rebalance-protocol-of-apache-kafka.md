---
title: The Magical Rebalance Protocol of Apache Kafka
date: 2023-03-24T12:08:43.9806268Z
author: Miguel Alho
draft: false
url: /bookmark-magical-rebalance-protocol-of-apache-kafka/
imageFolderId: magical
tags:
- bookmark
- talk
video:
  source: youtube
  id: MmLezWRI3Ys
  link: https://www.youtube.com/embed/MmLezWRI3Ys
  author:
    name: Gwen Shapira
    twitter: ''
    linkedIn: ''
  context:
    name: StrangeLoop
    link: https://www.thestrangeloop.com/2018/sessions.html
summary: >-
  I've been debugging a weird and considered impossible situation on a Kafka cluster and/or consumer service. I have a multi-instance service where something occurs that causes one of the instances to get all the partitions assigned, though the partitions on the other instance do not get revoked. Theoretically, a partition can only be consumed by a single consumer in a group; in this case though, 2 consumers in the same group are consuming the same partition. This leads to concurrent processing on the same instance. Luckily, the consumers are idempotent, so they can still handle the situation, but it does generate a bunch of error events.


  This clip was recommended to me by colleague Douglas, to help understand better the rebalancing protocol.
notes:
- id: 0
  type: Slide
  time: 04:12
  image: magical/001.png
  title: ''
  content: >-
    Gwen gives a great overview of how Kafka is organized starting with Partitions. "Partitions are the unit of scalability".


    Replication was a knowledge gap for me. Every Partition is replicated 3 times for availability. One of the replicas is the leader, and events are written to the leader and consumed from the leader. Replicas copy events from the Leader and try to keep up. If the leader fails or dies,one of the replicas get's chosen as the new leader.


    As consumers read, they store their position (offset) on the `__consumer_offsets` topic. All the offsets go onto the same partition of the __consumer_offsets topic.
  comment: ''
- id: 1
  type: Slide
  time: 10:02
  image: magical/002.png
  title: ''
  content: >-
    These were the requirements for the new consumer group management protocol (third try), that replaces ZooKeeper.


    "Partition assignment must de defined by the clients, not by the brokers" was controversial. I would think in terms of central management naturally. The broker knows pretty much the same that the clients already know. Putting the management on the broker side would over restrict the clients.


    Something interesting is said, related to my current issue - "(you would say) the brokers will know if two clients are trying to consumer from the same partition, and we need to prevent that. But no. Who say's you NEED to prevent that? Maybe my application want's to have some kind of standby and have two clients consume the same partition."


    I take from that there are modes and controls in place that would allow the multiple consumers on the same partition. The idea of impossibility may be a legacy concept, then. I may be caught in what is inevitable on the web - outdated docs.
  comment: ''
- id: 2
  type: Quote
  time: 11:20
  image: 
  title: ''
  content: The hardest problem in computer science is to get two teams in the same company to work together.
  comment: Yup.
- id: 3
  type: Slide
  time: 13:53
  image: magical/003.png
  title: ''
  content: >
    The protocol is layered, and used in 4 cases 

    * consumer groups membership and partition assignment (base use case)

    * KafkaConnect uses it to assign tasks to workers

    * KafkaStreams for assigning partitions to tasks to hosts

    * Schema registry for leader election
  comment: ''
- id: 4
  type: Slide
  time: 15:32
  image: magical/004.png
  title: ''
  content: >
    For the consumer group use case, Consumers want to know which partitions they can consume.


    On the broker side are coordinators. 

    * Any broker can be a coordinator

    * Every broker is a coordinator for a subset of Consumer Groups

    * A group only talks to a single coordinator at any given time

    * The coordinator facilitates the communication between consumer groups.

    The first thing a consumer group does is find it's coordinator, using the only call (request/response) in the protocol that can be made to any of the brokers - `FindCoordinator` . It's like "I am a member of group X, who is my coordinator?".
  comment: ''
- id: 5
  type: Slide
  time: 20:40
  image: magical/005.png
  title: ''
  content: >-
    Once a consumer has found a coordinator, it will attempt a `JoinGroup`.


    * First call to `JoinGroup` by consumer has an empty memberId string

    * First consumer to join the group is the leader of the group

    * Consumers in a linked list, so if leader disappears, next one is leader

    * Consumers communicate the Assignment Protocols they prefer to use during that call
  comment: ''
- id: 6
  type: Slide
  time: 18:06
  image: magical/006.png
  title: ''
  content: >-
    During a `JoinGroup`, the coordinator looks at the requested protocols and determines which one to use based on a weighted voting mechanism (most requests of a protocol wins).


    When any consumer does a `JoinGroup`, because protocol preferences can change (suc as in an upgrade, where a new preference is communicate), all the consumers do a * JoinGroup` (or is it a `SyncGroup`?) where they'll get the updated meta info back.
  comment: ''
- id: 7
  type: Note
  time: 23:09
  image: 
  title: ''
  content: >-
    Other methods

    * `SyncGroup` returns the metadata and assignments to consumers

    * `Heartbeat` allows consumers to say "don't forget me, I'm still here`

    * `LeaveGroup` gives the consumer a chance to elegantly get out.
  comment: ''
- id: 8
  type: Slide
  time: 26:30
  image: magical/007.png
  title: ''
  content: >-
    Rebalancing is a key activity. Rebalance occurs on -

    * missing heartbeat for a consumer after a long time

    * member leaves

    * new member joins

    * changes on topics and subscriptions


    There's constant polling which helps understand what's going on. It's critical that commits and consumption stop during rebalance as that can have really bad outcomes.
  comment: ''
- id: 9
  type: Note
  time: 36:30
  image: magical/008.png
  title: ''
  content: >-
    __A clue__ ":" as part of the future plans, (and considering the presentation is 5 years old) these might be in play ":"


    * Clients can keep working if they are using Sticky Assignment protocol. Might this be why one instance keeps consuming even though another has been assigned all the partitions?

    * If rebalance does not stop every consumer, can this also be one of the reasons the issue occurs?
  comment: ''
---
