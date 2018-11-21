CQRS Resources
==============

I've spent the last couple of weeks studying Command Query Responsibility Segregation (CQRS).
I found that information is scattered all over the internet 
and that at the moment there isn't a single authoritative resource yet,
this slows studying CQRS down somewhat.
In this post, I'll summarize the resources I found useful.

I'll first introduce some people and then list some good starting points,
depending on your needs.

.. more::

.. _cqrs_people:

People in CQRS
--------------

A short who-is-who in CQRS. 
I'll probably forget some important people ... sorry for that.

*Greg Young* is credited for coining the term Command Query Responsibility Segregation.
Rumors are that he's working on a book; but is isn't nearly finished yet,
a compilation of samples is available though [#YoungDDDD]_.
He has written many blog posts on CQRS [#YoungBlog]_
and has given many talks - many available on the net.
Saying he's an authoritative resource on the subject is an understatement.

*Udi Dahan* started the NServiceBus project and the NServiceBus Ltd company.
He has advocated the use of CQRS principles in distributed applications
and found that it was a good match with the use of an enterprise service bus
such as NServiceBus.
His article "Clarified CQRS" [#Dahan]_ is referenced a lot.

*Rinat Abdullin* is a driving force behind Lokad.CQRS [#LokadCQRS]_ 
and the CQRS pocket guide [#CQRSPocketGuide]_. 
His blog [#AbdullinBlog]_ is a good read too.

*Eric Evans*' big blue book [#EvansDDD]_ on domain driven design is a must-read.
*Vernon Vaughn* is writing a book on implementing domain driven design [#VaughnIDDD]_,
but is is not yet published.
*Mark Nijhoff* created a comprehensive and complete 
CQRS reference application in .net. It is available on Github 
and is documented on his blog [#Nijhof]_.
*Jonathan Oliver* [#Oliver]_ created the EventStore [#EventStore]_ 
framework for .net and is also a contributor to Autofac and NServiceBus.


Starting points
---------------

I can recommend the following starting points:

 * http://martinfowler.com/bliki/CQRS.html, Fowler, what is CQRS
 * http://cqrsguide.com/
 * http://abdullin.com/cqrs/
 * http://lostechies.com/gabrielschenker/2012/07/15/adnug-presentation-july-12-2012/

If you want to study a working example:

 * super-simple, by Greg Young: http://github.com/gregoryyoung/m-r
 * comprehensive, complete and documented, by Mark Nijhof: https://github.com/MarkNijhof/Fohjin
 * take it even further: http://lokad.github.com/lokad-cqrs/
 * https://github.com/etishor/CQRSEventSourcingSample
 * https://github.com/haf/Documently
 
 
If you've got some time and want to watch a video:

 * I've got about an hour and want an introduction to CQRS:
   view Oliver http://vimeo.com/9573973
 * I've got about two hours and want to put CQRS in business perspective:
   http://www.youtube.com/watch?v=KXqrBySgX-s
 * I've got about a day and want a thorough understanding of CQRS; 
   both from a business and developer perspective:
   http://www.viddler.com/v/dc528842
   
The last video is almost 7 hours long, 
but it is AWESOME and worth every minute of your time 
if you're seriously considering applying CQRS to one of your projects.



Code - examples & frameworks
----------------------------

The basic principles for doing CQRS are pretty straightforward and 
do not require the use of advanced frameworks.

Using a framework for event sourcing (persisting domain state using events) 
might be useful. 
For all other things, you are probably best of rolling your own infrastructure,
which should not be more then several hundreds lines of code.

.. _CQRS-examples:

Examples
^^^^^^^^
 
 * super-simple, by Greg Young: http://github.com/gregoryyoung/m-r
 * comprehensive, complete and documented, by Mark Nijhof: https://github.com/MarkNijhof/Fohjin
 * take it even further: http://lokad.github.com/lokad-cqrs/
 * https://github.com/Lokad/lokad-cqrs

"Frameworks"
^^^^^^^^^^^^

Packages on NuGet: http://nuget.org/packages?q=cqrs

 * http://nuget.org/packages/EventStore
 * http://nuget.org/packages/Lokad.CQRS
 * http://nuget.org/packages/Scritchy
 * http://nuget.org/packages/SimpleCqrs
 * http://nuget.org/packages/ncqrs 

.. rubric:: Footnotes

.. [#AbdullinBlog] http://abdullin.com/
.. [#CQRSPocketGuide] http://cqrs.wikidot.com/
.. [#LokadCQRS] http://lokad.github.com/lokad-cqrs/
.. [#EvansDDD] Domain Driven Design
.. [#EventStore] https://github.com/joliver/EventStore/
.. [#Oliver] http://blog.jonathanoliver.com/
.. [#YoungBlog] http://goodenoughsoftware.net/2012/03/02/cqrs/
.. [#YoungDDDD] http://abdullin.com/storage/uploads/2010/04/2010-04-16_DDDD_Drafts_by_Greg_Young.pdf
.. [#Dahan] http://www.udidahan.com/2009/12/09/clarified-cqrs/
.. [#VaughnIDDD] http://my.safaribooksonline.com/9780133039900
.. [#Nijhof] https://github.com/MarkNijhof/Fohjin

.. author:: default
.. categories:: none
.. tags:: CQRS
.. comments::
