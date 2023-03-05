---
title: Build vNext projects in TFS (on-premise)
author: Miguel Alho
type: post
date: 2015-07-16T23:34:11+00:00
url: /build-vnext-projects-in-tfs-on-premise/
categories:
  - 'Code &amp; IT'

---
Since I&#8217;ve been working with vNExt these last few weeks at Celfinet, and having started a brand new solution for some services, one of my main concerns was to get a build setup ASAP. The lesser the amount of dependencies and projects and what not, the easier and faster it is to setup the build. Faster here is mainly for code sync (from source control, in this case, GIT) and the build feedback. The less amount of work that needs to be done, the faster it will be to se the whole job pass or fail, which in the setup phase can be daunting if you have to wait long.

We generally use one of two build systems &#8211; TFS or Jenkins. The new build system (also known as Build.VNext or Build.Preview in the menu) was recently made available to the team in our installation. I&#8217;m not a fan of the previous build system. The Xaml workflow concept for defining jobs isn&#8217;t a bad idea, but it just isn&#8217;t a great experience and is really hard to get into and use. I do like Jenkins flexibility &#8211; you build is basically just a sequence of steps of anything &#8211; but the UI and navigation could really use some work. When I saw the presentations for the new TFS system, I was hooked (even without trying!). The interface is super clean and has all the flexibility that we got from Jenkins. There is still plenty that can be done to make the new TFS build system spectacular, but it clearly is going in a great direction.

Sidenote: I&#8217;m quite curious as to why the ASP.Net team hasn&#8217;t adopted the TFS / VSO build system. Preference seems to have gone to Travis/ AppVeyor based builds (as presented on GitHub project pages).

Anyway, not everything was a smooth setup in the build system for my projects. Not that it isn&#8217;t easy, but it&#8217;s still a preview and documentation is scarce, so I naturally hit some obstacles.

### 1 &#8211; DNVM, DNU updates (prerequisite)

First thing to do in the build, and because we&#8217;ll need to build using dnu, is to install/update DNVM and install / upgrade DNU to whatever our solution requires. The process is an automated version of [my previous post][1]. The powershell script used is similar to the one at <https://msdn.microsoft.com/Library/vs/alm/Build/azure/deploy-aspnet5#Createthedefinition>. It :

  * updates / installs dnvm
  * updates dnu to whatever is in the solution&#8217;s global.json, and sets the appropriate environment var to use it.
  * restores packages for the solution&#8217;s projects.

The only difference is referenced script only restored dependencies for projects within the \src folder. My modified version restores packages for all the projects (or, better yet, all the project.json) in the solution.

Therefore, the following powershell script was added to the solution folder (and source controlled) and added as a first step in the build process.

 

### 2 &#8211; MSBuild it

Second step was a regular MSBuild step, using the MSBuild task and pointing to the solution file. It&#8217;s more than enough and will correctly capture all the projects and dependency graph within it.  

### 3 &#8211; Run xUnit tests

This was a painfull one for me. I spent a lot of time trying to use the VSTest task available, with no luck. It would run, sometimes with success (depending on whatever settings I used &#8211; I tried so many it&#8217;s now hard to tell). [Donovan Brown had already written up a post on this][2] which I tried, but it just wouldn&#8217;t work for me &#8211; the path to the adapters the he mentions wouldn&#8217;t work in my installation. So after a lot of struggle I tried inventing a powershell script to recursively find all the test projects under the solution&#8217;s \test folder and run dnx . test on all of them. This off course assumes that all the test projects are under \test and that all of them have a test command in the project.json. It&#8217;s a bit forced, but for a first effort should be ok. refactoring later on when more info is available will be required! So after the MSbuild step I added another powershell step to run the xUnit tests:



Like the previous one, the script was placed in source control in the solution directory.

### Next steps

Build and test was the most important parte of what I wanted in this first approach to the build. Next step will be generating the appropriate artifactes (nuget packages) and [deploying to an environment with OctopusDeploy][3]. Hopefully it&#8217;ll be pretty straightfoward 😀

 [1]: http://www.miguelalho.pt/upgrading-vnext-mvc6-apps-from-beta4-to-beta5/
 [2]: http://www.donovanbrown.com/post/2015/06/15/how-to-run-xunit-test-with-vnext-build
 [3]: https://octopusdeploy.com/blog/octopus-integration-with-tfs-build-vnext