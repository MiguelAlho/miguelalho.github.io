---
title: Delivering Changes for Applications and Databases
author: Miguel Alho
type: post
date: 2016-08-04T22:35:37+00:00
url: /delivering-changes-for-applications-and-databases/
tags:
  - 'talk'

---
I had a chance to do a presentation with my good friend <a href="http://www.eduardopiairo.com/" target="_blank">Eduardo Piairo</a> on &#8220;Delivering Changes for Applications and Database&#8221; at the monthly <a href="http://www.portodata.net/" target="_blank">PortoData </a>meeting. This was a problem we worked on quite a lot at Celfinet, the client whom I worked for previously, where we met.

Delivering changes on apps that we want to be able to work on and iterate on fast require an interesting deal of automation, achievable through migrations, automated tests, CI, and CD. We talked about the shared database pattern and the problems we faced with it, solutions to mitigating that problem with source control and migrations, and how moving to more of a microservice strategy helped in new services. We talked about the common tools we used, like <a href="https://flywaydb.org/" target="_blank">Flyway</a>, to perform the migrations.

I demoed some aspects from the dev / app side of things, and showed off a simple migrations and integration test strategy, using <a href="https://dbup.github.io/" target="_blank">DbUp </a>and LocalDb, to aid devs on performing db changes fearlessly throughout the product&#8217;s iterations.

The code repo is at <a href="https://github.com/MiguelAlho/Purchases-DbMigration-sample" target="_blank">https://github.com/MiguelAlho/Purchases-DbMigration-sample</a>, and I just updated the readme to reflect how the code evolves, as in the demo.

The slides are hosted at slideshare ( <a href="http://www.slideshare.net/MiguelAlho/delivering-changes-for-applications-and-databases" target="_blank">http://www.slideshare.net/MiguelAlho/delivering-changes-for-applications-and-databases</a>):

[slideshare id=64573071&doc=deliveringchangesforapplicationsanddatabasesv21-160801092836]