Generating C# code files for B2MML schema's (2)
===============================================

I tried generating C# code files from the B2MML-V0401 xsd schema's 
using ``xsd.exe``. 
It required utilizing a bug in ``xsd.exe``, 
typing in a command with as many parameters as xsd schema's
and resulted in a single, huge code file with code that I didn't really like.
It worked, but I wasn't completely satisfied 
(see my previous post on this subject).
After posting my findings to the 
`WBF XML newsgroup <http://tech.groups.yahoo.com/group/wbf-xml-wg/>`_, 
I got a useful reply from Nick.

Nick advised me to create a single schema file that includes the 
B2MML-V0401 xsd schema's for which code should be generated and name it
(for instance) ``B2MML-V0401.xsd``.
Then run ``xsd B2MML-V0401-AllExtensions.xsd .\B2MML-V0401.xsd /c`` 
on this file and voilà, you'll get the code generated into ``B2MML-V0401.cs``.

I like this method, because the command is readable 
and I can cleanly specify the schema's to use for input.

However, it still left me with the code I didn't really like. I'd like:

 * generic lists instead of arrays
 * the ability to generate data contract attributes
 * to be able to automatically put the code in separate code files
 
Nick pointed out that his company provides proprietary libraries 
that are compatible with B2MML messages and that provides the API's I prefer.
At this time however, I don't want to go the proprietary road.
However, if I can get my hands on an evaluation version, 
I'll experiment a bit with it.
 
I spend some, or rather too much, time searching for a freely available tool
to do this code generation for me. I came across ``CodeXS`` en ``XsdObjectGen``,
but I found it cumbersome to start using them, 
and their current status was unclear to me.
I ended up using `Xsd2Code <http://xsd2code.codeplex.com/>`_, 
which enables me to generate the code I like from within Visual Studio (2008). 
It also has a command line tool, but I haven't checked it out yet.

Any suggestions on code generation tools (preferably open source) are welcome.
