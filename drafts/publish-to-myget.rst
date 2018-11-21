.. _publis_to_myget:

Blog Title
==========

.. Introduction

Set api key for the current user:

  NuGet setApiKey c18673a2-7b57-4207-8b29-7bb57c04f070 -Source http://www.myget.org/F/testfeed

Don't check this into source control - it's secret.
  
.. more::

When `NuGet setApiKey`, the key is stored in `%AppData%`, which is considered safe [#Ebbo]_.

Don't forget to set api key on build server for the user (bot) that 
performs your publish step.



.. rubric:: Footnotes

.. [#Ebbo] http://blog.davidebbo.com/2011/03/saving-your-api-key-with-nugetexe.html
.. [#MyGetBlog] http://blog.myget.org/post/2011/06/01/MyGet-now-supports-pushing-from-the-command-line.aspx


.. author:: default
.. categories:: none
.. tags:: ci
.. comments::


