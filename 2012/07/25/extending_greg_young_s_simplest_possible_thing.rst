.. _extending_greg_young_s_simplest_possible_thing:

Extending Greg Young's Simplest Possible Thing
==============================================

As part of my process learning CQRS,
I thought it would be interesting to extend 
:ref:`one of the existing examples <CQRS-examples>`
with some new behavior.
I decided to start with 
`The Simplest Possible Thing <http://github.com/gregoryyoung/m-r>`_,
an example application by Greg Young.
Simple it is - if you accept Lines of Code as a metric for simplicity: 
the total solutions has about 900 non-blank lines
(includes braces and comments).
It doesn't do much either; it allows users to:

 * create inventory items
 * rename inventory items
 * check in a number of items
 * remove a number of items

Primary goal for me was to 
"get better acquainted with the CQRS way of doing things".
I tried to make that goal a bit smarter,
by making up a couple of new requirements (both functional and non-functional)
and implementing them.

.. more::

These are the requirements I came up with thus far:

New functional requirements:

 * As a user, I should get a warning 
   when I try to remove more items than are currently in stock.
 * As a user, I should get a warning 
   when I try to remove or add a negative number of items.
 * As a user I should *not* be able 
   to remove more items from inventory than are currently in stock.
 * As a user, I want to receive a notification 
   when an inventory item's count falls below a certain threshold value.

Non functional requirements:

 * There are no unit tests.
   From now on, unit tests are required when introducing new features!
 * The system does not persists domain state: 
   all information is lost when the MVC application is unloaded by IIS. 
   Use `Oliver's EventStore <https://github.com/joliver/EventStore>`_
   to persist domain state by persisting events.
 * Our customers want a WPF rich client instead of this web interface.
   To prepare for this change, implement 
   an application service that hosts the domain model,
   and connect to the existing MVC front end using an open source service bus.  
 * Persist the read model in RavenDB,
   so that we can easily distribute the read model 
   across nodes on our site.

I'll post how I implemented the requirements.
If I think of more requirements, I'll list them here.
I forked `the repo on Github <https://github.com/serraict/m-r>`_
and will apply my changes there.

..
  More ideas:
  * share command validation between UI and aggregate roots
  * add tests using the simple testing framework & Moose
  * use memento

.. author:: default
.. categories:: none
.. tags:: CQRS
.. comments::
