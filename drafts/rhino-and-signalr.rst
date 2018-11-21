.. _setting-up-rhino-servicebus-with-rhinoqueues:

Hooking up Rhino Servicebus to SignalR
======================================

.. Introduction


.. more::

Steps:

 * Add NuGet packages
   * singalr, use version 0.4 because og newton soft dependency [#ayende_newtonsoft]_
     `Install-Package SignalR -Version 0.4.0 `
   * server
   * client code
 * Inject the hub into the message handlers
 * send the tostring of the event to publish it

.. rubric:: Footnotes

.. [#BjRo] https://github.com/BjRo/LearningRhinoServiceBus
.. [#signalr] 
.. [#hanselman] http://www.hanselman.com/blog/AsynchronousScalableWebApplicationsWithRealtimePersistentLongrunningConnectionsWithSignalR.aspx
.. [#ayende_newtonsoft] http://ayende.com/blog/157505/ravendb-1-0-amp-newtonsoft-json-4-5-7
.. [#so_raven_signalr] http://stackoverflow.com/a/13415295/322283
.. [#detailed_example] http://beta.knowitlabs.no/a-quick-introduction-to-signalr-an-asynchronous-signaling-framework-for-asp-net/

.. author:: default
.. categories:: none
.. tags:: CQRS
.. comments::


