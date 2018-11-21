Generating C# code files for B2MML schemas
==========================================

Recently I was doing some proof-of-concepts for using B2MML messages in my 
.NET/C# application. 
I wanted to be able to generate xml-serializable C# classes, 
compliant to the B2MML xsd schema's. Since the xsd schema's are 
`available on the WBF website <http://www.wbf.org/catalog/b2mml.php>`_, 
I figured I'd simply run those through ``xsd.exe`` and start coding away.

However, for some mysterious reason, you are not able to specify the filename 
of the .cs output file ``xsd.exe`` generates. 
By default it concatenates the names of all the schema's you pass into it. 
If you generate a code file for the  twenty-or-so schema's in the B2MML 
implementation, ``xsd.exe`` chooses a filename that is awkward to read and, worse, 
too large to be handled by my Windows installation.

I found a `work-around for this issue on stackoverflow <http://stackoverflow.com/questions/906093/xsd-exe-output-filename>`_. 
It appears that there is a `known bug
<http://social.msdn.microsoft.com/Forums/en-US/xmlandnetfx/thread/8ab41df8-69d4-44e4-8795-436088f230e2>`_ 
in ``xsd.exe`` that you can exploit to get a short, readable filename: 
pass the last schema using the ".\" path characters. 
Now ``xsd.exe`` only  uses the last schema in the output filename. 
So I created an empty xsd schema named ``B2MML-V0401.xsd`` and ran

``xsd B2MML-V0401-AllExtensions.xsd B2MML-V0401-Common.xsd 
[other schemas go here] .\B2MML-V0401.xsd /c``

Which resulted in the file ``B2MML-V0401.cs``.

The resulting C# code is usable in my application, 
however there are some things that still bother me:

 - I don't really like the code that is generated 
   (amongst other things I'd prefer generic lists over arrays and
   all code ends up in one huge 30k+ LOC codefile)
 - I don't like the fact that I utilize a known bug in ``xsd.exe``
 - The command needs many parameters, which makes it unreadable.

I posted my work-around to the 
`WBF XML newsgroup <http://tech.groups.yahoo.com/group/wbf-xml-wg/>`_ 
and got some useful replies to overcome these issues. 
I'll investigate those and post my findings here.