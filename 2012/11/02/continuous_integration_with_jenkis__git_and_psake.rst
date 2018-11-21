Continuous Integration with Jenkins, git and psake
==================================================

Right now, I'm using a Continuous Integration (CI) setup consisting of:

 * A Jenkins CI server, running on a Rackspace Ubuntu 12.04 server
 * git source control, backed by a github project
 * .net projects, with psake build scripts
 * Windows 7 build machine

Here are a few take-aways:

.. more::

Windows slave
-------------

 * Use labels to identify a build machine as a Windows machine.
 * Since my build machine isn't accessible from the Internet,
   I used Java Web start to get started and install
   it as Windows service from the UI [#javawebstart]_.
   Note you might want to run the `javaws ... ` 
   from a command prompt with elevated privileges
   to be able to install a Windows Service.
 * I configured the Jenkins Slave service to log on as 
   dedicated user on the build machine that has all the build paths
   and ssh keys set up to work with my bot account on Github [#serra_bot]_   
 * Don't forget that your slave has to give consent to restore missing 
   using NuGet [#nuget_consent]_; id did this by setting the 
   `EnableNuGetPackageRestore` environment variable to `true`.
 * Install the Windows (7/8/...) SDK
 * You might need to copy some build targets from 
   `C:\Program Files (x86)\MSBuild\Microsoft\VisualStudio`,
   or alternatively install Visual Studio on your build server
   (now WHO would want to do THAT?).

git & Github
------------

 * Authenticate using https, 
   make sure you can connect WITHOUT entering credentials.
   I did this using gitextensions, 
   which stores the credentials in the Windows credential cache.
 * Or install using SSH:  
 
     * On your build slave, set up an ssh key pair that doesn't require a pass phrase.
     * The first time, connect manually from the command line [#github_ssh]_.
     * Run the slave service as a user that has a ssh key pair setup for the git repo.
     * Check from a command window that your git client is properly set up.
       You should be able to do `ssh -T git@github.com` and
       `git clone git@github.com:<user>/<repo>.git` 
       without any additional key strokes.
     * When setting up your Windows slave, you might find Thomas' blog post 
       [#vandoren]_ useful.
   
psake
-----

 * Don't forget to set the PS execution policy to `RemoteSigned` 
   on your build slave.
 * When using the Jenkins PS plugin, 
   use the `psake.ps1` helper script with `$psake.use_exit_on_error = $true` 
   [#psake_jenkins_integration]_.
   This to make sure the build script exits with an exit code not equal to 0
   in the case of build errors.
   I didn't get this to work; not sure why. So ...
 * Instead of using the Jenkins PowerShell[#PSPLugin]_ plugin for executing 
   psake build scripts, I used the `psake.cmd` deployed with psake.
   By default, `psake.cmd` bypasses the execution policy, 
   so you might not want to do this.
  

.. rubric:: Footnotes

..  [#psake_jenkins_integration] https://github.com/psake/psake/wiki/How-can-I-integrate-psake-with-Hudson%3F
..  [#psake_using_ps_plugin] http://matosjorge2013.wordpress.com/2010/07/27/how-to-integrate-psake-with-hudson/
..  [#psake] https://github.com/psake/psake/wiki
..  [#PSPLugin] https://wiki.jenkins-ci.org/display/JENKINS/PowerShell+Plugin
..  [#github_ssh] https://help.github.com/articles/generating-ssh-keys
..  [#vandoren] http://blog.thomasvandoren.com/2011/09/jenkins-windows-slave-with-git/
..  [#javawebstart] https://wiki.jenkins-ci.org/display/JENKINS/Distributed+builds#Distributedbuilds-LaunchslaveagentviaJavaWebStart  
..  [#serra_bot] https://github.com/serra-bot
..  [#nuget_consent] http://blog.nuget.org/20120518/package-restore-and-consent.html
..  [#onehour_guide] http://ferritedog.wordpress.com/2011/05/27/1-hour-guide-to-continuous-integration-setup-jenkins-meets-net/
..  [#nunit_plugin] https://wiki.jenkins-ci.org/display/JENKINS/NUnit+Plugin
..  [#xunit_plugin] https://wiki.jenkins-ci.org/display/JENKINS/xUnit+Plugin
..  [#htmlpublisher_plugin] https://wiki.jenkins-ci.org/display/JENKINS/HTML+Publisher+Plugin
  
  
.. author:: default
.. categories:: none
.. tags:: none
.. comments::
