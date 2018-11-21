.. _adding_mongodb_event_store:

Adding a persistent event store
===============================

Right now, the Simple.CQRS application use an in-memory event store,
which means all events are lost when the asp.net MVC application unloads.
The read model is also only kept in memory.

Instead of building my own even store, I choose to use Jonathan Oliver's [#Oliver]_
event store [#EventStore]_, which supports a wide variety of persistence engines.
I picked MongoDB [#EventStoreMongoDB]_, 
basically just because I never had used MongoDB before.

.. more::

.. note:: 

  Installing MongoDB [#MongoDB]_ was a breeze and I had it up-and running in no time.
  I found the post *Getting started with MongoDB on .net* [#MongoDB.net]_
  a good starting point.
  For exploring databases (and for viewing diffs), 
  I prefer using a GUI over the command line, 
  so I installed MongoVue [#Mongovue]_.
  I had some issues with serialization, which were solved 
  by registering my event types with MongoDB [#SOMyq]_ [#SOOtherQ]_.

Finding the seams ...
---------------------

``Simple.CQRS`` defines a ``SimpleCQRS.IEventStore`` interface::

    public interface IEventStore
    {
        void SaveEvents(Guid aggregateId, IEnumerable<Event> events, int expectedVersion);
        List<Event> GetEventsForAggregate(Guid aggregateId);
    }

Simple enough; I figured I'd just implement that interface using Oliver's event store.

I choose not to persist the read model, 
but instead replay all events on application startup,
which rebuilds the in-memory read model.
Only thing we have to do, is provide a way to get all events from the event store,
so I introduced::

    internal interface IGetAllEvents
    {
        Event[] GetAll();
    }  

Hook into the seams
-------------------

Well, just implement the above interfaces, full code 
`on Github <https://github.com/serraict/m-r/blob/master/CQRSGui/Infra/EventStore.cs>`_ ::

    public class EventStore : SimpleCQRS.IEventStore, IGetAllEvents
    {
        private IStoreEvents _store;

        public EventStore(EventStore.IStoreEvents store)
        {
            _store = store;
        }

        public void SaveEvents(Guid aggregateId, IEnumerable<Event> events, int expectedVersion)
        {
            // snip ... see github
        }

        public List<Event> GetEventsForAggregate(Guid aggregateId)
        {
            // snip ... see github
        }

        public Event[] GetAll()
        {
            return _store.Advanced.GetFrom(DateTime.MinValue)
                .SelectMany(c => c.Events)
                .Select(ToSimpleCQRSEvent)
                .ToArray();
        }
    }

This event store is initialized in ``global.asax`` ::

    private static CQRSGui.Infra.EventStore GetWiredEventStoreWrapper()
    {
        var types = Assembly.GetAssembly(typeof(SimpleCQRS.Event))
                                    .GetTypes()
                                    .Where(type => type.IsSubclassOf(typeof(SimpleCQRS.Event)));
        foreach (var t in types)
            BsonClassMap.LookupClassMap(t);

        var store = Wireup.Init()
            .UsingMongoPersistence("mongo", new DocumentObjectSerializer())
            .UsingSynchronousDispatchScheduler()
            .DispatchTo(new DelegateMessageDispatcher(DispatchCommit))
            .Build();

        return new CQRSGui.Infra.EventStore(store);
    }

And then passed to the ``SimpleCQRS`` repository as a ``SimpleCQRS.IEventStore``.
    
Rebuilding the read model
-------------------------

After wiring up the event store, we have to replay all events 
to rebuild the in-memory read model::

    private void RebuildReadModel(IGetAllEvents store, FakeBus bus)
    {
        foreach (var e in store.GetAll())
        {
            bus.Publish(e);
        }
    }

This is a hack; imagine we had hooked up an email event handler that mailed 
our boss when an inventory item was deactivated: 
the mail would be resend every time the app started.

But for now it works.



.. rubric:: Footnotes

.. [#Oliver] You might remember him from :ref:`cqrs_people`
.. [#EventStore] https://github.com/joliver/EventStore and http://nuget.org/packages/EventStore
.. [#EventStoreMongoDB] http://nuget.org/packages/EventStore.Persistence.MongoPersistence
.. [#MongoDB] http://www.mongodb.org/
.. [#MongoDB.net] `Getting started with MongoDB on .net <http://www.simple-talk.com/dotnet/.net-framework/mongodb-basics-for-.net-by-example/>`_
.. [#MongoDBCSSerialization] Serialization using the MongoDB c# driver <http://www.mongodb.org/display/DOCS/CSharp+Driver+Serialization+Tutorial>
.. [#SOMyq] http://stackoverflow.com/q/11755859/322283
.. [#SOOtherQ] http://stackoverflow.com/q/7451422/322283
.. [#mongoVue] http://www.mongovue.com/

.. author:: default
.. categories:: none
.. tags:: CQRS
.. comments::


