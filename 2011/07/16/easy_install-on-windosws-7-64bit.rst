Installing setuptools on 64-bit Windows 7
=========================================

I ran into some problems installing setuptools 0.6c11 for Python 2.7 
on my Windows 7 (64 bit) machine.
When I ran the Windows installer downloaded from the python 
`package index <http://pypi.python.org/pypi/setuptools>`_,
it gave me the error message 
"Python version 2.7 required, which was not found in the registry.".
And I'm quite sure Python 2.7 is installed.

Only then I noticed I downloaded a win32 installer - 
at least it says so in the filename: setuptools-0.6c11.win32-py2.7.exe.
I could find no 64-bits installer on pypi.

There appears to be 
`a bug <http://bugs.python.org/setuptools/issue2>`_
involving setuptools on 64 bits Windows.
It is solved according to the tracker, 
but apparently, nobody has created a 64 bit installer yet. 

A quick Google search brought me to a 
`post on hwiecher's blog <http://hwiechers.blogspot.com/2009/09/installing-easyinstall-and-setuptools.html>`_
where he (?) describes a very simple solution:

   1. Download ez_setup.py from the PEAK site. 
      (I found it here: http://peak.telecommunity.com/dist/ez_setup.py.)
   2. Run it! (python [path-to]\ez_setup.py

Yep, it's that simple.