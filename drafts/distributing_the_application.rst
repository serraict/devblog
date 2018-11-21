.. _blog_title:

Distributing the application
============================

We can use a service bus to send commands to 
the domain model from different client applications,
distributed across the network.
We'll have a simple console application that hosts the domain model.
We'll connect the existing asp.net GUI to this application service.
The stack:

 * front end : the existing asp.net MVC GUI
 * bus       : Rhino service bus
 * transport : Rhino Queues
 * backend   : console application

.. more::

Arghhh ... the readmodel
------------------------

Persist in MongoDB, access directly from UI.

Temporarily disable ``ThreadPool.QueueUserWorkItem(...)``
MongoDb doesn't support transactions, 
so we have to make sure read model is updated serially.

Finding the seams: UI
---------------------

FakeBus; replace with one-way bus

Finding the seams: app service
------------------------------

Setting up RSB:

https://github.com/BjRo/LearningRhinoServiceBus
http://ayende.com/blog/139266/setting-up-a-rhino-service-bus-application-part-i
http://hibernatingrhinos.com/open-source/rhino-service-bus/logging


Take aways:

 * enable logging immediately
 * descritpive tostrings for your events
 * read model: split handlers and repository-like methods?
 * make automated serialization tests for all cmmands and events that go over the wire
 * place commands and events in a dedicated namespace to enable routing
 * ``Publish`` fails if there are no subscribers, whereas ``Notify`` doesn't really care.
 * auto register handlers using the container: http://kozmic.pl/2010/03/11/advanced-castle-windsor-ndash-generic-typed-factories-auto-release-and-more/
 
 
Questions 

 * what happens to failed messages (expect tp find them in a q or similar
 * how to handle generic message failures (e.g. failed deserialization)
 * handle interceptor for web app message q
 * annoying: UI not updated with last command
 * annoying: command status not visible

 
Non-functional requirements

 * replay events and rebuild view model
 * enable logging immediately
 * descritpive tostrings for your events
 * read model: split handlers and repository-like methods?
 * make automated serialization tests for all cmmands and events that go over the wire
 * automated persistence tests (e.g. properties)
 
.. todo::

   get configuration from config files

.. rubric:: Footnotes

.. [#RhinoESB] Rhino Service Bus

.. author:: default
.. categories:: none
.. tags:: CQRS
.. comments::


