Unit testing in VS 2010
=======================

After installing VS 2010, I figured I'd create a simple MVC project 
to get acquainted with the new interface.
In the project creation wizard I was presented with the option 
to create an MS test project.
Nice, unit testing natively integrated to Visual Studio! 

How does it compare to my current unit-testing setup of
NUnit 2.5 and ReSharper as test runner?

A clean comparison is made 
`by Jeff <http://www.barebonescoder.com/2010/06/mstest-vs-nunit-with-visual-studio-2010-tdd/>`_, 
although he constraints himself by requiring to use the NUnit GUI runner.
(He has `Testdriven.NET <http://testdriven.net/>`_ installed, though.)
He concludes that NUnit wins, because of the clarity of the test code. I agree.
He also likes the fluent interface of NUnit, which is a matter of taste - 
I don't like it that much. 
Luckily using the fluent api is optional when using NUnit.

Another point I like about NUnit is its track record of integration 
with CI tools and msbuild scripts.
For MSTest, this level of automation 
`appears to be harder to achieve <http://stackoverflow.com/questions/2367734/nunit-vs-visual-studio-2010s-mstest>`_.
Although this might change in the futurre, I'm not going to invest in it now.

For now I'm sticking with ReSharper and NUnit.