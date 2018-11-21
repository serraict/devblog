Passing build properties to psake.cmd
=====================================

I love psake and used Chocolatey's ``cinst psake`` to make ``psake.cmd`` globally available on my system.

When I tried to do::
    
    psake PackageSite -properties @{"configuration"="staging-standalone"}
    

I got the error message::
    
    C:\Chocolatey\lib\psake.4.2.0.1\tools\psake.ps1 : Cannot process argument transformation on parameter 'properties'. Cannot convert the "System.Collections.Hashtable" value of type "System.String" to type "System.Collections.Hashtable".
    

To pass the string-formatted hashtable to Power Shell, 
use single quotes inside and double quotes around::
    
    psake PackageSite -properties "@{'configuration'='staging-standalone'}"
    

.. author:: default
.. categories:: none
.. tags:: doh
.. comments::
