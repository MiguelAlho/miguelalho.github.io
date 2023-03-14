---
title: "Balancing Complexity in Software"
date: 2023-03-12T21:04:23Z
draft: true
---

> I tend to battle frequently with complexity in my software (and software I interact with). We tend to add multiple capabilities into the software components we build, especially at the application level, behind an many times poorly defined label of being "enterprise-ready", as if you need to check off all the boxes to get the seal of approval.

I'm not saying that applications cannot or should not be complex - hard problems generally require software that is complex to actually be useful; complex environments do too. It's just that we (or I?) don't want to exaggerate in that complexity. Complexity that can cripple the flow in the development process; Complexity that can sometimes bring it's own problems and bugs that nothing have to do with issues in the business process, and make you lose days trying to figure out what's going on.

# About complexity (in the context of this problem)

> Define complexity (accidental , germaine, intrinsic? - get TT definition)
    which one is in our problem

# Application / service complexity

Regardless of a monolith or microservice, we add complexity at many levels not only to make the service work but also keep it running:

* logging
* security functions
* control planes
* observation
* resiliency
* configuration (dynamism)
* encryption
* error management
* profiling
* Inversion of control container
* internal pub-sub
* Validation layers
* serializzation / deserialization layers
* Testability
* Documentation
* clients 

middleware vs injected, vs external environment capability

even in the build process

* package management
* cli / build system
* task management
* containerization


.Net landscape

* logging: Serilog , older NLog / log4net
* security functions - auth, jwts, CORS, etc...
* control planes - health check
* observation - Serilog (sinks) Otel
* resiliency - Polly
* configuration (dynamism) - Option<>
* encryption - 
* error management - Global handlers
* profiling - 
* Inversion of control container - MS DI,  Autofac, ninject, 
* internal pub-sub - Mediatr, Wolverine, 
* Validation layers - Fluent Validations, Data anotations, 
* serializzation / deserialization layers
* Testability - Nunit, Xunit, TestServer, 
* Documentation - Swagger / swashbuckle, Rest docs
* clients - SDKs, Restsharp, 
* Data access - ORMs (Entity Framework, Dapper, Nest)


# essential vs additional

Looking a the above, one can wonder what's REALLY needed - and what is an enhancement that may or may not produce value. 

How much do you really need / how much is YAGNI?
how much extra effort or boilerplate is required, vs drop in and use?
HOw much time do you spend solving the edge-cases in the external tools vs the actually business component