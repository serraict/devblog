Unit-testing System.Net.Mail.SmtpClient
=======================================

On a recent project I wanted to write unit-tests for a class using a 
``System.Net.Mail.SmtpClient`` instance. 
In certain situations, the class should send an email with an attachment. 
Before I started, I thought I'd simply mock the ``ISmtpClient`` interface 
for this purpose ... however, such an interface does not exist.

After doing some research on the web, I came up with the 
following possible solutions:

 #. Dumping the mails on disk, by configuring System.Net.Mail.SmtpClient to 
    use a ``deliveryMethod`` of type ``SpecifiedPickupDirectory`` (`example <http://stackoverflow.com/questions/567765/how-can-i-save-an-email-instead-of-sending-when-using-smtpclient>`_)
 #. Running a local smtp server to catch my test mails (and then somehow 
    inspect those emails to see if they meet my requirements)
 #. Writing a mockable wrapper (e.g. implementing "``ISmtpClientWrapper``") 
    and pass this wrapper to my consuming class
 #. Run a simple SMTP server within my unit tests
 
Of these options, the last one appeared the least intrusive to me. 
I first used 
`nDumpster <http://ndumbster.sourceforge.net/default.html>`_
(a .NET port of the Java project 
`Dumpster <http://quintanasoft.com/dumbster/>`_), 
however it would cause my unit tests to hang. 
Before figuring out why, I switched to 
`netDumpster <http://netdumbster.codeplex.com/>`_, 
which has (about) the same API. ``netDumpster`` ran just fine 
and enabled me to run my unit tests fast enough.