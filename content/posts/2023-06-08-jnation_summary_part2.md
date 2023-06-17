---
title: "JNation Takeaways- Part 2"
date: 2023-06-17T00:04:23Z
draft: true
---

I had the opportunity of checking out the [JNation 2023 conference](https://jnation.pt/) in Coimbra for the first time this past week. It's a really great conference, well run and affordable. The team is clearly experienced in setting up the activities and interactions. And, even though I'm not a Java dev, I was still able to get a ton out of it as the diversity in talks had a lot I could consume.

I generally tend to take some notes for future reference, and I'm going to try to summarize my experience. I've listed the talks I saw, and embedded the videos with some of my notes ans takeaways, and quotes.

# The Influential Software Engineer -  Nate Schutta (Keynote)

{{< youtube id="Sq9aqieVlD8?t=2364" >}}

Culture is a key element in software development. I've found myself many times reminding folks that the systems we develop are not just technical - they are "social-technical". People are always involved, whether it's the team building it or the final user.

Nate covered a lot of this in his keynote - how culture is formed, how it's evolved, how we influence it, and a lot of examples of what is part of a good software development culture - with great stories and references to back it up.

> Culture is where good ideas go to die

> The six most dangerous words: "That's always how we've done it"

> "You're not going to JIRA you're way into culture change"

New ideas are important, but bringing them in requires change and changing people and orgs is one of the hardest problems to solve - people are typically adverse to change. It take's influencing, it takes poking. It takes **Time**. You need to insist and you need to learn the "soft" skills that will make you more effective at influencing the cultural change. You need to model it, champion it, or help those more influential in the org champion the change - either make them think it's their idea or influence the influencers.

> You don't add innovation to culture, you get out of it's way.

> Best practices are fantastic, unless they are ignored

Change is incremental. Small increments of improvements, applied frequently, compound. It doesn't have to be big bang. Regardless, it's not easy - to influence those changes, you still need to "get out of the technical comfort zone". It's always a "people" problem. Many of us, in this profession, are not equipped for the needed skills but it's important to recognize that those skills are learnable. I know I've certainly had to focus on many of those skills to improve to be even a little effective (and there's still a never ending journey ahead of learning). The work is not just code - it also conversations and interactions. We need to learn how to deal with people, with groups. We need to learn how to make those around us better, too, so we ourselves can further improve a step further. We need to learn and practice the so called "soft skills".

> The Technology is easy. It's the culture that's hard.

A wonderful talk, full of great bits to extract, and absolutely shareable across all roles in an org.

# Empowering Developers to Embrace Security - Brian Vermeer

{{< youtube id="4yIkYQqKQv4?t=2025" >}}

Brian has interesting talk regarding application security and common vulnerabilities, and more importantly, how to help change the organization so that security concerns are surfaced and integrated into the development process.

![problems](/posts/20230608-jnation/20230608Jnation001.png)

Some code examples were given, both common and that a code scanner can find and help you fix, and more difficult ones that are context specific like a leak based on the data mapping and association. RCEs are always fun to see live.

Code Reviews are not enough for finding security issues - it's a common element in our process for "ensuring" code quality, but it's human dependent and therefore error prone.

My experience in reviewing - it's hard. You *can* catch a lot of issues correctly in the review. We expect seniority to be a key to quality in the review process, but it's not reliable. Longer change sets are more prone to nitpick analysis, and impossible to mentally work through with content overload. Plus, seniority is not a guarantee that I've seen "all the things". In my case, I have seen some, and will recall a few of them in the review, but I only know what I know. The cases and issues I'm not aware of will get through. Even with training it can be VERY hard.

Also, ever notice no one is really trained in code review? Formal, structured training? Doesn't exist. Code review experience is mostly built on habit, team level convergence of things to look for, and looking for things that were caught in your own reviews. If you haven't been through thorough reviews, haven't ever seen thorough reviews, you won't know what it looks like and will have a hard time reviewing thoroughly.

So, tooling helps, and can cover some of the cases. For sure, more than a human can, but not what is context specific. Still, it can offload effort and allow the human reviewer focus on the complex conditions.

A concept that I liked from the talk and can help bridge the security awareness and knowledge gap is the Security Champion Program. It can help share and embed the security team's knowledge into the development team. It's still dev focused, and devs can proxy and work with security teams to scale the specialized knowledge. This can help fix the silos and the culture regarding security knowledge.

![problems](/posts/20230608-jnation/20230608Jnation002.png)

OWASP has some [guidance](https://owasp.org/www-project-security-culture/v10/4-Security_Champions/) in this area, too.


# LLMs

I'm an absolute noob in this field so the talk wasa great help to get a grip on some of the concepts and terms. 

At first I was a bit scared, since the description didn't seem to match the talk structure, but still great intro

Hoistory and evolution, the problems overcome at each step and resolution of previos. clearly iterative; imagie the path forward

Hallucination appears as the major problems

Tranformers as a key element

Alpaca and OSS projects; hugging face model repo



# LLMs2 - Semantic Search

Phillip Krenn is a favorite. PRevious interactions with great talks at DDD. Excelent speaker so I knew I was going to pick stuff up

Good explenation of how Elastic and semantic search works under the hood, and how it's being transformed with vector search integration.  Nearest Neighbor algo (kNN) 

Improvements derived from combining the searhced - scope to semantic search results and enhance with vector search results; integrate with LLM to explain and summarize results

Once again size of search space and dimensions is key challenge and limitation



# REST

Church is amazing for talks, and this one seemed ok. hit a bit of cognitive disonance with me. Also got distracted with some work related messages.

Couple of keys I got out:
?

# observability

Not into spring boot 3, but I have been working with setting up some Otel collectors lately. Nice to finally look at the combination of Grafana Loki and Tempo and how you jump across things.

Speaker should how things can get recorded - still a buch of work building decorators with the Micrometer Observaility libraries. AOP felt like a nicer way.
Having to buidl so much per activity / observation felt "wrong" or overkill - though I trully don't know a better way.



# Automation

Interesting ideas and a lot of work coming out of sonatype to help protect us, using AI. I kind of expected a different thread of focus, but speaker's energy was high and I still maintained engagement. Bom doctor tool sounds interesting and useful.


# Conference and venue

- Amazing venue - supprised we don't se more there. Country center deserves more activities - venue was full.
- fun cosplay
- gems trade in was excellent idea
- 

