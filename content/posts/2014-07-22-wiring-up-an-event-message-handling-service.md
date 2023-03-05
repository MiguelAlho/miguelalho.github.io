---
title: Wiring up an event-message handling service
author: Miguel Alho
type: post
date: 2014-07-22T13:25:11+00:00
url: /wiring-up-an-event-message-handling-service/
---
Recently, in a project I&#8217;m working on, I needed to create a service that would allow me to monitor what was going on in the application. In this case, log file info wasn&#8217;t rich enough for the type of analysis required. We wanted a contextualized view of what was going on at any moment within this application, but also to measure key aspects of its use. In a sense, it would be an implementation of some of the control patterns referred to in [Architecting Enterprise Solutions: Patterns for High-capability Internet-based Systems (Wiley Software Patterns Series)][1]<img style="border: none !important; margin: 0px !important;" src="http://ir-uk.amazon-adsystem.com/e/ir?t=mytymykysphot-21&l=as2&o=2&a=0470856122" alt="" width="1" height="1" border="0" />  
, such as the System Monitor.

Lately I&#8217;ve been looking at and quite interested in <a title="Fowler's CQRS bliki entry" href="http://martinfowler.com/bliki/CQRS.html" target="_blank">CQRS</a>. I like the concept and what it offers, especially when used with event sourcing. Unfortunately, I haven&#8217;t been able to apply it for many reasons, namely lack of experience with it and time to focus on learning how to do it, especially considering the need for features in a time constrained production application. For this particular case, though, I considered I could use some of the concepts to get to a solution that&#8217;ll do what I want.

<!--more-->

## The Requirement

I currently have an application that handles a specific business domain. I want to capture information about what is going on within the application &#8211; what business processes are running, etc &#8211; and also capture usage statistics &#8211; processing time, number of calls, etc &#8211; in order to identify bottlenecks and allows allow managers to focus our team&#8217;s sprints on the most used aspects (especially if there are problems with them).

Also, I find it very important not to pollute my business logic code with monitoring concerns &#8211; I believe the business logic code should be clean and simple and as close to the domain as possible. Therefore, I consider that capturing this information should be somehow connected to an application-level service external do the domain code (possibly inserted through IOC-based interceptors), and the processing of this information should be in an external process.

The next diagram provides an overview of what I want:

<div id="attachment_1850" style="width: 710px" class="wp-caption aligncenter">
  <a href="http://www.miguelalho.pt/wp-content/uploads/2014/07/EventHandlers_highlevel-e1405450575190.png"><img aria-describedby="caption-attachment-1850" class="size-large wp-image-1850" src="http://www.miguelalho.pt/wp-content/uploads/2014/07/EventHandlers_highlevel-e1405450575190-1024x325.png" alt="High level design / context blocks" width="700" height="222" srcset="http://www.miguelalho.pt/wp-content/uploads/2014/07/EventHandlers_highlevel-e1405450575190-1024x325.png 1024w, http://www.miguelalho.pt/wp-content/uploads/2014/07/EventHandlers_highlevel-e1405450575190-300x95.png 300w, http://www.miguelalho.pt/wp-content/uploads/2014/07/EventHandlers_highlevel-e1405450575190.png 1177w" sizes="(max-width: 700px) 100vw, 700px" /></a>
  
  <p id="caption-attachment-1850" class="wp-caption-text">
    High level design / context blocks
  </p>
</div>

CQRS includes a concept of event-handling mechanisms that can populate read models. Event processing services handle domain-emitted events, and use that event information to update a model designed  to match the presentation layer&#8217;s requirements. In my case, I&#8217;m considering capturing events (both domain-specific and application-specific depending on what I need) that my application publishes . These events will be represented as messages passed through a communication channel, with an endpoint that could be a web service (RPC style) or a subscriber on a message bus. The subscribing endpoint is a new context where the event messages will be consumed and processed to update the read model. The read model will be designed to optimize the reads by a GUI for dashboards and/or reporting. This data doesn&#8217;t need to be updated in real time, so the processing associated to the messages can be done asynchronously and throttled if necessary, although care may be needed in dealing with the order of the event-messages.

A high level view of the monitoring context is present in the next diagram.

<div id="attachment_1851" style="width: 710px" class="wp-caption aligncenter">
  <a href="http://www.miguelalho.pt/wp-content/uploads/2014/07/EventHandlers_MonitoringContextHighLevel.png"><img aria-describedby="caption-attachment-1851" class="size-large wp-image-1851" src="http://www.miguelalho.pt/wp-content/uploads/2014/07/EventHandlers_MonitoringContextHighLevel-1024x576.png" alt="High Level view of the monitoring event processing context" width="700" height="393" srcset="http://www.miguelalho.pt/wp-content/uploads/2014/07/EventHandlers_MonitoringContextHighLevel-1024x576.png 1024w, http://www.miguelalho.pt/wp-content/uploads/2014/07/EventHandlers_MonitoringContextHighLevel-300x168.png 300w, http://www.miguelalho.pt/wp-content/uploads/2014/07/EventHandlers_MonitoringContextHighLevel.png 1280w" sizes="(max-width: 700px) 100vw, 700px" /></a>
  
  <p id="caption-attachment-1851" class="wp-caption-text">
    High Level view of the monitoring event processing context
  </p>
</div>

The flow is quite simple, really. Event messages are published to the channel that our monitoring context is subscribed to. We&#8217;ll have a channel adapter on our endpoint to enable to correctly connect the service technology to the channel. The message is passed to a process manager that has a certain &#8220;saga&#8221; aspect to it, not so much for the state machine like nature, but because it controls the processing lifecycle or flow. The processor will request from the factory the handler objects that can handle whatever event type was passed in to the service. The factory will use a handler registry of some sort to figure out who can handle the request. The processor will then call each usable handler to handle the event. Each handler will do something to the information, updating the read model with some new state.

The handlers are, in this case, independent classes that can handle one or more event types. The handlers should be dedicated to some restricted aspect of the information we wan&#8217;t to capture, and only handle the event types that will be useful to produce that information. The more independent the handlers are of each other, the better, as we&#8217;ll avoid concurrency details, and parallelism will be obtainable. Also, event message object should be immutable, to avoid state change within the handlers.

A great thing about this is since you can have more than one handler handle a specific event type, each handler will only be concerned about some part of the event&#8217;s data and will do only a smaller, focused calculation. Algorithms and handling code should be simpler, even though there may be a large volume of these created in the context.

To cover the implementation, I&#8217;ve set up a sample solution, <a title="source code" href="https://github.com/MiguelAlho/C--Event-Processors-Example" target="_blank">hosted at Github</a> . It&#8217;s based on a spike I originally used to produce the solution. I recommend cloning and opening up the solution to follow along.

## Domain Event Message Objects

First lets take a look at the domain event message objects. They are basically POCOS, all based on an abstract Event class, and grouped into a library that can be shared across applications / contexts, to simplify the code base. Note that it is not necessary to share this library &#8211; since we are passing information across bounded contexts (from the domain app to the monitoring app), our language between contexts can change. In that case, a separate library or message parser would be used to capture the data that is in a message. Nonetheless, because our sample event classes are very simple, we&#8217;ll just share the library in the example project. Sharing would probably be done though NuGet packaging.

Here&#8217;s the code for the abstract base **Event** class:

<pre class="brush: csharp; title: ; notranslate" title="">[DataContract]
[KnownType("DerivedTypes")]
public abstract class Event
{
   ///&lt;summary&gt;
   /// Unique Id Event
   ///&lt;/summary&gt;
   public Guid Id { get; private set; }

   ///&lt;summary&gt;
   /// DateTime the event was created
   ///&lt;/summary&gt;
   public DateTime CreatedAt { get; private set; }

   ///&lt;summary&gt;
   /// Base constructor for Events. Initializes Id and creation time
   ///&lt;/summary&gt;
   public Event()
   {
      Id = Guid.NewGuid();
      CreatedAt = DateTime.UtcNow;
   }

   //... type resolution for known types next ...
}
</pre>

All it has is a **Guid** and a **DateTime** that describes and distinguishes each individual event. They are set through the constructor when the object instance is created. Because event POCOs must be serializable, I&#8217;ve added the **DataContract** attribute to the class. Also, since **Event** is abstract, we actually need to be able to (de)serialize derived types, so the **KnownType** attribute was also added. Typically, you add a reference to specific known types, but since we need them to be discovered at runtime, a method name is passed to **KnownType**, that is a private member of the **Event** class:

<pre class="brush: csharp; title: ; notranslate" title="">#region Known types resolution

        //-Next block: known types resolution based on :
        //http://stackoverflow.com/questions/16220242/generally-accepted-way-to-avoid-knowntype-attribute-for-every-derived-class

        /// &lt;summary&gt;
        /// This method is used to scan assemblies and get all known types with
        /// Event as it's base, inorder for service deserialization to work
        /// correctly
        /// &lt;/summary&gt;
        /// &lt;returns&gt;&lt;/returns&gt;
        private static Type[] DerivedTypes()
        {
            return GetDerivedTypes(typeof(Event), Assembly.GetExecutingAssembly()).ToArray();
        }

        /// &lt;summary&gt;
        /// Gets types -&gt; can be moved to a utilizty class
        /// &lt;/summary&gt;
        /// &lt;param name="baseType"&gt;&lt;/param&gt;
        /// &lt;param name="assembly"&gt;&lt;/param&gt;
        /// &lt;returns&gt;&lt;/returns&gt;
        private static IEnumerable&lt;Type&gt; GetDerivedTypes(Type baseType, Assembly assembly)
        {
            var types = from t in assembly.GetTypes()
                        where t.IsSubclassOf(baseType)
                        select t;

            return types;
        }

        #endregion
</pre>

**DerivedTypes()** searches and returns all types that derive from **Event**. The **GetDerivedTypes()** is reusable and can be moved to a framework level class.

Next, we have the **ProcessStarted** class, a sample domain event, that describes a (domain) process that was initiated, somewhat like a long-running process (but could be any type of process really).

<pre class="brush: csharp; title: ; notranslate" title="">[DataContract]
public class ProcessStarted : Event
{
   ///&lt;summary&gt;
   /// The Id of the process that was started
   ///&lt;/summary&gt;
   public Guid ProcessId { get; private set; }

   ///&lt;summary&gt;
   /// The Id of the specific process type
   ///&lt;/summary&gt;
   public Guid ProcessTypeId { get; private set; }

   /* Include any other relevant properties here like process name, user, etc, etc..*/

   ///&lt;summary&gt;
   /// Creates and object that describes an event related to some process being initialized
   ///&lt;param name="processId"&gt;The processId for the process that was started&lt;/param&gt;
   ///&lt;param name="processTypeId"&gt;The type of process that was started&lt;/param&gt;
   ///&lt;/summary&gt;
   public ProcessStarted(Guid processId, Guid processTypeId)
      :base()
   {
      ProcessId = processId;
      ProcessTypeId = processTypeId;
   }
}
</pre>

In this case, it adds a **ProcessId** guid and a **ProcessTypeId** guid to the event class. The **ProcessId** is an individual process identifier, and anything related to the process request will share this id. The **ProcessTypeId** is for categorizing the process types. Any other relevant properties could be added to the event POCO that describes the event. The source code also includes a **ProcessEnded** class that can represent an event to be emitted at the end of the process, with details about how long the process ran and if it ended with errors.

All in all, they are simple POCOS that can be serialized and transferred on the wire to the monitoring context. There, the data in them will be handled by event processors. Once again, serialization is a concern, and each derived type from **Event** will require the **DataContract** attribute.

Note: I believe that the **DataContract** / serialization attributes are the only infrastructure concerns attached to these classes. Whether that is acceptable or not is up for debate. If it were possible to remove these, or better yet, move them to an infrastructure / application level, then the application could decide what type of serialization mechanisms and attribute types to use. This is really only to point out that, lets say, if I wanted to use **MessagContract** attributes, or **XmlElement** attributes for the serialization, I would have to change the domain classes or at least their decorators. Seems to me to be a bit of a violation of the <a title="8th light's (Robert Martin) entry about the Open-Closed Principle" href="http://blog.8thlight.com/uncle-bob/2014/05/12/TheOpenClosedPrinciple.html" target="_blank">Open-Closed Principle.</a>..

## A Look at the Event Processors

The event processors are classes that are marked to handle specific groups of events. Each processor handles whatever event types it needs to get its job done, and this is done by implementing the open-generic interface **IHandleEvent<TEvent>**.

The source code for this part is made up by three projects. There is a very small and generic &#8220;**EventProcessing.Framework**&#8220;, that defines the previously mentioned **IHandleEvent<>** interface:

<pre class="brush: csharp; title: ; notranslate" title="">public interface IHandleEvent&lt;TEvent&gt; where TEvent : Event
{
 void Handle(TEvent @event);
}
</pre>

The interface is very focused, and only demands the **Handle()** method. It also restricts handled types to **Event** derivatives. Our framework also contains a **IProcessEvents** marker interface and the **Entity** abstract base class. Entity here might not be a good name. I consider the inheriting classes to be a sort of <a title="Fowler's bliki Entry for Aggregate roots" href="http://martinfowler.com/bliki/DDD_Aggregate.html" target="_blank">aggregate roots </a>withing my monitoring context (although my understanding of ARs might be hindered here by my lack of experience with correct DDD &#8211; please correct me if I am wrong). Entities, in this framework library, contain only an externally set id, passed in to its constructor. These three classes are grouped into their own library for reuse.

The other two projects associated to the monitoring  context are the **DomainAEventProcessors** library and the associated unit test library. This library has a **DailyActivityEventProcessor** that determines the number of active processes at the moment, and a **ProcessTypeCounterEventProcessor** that counts processes by type. Both can be used to update the read model used by a dashboard UI. Both retrieve data of an entity (or AR), act on it by calling commands, and persist it through an **IRepository<TEntity>** implementation.

Lets look specifically at the **DailyActivityEventProcessor** class. Because it is an event processor, it implements the marker interface **IProcessEvents**, and also handles **ProcessStarted** and **ProcessEnded** event types:

<pre class="brush: csharp; title: ; notranslate" title="">public class DailyActivityEventProcessor :
        IProcessEvents,
        IHandleEvent&lt;ProcessStarted&gt;,
        IHandleEvent&lt;ProcessEnded&gt;
    {
        private IRepository&lt;DailyActivity&gt; repository;

        public DailyActivityEventProcessor(
            IRepository&lt;DailyActivity&gt; repository
            )
        {
            Condition.Requires(repository, "repository").IsNotNull();

            this.repository = repository;
        }

        //...

</pre>

The **IRepository<DailyActivity>** implementation is injected at construction, and the implementation will typically be associated to some storage type (SqlServer, MongoDb, etc.). The **Handle()** methods have straightforward implementations, getting the current entity state from the repository, executing a command on it, and storing it again. There are obviously concurrency considerations to have, but I am not yet applying them here to simplify the example.

<pre class="brush: csharp; title: ; notranslate" title="">public void Handle(ProcessStarted @event)
        {
            string id = DailyActivity.GetIdFrom(@event.CreatedAt);
            DailyActivity activity = GetOrCreateActivityEntity(id);

            //execute change / update
            activity.AddProcess(@event.ProcessId, @event.CreatedAt);

            //save - persist?
            repository.Save(activity);
        }

        public void Handle(ProcessEnded @event)
        {
            string id = DailyActivity.GetIdFrom(@event.CreatedAt);
            DailyActivity activity = repository.Get(id);   //should exist for this event

            if (activity != null)
            {
                //execute change / update
                activity.EndProcess(@event.ProcessId, @event.EndTime, @event.WithError);

                //save - persist?
                repository.Save(activity);
            }
        }
</pre>

The **DailyActivity** class contains an internal dictionary to store process statuses keyed by id, and has a set of properties that determine, based on the dictionary&#8217;s data, the number of active processes. Because it is a daily record, the id for the entity is simply the date the process was executed, converted to string.

<pre class="brush: csharp; title: ; notranslate" title="">public class DailyActivity : Entity
    {
        public int ActiveProcesses {
            get { return TotalProcessCount - CompletionCount; }
        }   //number of active work orders (snapshot count - could also be calculated if need be)

        public int TotalProcessCount {
            get { return statuses.Keys.Count; }
        }
        public int CompletionCount {
            get { return statuses.Values.Count(o =&gt; o.EndedAt.HasValue); }
        }

        public int CompletedWithErrorCount{
            get { return statuses.Values.Count(o =&gt; o.EndedWithError); }
        }

        private Dictionary&lt;Guid, ProcessStatus&gt; statuses = new Dictionary&lt;Guid, ProcessStatus&gt;();

        public DailyActivity(string id)
            :base(id) {
        }

        public void AddProcess(Guid processId, DateTime startTime)
        {
            statuses.Add(processId, new ProcessStatus(startTime));
        }

        public void EndProcess(Guid processId, DateTime endTime, bool withErrors)
        {
            if (statuses.ContainsKey(processId))
            {
                statuses[processId].Complete(endTime, withErrors);
            }
        }

        /// &lt;summary&gt;
        /// Creates Id's for the entity
        /// &lt;/summary&gt;
        /// &lt;param name="date"&gt;&lt;/param&gt;
        /// &lt;returns&gt;&lt;/returns&gt;
        public static string GetIdFrom(DateTime date)
        {
            return date.ToString("yyyy-MM-dd");
        }
    }
</pre>

Notice that the class has two methods that allow for command-sytle calls &#8211; **AddProcess()** and **EndProcess()**. These essentially update the process list, which alters the entities properties.

##  Dispatching Events to Handlers

Now that we have events and event processors, it&#8217;s time to wire everything up. Alot of what needs to be done isn&#8217;t so much a domain specific aspect of the monitoring context, but more of an infrastructure&#8217;s concern. Because of this, I&#8217;ve tried to do as much as possible with the IOC container (which in this case will be <a href="http://docs.castleproject.org/Windsor.MainPage.ashx" target="_blank">Castle Windsor</a>). Also, to support the service, I&#8217;ll be using a WCF service, with the endpoint based on a HTTP binding for SOAP messages, but any other type of service could be used.

First thing I&#8217;ll add is the dispatcher, that will get handlers for a specific event type from the registry, and call **Handle()** on each of them with the event as an argument. I&#8217;ll add this to the framework since it is reusable. The dispatcher implements a very simple interface:

<pre class="brush: csharp; title: ; notranslate" title="">public interface IEventDispatcher
    {
        void DispatchEvent(Event @event);
    }
</pre>

And has a quite a simple implementation:

<pre class="brush: csharp; title: ; notranslate" title="">public class EventDispatcher : IEventDispatcher
    {
        private IHandlerFactory factory;

        public EventDispatcher(IHandlerFactory factory)
        {
            Condition.Requires(factory, "factory").IsNotNull();

            this.factory = factory;
        }

        public void DispatchEvent(Event @event)
        {
            IEnumerable&lt;dynamic&gt; eventHandlers = factory.GetHandlersFor(@event);

            if (eventHandlers != null && eventHandlers.Any())
            {
                Parallel.ForEach(eventHandlers, handler =&gt; handler.Handle((dynamic)@event));
            }
        }
    }
</pre>

Do take note of a couple of things:

  * The **EventDispatcher** implementation gets a **IHandlerFactory** injected. The factory can be implemented in many ways, basing it on a static map of types to handlers, or using IOC related functionality.
  * **GetHandlersFor()** returns an **IEnumerable<dynamic>**. The fact that we are using open generics (the **IHandle<>** interface), complicates things slightly, and requires us to use dynamics. Types will be resolved at runtime. This was one of the aspects where I got lost for the most part due to lack of use.
  * We are using a **Parallel.ForEach** to iterate through the handlers, because they are independent (or at least we implement them as such). This is an advantage of using simple, independent handler implementations.

The handler factory is also quite simple, and reinforces the dynamic nature of the handlers:

<pre class="brush: csharp; title: ; notranslate" title="">public interface IHandlerFactory
    {
        IEnumerable&lt;dynamic&gt; GetHandlersFor(Event eventHandler);
    }
</pre>

The dispatch mechanism is testable:

<pre class="brush: csharp; title: ; notranslate" title="">[Test]
        public void DispatcherDispatchesEventsToHandlers()
        {
            Mock&lt;IHandleEvent&lt;EventThatHappened&gt;&gt; handler1 = new Mock&lt;IHandleEvent&lt;EventThatHappened&gt;&gt;();
            handler1.Setup(o =&gt; o.Handle(It.IsAny&lt;EventThatHappened&gt;())).Verifiable();

            Mock&lt;IHandleEvent&lt;EventThatHappened&gt;&gt; handler2 = new Mock&lt;IHandleEvent&lt;EventThatHappened&gt;&gt;();
            handler2.Setup(o =&gt; o.Handle(It.IsAny&lt;EventThatHappened&gt;())).Verifiable();

            Mock&lt;IHandlerFactory&gt; factory = new Mock&lt;IHandlerFactory&gt;();
            factory.Setup(o =&gt; o.GetHandlersFor(It.IsAny&lt;EventThatHappened&gt;()))
                .Returns(new List&lt;dynamic&gt;()
                {
                    handler1.Object as dynamic,
                    handler2.Object as dynamic
                });

            EventDispatcher dispatcher = new EventDispatcher(factory.Object);

            dispatcher.DispatchEvent(new EventThatHappened());

            handler1.Verify(mock =&gt; mock.Handle(It.IsAny&lt;EventThatHappened&gt;()), Times.Once);
            handler2.Verify(mock =&gt; mock.Handle(It.IsAny&lt;EventThatHappened&gt;()), Times.Once);
        }
</pre>

Here, two mock handlers where created for a fake event. The factory returns both mocks when called, and we verify that each handle method was called.

## Using Castle to Resolve Handlers

I only added the **IHandlerFactory** interface to the framework, but no implementation for it. We can implement this factory in a couple of ways, but a really clean and interesting one is to use the IOC container. Castle Windsor has an **AsFactory()** method that proxies factory classes and registers them in the container. The only extra work is to create a selector definition when the default selection method is not useful.

I&#8217;ll start by adding the service application, which I mentioned is a WCF service (application) and that I&#8217;ll call **MonitoringService**. It&#8217;ll have a single service endpoint (**MonitorService.svc**) that implements the **IMonitor** **ServiceContract**:

<pre class="brush: csharp; title: ; notranslate" title="">[ServiceContract]
    public interface IMonitor
    {
        [OperationContract]
        [ServiceKnownType("RegisterKnownTypes", typeof(ServiceKnownTypeRegister))]
        void ReceiveEvent(Event value);
    }
</pre>

Because **Event** is abstract and we need to deserialize it&#8217;s derived types, the **ServiceKnownType** attribute was added, and known types are to be discovered via a **RegisterKnownTypes()** method on the **ServiceKnownTypeRegister** class:

<pre class="brush: csharp; title: ; notranslate" title="">public class ServiceKnownTypeRegister
    {
        //-Next block: known types resolution based on :
        //http://stackoverflow.com/questions/16220242/generally-accepted-way-to-avoid-knowntype-attribute-for-every-derived-class
        // and http://stackoverflow.com/questions/3044078/provide-serviceknowntype-during-runtime

        /// &lt;summary&gt;
        /// This method is used to scan assemblies and get all known types with
        /// Event as it's base, in order for service deserialization to work
        /// correctly
        /// &lt;/summary&gt;
        static public IEnumerable&lt;Type&gt; RegisterKnownTypes(ICustomAttributeProvider provider)
        {
            List&lt;Type&gt; types = new List&lt;Type&gt;();
            Assembly[] assemblies = AppDomain.CurrentDomain.GetAssemblies();

            foreach(Assembly assembly in assemblies)
            {
                types.AddRange(GetDerivedTypes(typeof(Event), assembly));
            }

            return types;
        }

        /// &lt;summary&gt;
        /// Gets types -&gt; can be moved to a utilizty class
        /// &lt;/summary&gt;
        /// &lt;param name="baseType"&gt;&lt;/param&gt;
        /// &lt;param name="assembly"&gt;&lt;/param&gt;
        /// &lt;returns&gt;&lt;/returns&gt;
        private static IEnumerable&lt;Type&gt; GetDerivedTypes(Type baseType, Assembly assembly)
        {
            var types = from t in assembly.GetTypes()
                where t.IsSubclassOf(baseType)
                select t;

            return types;
        }
    }
</pre>

Like what we did for the **Event** class **DataContract**, we&#8217;ll scan the assemblies and discover any and all registered **Event** derivatives as deserialization candidates for the **@event** argument of the **ReceiveEvent()** service method. **GetDerivedTypes()** is repeated here, but could be moved to some utility or framework level library.

Back to wire-up: we need to do a couple of things to get Castle to control object instance creation in the WCF application. First, we need to create the container, in order to have a composition root, which we&#8217;ll do in a **ContainerManager** static class:

<pre class="brush: csharp; title: ; notranslate" title="">public static class ContainerManager
    {
        private static IWindsorContainer _container;

        static ContainerManager()
        {
            _container = new WindsorContainer()
                    .AddFacility&lt;WcfFacility&gt;()
                    .AddFacility&lt;TypedFactoryFacility&gt;();

        }

        public static void CallInstallers()
        {
            _container.Install(FromAssembly.InThisApplication());
        }

        public static IWindsorContainer Container
        {
            get
            {
                return _container;
            }

        }
    }
</pre>

It needs to be instantiated in the **global.asax**:

<pre class="brush: csharp; title: ; notranslate" title="">protected void Application_Start(object sender, EventArgs e)
        {
            ContainerManager.CallInstallers();
        }
</pre>

The **ContainerManager** creates a new **WindsorContainer** and adds a couple of facilities we will need, namely the **WcfFacility** for WCF integration, and the **TypedFactoryFacility** to create factories. It is created once, at application start (right before we call the **CallInstaller()s** method), and then registers all the dependencies that need to be resolved by calling each available installer in the application (when the **CallInstallers()** method is calles). We create it in **Application_Start()** from the **global.asax** because we are using an HTTP version of the service. In other scenarios the **HttpApplication** is not available. Still, somewhere at application start, the container should be created and items registered for resolution.

By considering our implementation for **IMonitor**, we can pretty much get to every component we will need to register.

<pre class="brush: csharp; title: MonitorService.svc.cs; notranslate" title="MonitorService.svc.cs">public class MonitorService : IMonitor
    {
        private IEventDispatcher dispatcher;

        public MonitorService(IEventDispatcher dispatcher)
        {
            Condition.Requires(dispatcher, "dispatcher").IsNotNull();

            this.dispatcher = dispatcher;
        }

        public void ReceiveEvent(Event @event)
        {
            dispatcher.DispatchEvent(@event);
        }
    }
</pre>

**MonitorService**&#8216;s **ReceiveEvent()** implementation does only one thing &#8211; call the dispatcher&#8217;s **DispatchEvent()** to pass the event to the handlers. The class will therefore require an **IEventDispatcher** to be injected. **MonitorService** will no longer work with a default constructor, so we need to change the .svc so that Castle can handle service instance creation:

<pre class="brush: xml; title: MonitorService.svc; notranslate" title="MonitorService.svc">&lt;%@ ServiceHost Language="C#" Debug="true"
    Service="MonitoringService"
    Factory="Castle.Facilities.WcfIntegration.DefaultServiceHostFactory, Castle.Facilities.WcfIntegration"
     %&gt;
</pre>

The **ServiceHost** directive now has the **Service** attribute that names the service, and a **Factory** attribute that determines that Windsor will create the service instances.

With this in mind, let&#8217;s consider the set of dependencies:

  * The container should resolve **IMonitor** as **MonitorService**.
  * **MonitorService** requires an **IEventDispatcher**, resolved to **EventDispatcher**.
  * **EventDispatcher** requires a **IHandlerFactory**, that will be based on Castle&#8217;s typed factories.
  * The handler factory will be creating **IHandleEvent<>** instances, so we will need to register all those that are available.
  * The handlers require **IRepository<>**, so we will need to register those available, and possibly consider conventions for selecting repository types.

I like to split the statements associated to Installers into different files, depending on their focus. In this case there are three. The first is the **ServiceInstaller.cs** that registers **IMonitor**, **IEventDispatcher** and **IHandlerFactory**:

<pre class="brush: csharp; title: ; notranslate" title="">public void Install(IWindsorContainer container, IConfigurationStore store)
        {
            container.Register(
                Component.For&lt;IMonitor&gt;()
                    .ImplementedBy&lt;MonitorService&gt;()
                    .Named("MonitorService")
                    .LifeStyle.HybridPerWebRequestPerThread()
                );

            container.Register(
                Component.For&lt;IEventDispatcher&gt;()
                    .ImplementedBy&lt;EventDispatcher&gt;()
                );

            container.Register(
                Component.For&lt;ITypedFactoryComponentSelector&gt;()
                    .ImplementedBy&lt;HandleEventFactorySelector&gt;());

            container.Register(
                Component.For&lt;EventProcessing.Framework.Tests.Dispatch.IHandlerFactory&gt;()
                    .AsFactory(c =&gt; c.SelectedWith&lt;HandleEventFactorySelector&gt;())
                );
        }
</pre>

Notes about this installer:

  * **IMonitor** is registered with a name. The name is used by **HostFactory** to resolve the service implementation and should match the name in the .svc file.
  * **IEventDispatcher** registration is straightforward
  * **IHandlerFactory** registration is two-step. We use the **.AsFactory()** extension method to create the factory. We also register an **ITypedFactoryComponentSelector** to be used by the factory to contain the logic required to select the types to be used.

Because we are using open generics, and because we need to do casting, we can&#8217;t rely on Castle&#8217;s default factory selectors to resolve the instances. **HandleEventFactorySelector**, a class we have defined, will be used as the selection mechanism for the factory:

<pre class="brush: csharp; title: ; notranslate" title="">public class HandleEventFactorySelector : DefaultTypedFactoryComponentSelector
    {
        protected override Func&lt;IKernelInternal, IReleasePolicy, object&gt; BuildFactoryComponent(
            MethodInfo method,
            string componentName,
            Type componentType,
            IDictionary additionalArguments)
        {
            return (k, s) =&gt; k.ResolveAll(componentType).Cast&lt;dynamic&gt;(); // as IEnumerable&lt;dynamic&gt;;
        }

        protected override string GetComponentName(MethodInfo method, object[] arguments)
        {
            return null;
        }

        protected override Type GetComponentType(MethodInfo method, object[] arguments)
        {
            var message = arguments[0];
            var handlerType = typeof(IHandleEvent&lt;&gt;).MakeGenericType(message.GetType());
            return handlerType;
        }
    }
</pre>

The selectors are quite an interesting piece of code. It inherits from **DefaultTypedFactoryComponentSelector**, so that you only need to override whatever part of the default selector needs to be channged. For our factory, three methods need to be changed: **GetComponentName()**, **GetComponentType()**, and **BuildFactoryComponent()**. The factory usually calls the three methods (in that order, which can be debugged), and gets the name and type of the components we want to resolve. Finally, it gets the anonymous function that resolves the types based on the input. I&#8217;ve added a few overrides:

  * Since we want to resolve **IHandleEvent<>** based on the event type, we won&#8217;t be resolving components by name. Therefore we force **GetComponentName()** to return null, forcing the name component to be ignored.
  * We do want to resolve by type, based on the event type. Therefore, **GetComponentType()** creates a **Type** of **IHandleEvent<>**, and makes it a generic of the event type.
  * Since our factory will be returning multiple instances, instead of a single instance, **BuildFactoryComponent()** returns a delegate that calls the container&#8217;s **ResolveAll()**, for the type we determined, and casts each one to **dynamic**. The cast is important, and without it, we&#8217;ll get null instances or runtime errors.

This bit of logic &#8211; the selector and the **AsFactory()** &#8211; allows us to use our IOC container as the handler registry, and it will take care of creating the instances of the handlers we need to handle the received events.

We still need to register the handlers though. The associated logic is in **HandlerInstaller.cs**:

<pre class="brush: csharp; title: ; notranslate" title="">public void Install(IWindsorContainer container, IConfigurationStore store)
        {
            /// ref: http://simpleinjector.codeplex.com/wikipage?title=Castle%20Windsor

            container.Register(AllTypes.From(
                AppDomain.CurrentDomain.GetAssemblies()
                    .Where(a =&gt; !a.IsDynamic) //dynamic assemblys don't export types...
                    .SelectMany(a =&gt; a.GetExportedTypes()))
                .BasedOn(typeof (IHandleEvent&lt;&gt;))
                .Unless(t =&gt; t.IsGenericTypeDefinition)
                .WithService.Select((_, baseTypes) =&gt;
                {
                    return
                        from t in baseTypes
                        where t.IsGenericType
                        let td = t.GetGenericTypeDefinition()
                        where td == typeof (IHandleEvent&lt;&gt;)
                        select t;
                }));
        }
</pre>

I based this installer code on <a href="http://simpleinjector.codeplex.com/wikipage?title=Castle%20Windsor" target="_blank">a bit I found in SimpleInjector&#8217;s documentation</a>. Basically, what it is doing is going through all the loaded assemblies and registering any **IHandleEvent<>** it finds, with some extra filtering and an added selection delegate. This code could be changed to load assemblies in a specific folder, as if they were plugins, and the app would not need to know about them at compile time.

Finally, because our handlers require repositories, we&#8217;ll install them.

<pre class="brush: csharp; title: ; notranslate" title="">public void Install(IWindsorContainer container, IConfigurationStore store)
        {
            container.Register(
                Component.For(typeof(IRepository&lt;&gt;))
                    .ImplementedBy(typeof(InMemoryRepository&lt;&gt;))
                );
        }
</pre>

Castle in this case can automatically resolve the generic type, since we have been explicit about that in the handler constructors. We&#8217;ll use a very simple **InMemoryRepository** implementation added at the application level. In pretty much all of the installers, we&#8217;ve omitted lifestyle definitions. Castle by default creates singletons, which for our current setup is ok.

At this point , everything is setup and we can run the app and accept events.

## Testing the WCF Service

A great way to create an acceptance test (or at least something that tests the deserialization mechanism) is to throw SOAP messages at a running instance of the service (debug mode works well for this). I like to use <a href="http://www.soapui.org/" target="_blank">SOAP UI</a> for this type of verification.

The most important aspect to consider in our requests is the message format and extra elements to include to support deserialization automatically, using WCFs deserialization mechanisms. A sample request for a **ProcessStarted** event looks like this:

<pre class="brush: xml; highlight: [4,5,11]; title: ; notranslate" title="">&lt;soapenv:Envelope
	xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
	xmlns:tem="http://tempuri.org/"
	xmlns:mon="http://schemas.datacontract.org/2004/07/DomainA.Events"
        xmlns:i="http://www.w3.org/2001/XMLSchema-instance"
	&gt;
   &lt;soapenv:Header/&gt;
   &lt;soapenv:Body&gt;
      &lt;tem:ReceiveEvent&gt;
        &lt;tem:event i:type="mon:ProcessStarted"&gt;
                &lt;mon:Id&gt;c82e3c2a-0ae6-47f5-9306-fceeca08f029&lt;/mon:Id&gt;
                &lt;mon:CreatedAt&gt;2014-07-07T12:00:00&lt;/mon:CreatedAt&gt;
                &lt;!--ProcessStarted specific:--&gt;
         	&lt;mon:ProcessId&gt;f26278cc-19cc-4997-a507-a7af42d9793c&lt;/mon:ProcessId&gt;
         	&lt;mon:ProcessTypeId&gt;00000001-0000-0000-0000-000000000000&lt;/mon:ProcessTypeId&gt;
         &lt;/tem:event&gt;
      &lt;/tem:ReceiveEvent&gt;
   &lt;/soapenv:Body&gt;
&lt;/soapenv:Envelope&gt;
</pre>

SOAP UI will generally create requests based on the WSDL, but won&#8217;t include specific event type properties into the sample request. I&#8217;ve highlighted lines 4 and 5 since they are the additional namespaces that were added (**:mon** for the domain events and **:i** for the type attribute), and at line 11, where the event properties are defined, the type attribute specifies that it is a **ProcessStarted** event object. That will aid the deserialization process.

Executing the request will generate response with a SOAP envelope and a body, though the response object will have no content because the method has a **void** return type. Do note that since we are using an in-memory repository, with a singleton lifestyle, multiple calls with the same event (or at least the same processId) will generate an exception. Still, with this request you can debug and /or view the call lifecycle.

## Concluding Notes

This pretty much wraps it up. We&#8217;ve made it quite far, with a complete implementation. We have created an architecture that supports creating new event types, and handlers for those types, and automatically registering them in the application when the application starts up. Also, this architecture allows us to do multiple types of calculations and processing, in parallel, based on the same set of events.

In my opinion, the various aspects are correctly segregated, and separation of concerns is considered:

  * Event descriptions are in their own library and can be shared across projects;
  * Processors are separated and can be organized in different libraries based on the events they handle or on what they are focused on;
  * Base classes and interfaces are in framework level libraries and can be reused across projects;
  * Infrastructure-classes are defined and attached at the application level.

Resharper&#8217;s architecture graph shows the following graph that describes the previous list of notes:

[<img class="aligncenter size-large wp-image-1872" src="http://www.miguelalho.pt/wp-content/uploads/2014/07/Architecture-Graph-For-DomainEvents-1024x811.png" alt="Architecture Graph For DomainEvents" width="700" height="554" srcset="http://www.miguelalho.pt/wp-content/uploads/2014/07/Architecture-Graph-For-DomainEvents-1024x811.png 1024w, http://www.miguelalho.pt/wp-content/uploads/2014/07/Architecture-Graph-For-DomainEvents-300x237.png 300w" sizes="(max-width: 700px) 100vw, 700px" />][2]

Nonetheless, there are plenty of improvements that can be added. Some that occur to me are:

  * Out-of-order message handling (like getting a **ProcessEnded** event message before a **ProcessStarted** message for the same **processId**)
  * Handle repeated messages (what if a specific **ProcessStarted** message arrive twice?)
  * With calculations like those in **DailyActivity**, how do you handle processes that start in one day, and end on the next? (this is probably more a handler specific concern then the framework&#8217;s concern&#8230;)

##  Suggestions for Improvements?

I wanted to post this article and solution for a couple of reasons. First, it was kind of tough to get this setup, especially due to the use of dynamics, open generics, and some of Castle&#8217;s features that I wasn&#8217;t used to using. Once it is put together, though, everything makes sense. It did help me get a better understanding of how Castle works, and also some good uses for C# features.

Still, this is a type of design that is kind of new to me, especially from a usage point of view (I&#8217;ve been looking in to it a lot lately, just this is the first time I&#8217;m actually using it). Therefore, any suggestions and improvements are highly appreciated and welcome.

The source code for what I have presented here is <a title="source code" href="https://github.com/MiguelAlho/C--Event-Processors-Example" target="_blank">on GitHub (https://github.com/MiguelAlho/C&#8211;Event-Processors-Example)</a> and can naturally be cloned for use.

&nbsp;

 [1]: http://www.amazon.co.uk/gp/product/0470856122/ref=as_li_ss_tl?ie=UTF8&camp=1634&creative=19450&creativeASIN=0470856122&linkCode=as2&tag=mytymykysphot-21
 [2]: http://www.miguelalho.pt/wp-content/uploads/2014/07/Architecture-Graph-For-DomainEvents.png