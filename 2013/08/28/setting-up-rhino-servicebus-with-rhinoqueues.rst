.. _setting-up-rhino-servicebus-with-rhinoqueues:

Setting up Rhino Servicebus with Rhinoqueues
==========

.. Introduction

I'm builiding a CQRS applicatation and would like to use Rhino ESB with
Rhino Queues as transport channel. I start the backend using::

    var host = new DefaultHost();
    host.Start<EmptyBackendBootStrapper>();

and unexpectedly the application throws a `HandlerException`, with the message::

    "Can't create component 'Rhino.ServiceBus.Impl.DefaultServiceBus' as it has dependencies to be satisfied."

    'Rhino.ServiceBus.Impl.DefaultServiceBus' is waiting for the following dependencies:

     - Service 'Rhino.ServiceBus.Internal.ITransport' 
       which was not registered.
     - Service 'Rhino.ServiceBus.Internal.ISubscriptionStorage' 
       which was not registered.

What was the problem? I used NuGet to install the dependencies, but I 
forgot to add `Rhino.ServiceBus.RhinoQueues`, instead adding only 
the `Rhino.Queues` package. 
Solution is to install the `Rhino.ServiceBus.RhinoQueues` package :)

Also make sure you install the correct dependencies;
for instance an newer version of `Rhino.Queues` might throw 
an error because it cannot load the required assembly version.
Nuget should have added an assembly redirect, but if you don't have it, 
you can tell NuGet to add binding redirects based on your packages::

    Add-BindingRedirect -ProjectName IntegrationTests.AppServer

.. rubric:: Footnotes

.. [#BjRo] https://github.com/BjRo/LearningRhinoServiceBus

.. author:: default
.. categories:: none
.. tags:: CQRS
.. comments::


