---
title: Visual Studio Solution and Package Versioning – Part 1
author: Miguel Alho
type: post
date: 2014-10-05T10:44:49+00:00
url: /visual-studio-solution-and-package-versioning-part-1/
categories:
  - Uncategorized

---
Previously, I wrote in my previous post &#8220;<a href="http://www.miguelalho.pt/semver-team-development-and-visual-studio-solutions/" target="_blank">SemVer, Team Development and Visual Studio Solutions</a>&#8221; some of my thoughts about solution and application versioning. It&#8217;s been nearly two months since I wrote it, and after having followed the strategy I outlined, I can now discuss my experience with it. My team and I have felt some of the pains associated to it. Just like when I wrote it I was looking for a solution to my problem at the time, I continue to search for fixes to other problems that have arisen.

I understand that that post is kind of TL;DR but it pretty much states what my problem was and still is, and a possible path. At this moment, I still think that what was presented is still valid and works. The problems that I have encountered are essentially about tooling and concurrency. NuGet can be a REAL pain in many ways, if you don&#8217;t understand how it works.

What I want to do now is go over the pain point&#8217;s I have felt and try to find solutions for them. Hopefully, this will help me get my head around what I really need, since so many ideas are going through my mind. Hopefully, by writing this, I can get my mind straightened out, and help others to do the same.

## The &#8220;solution&#8221; I used&#8230;

The changes I applied, based on the previously mentioned post, was used across 3 VS solutions with 10, 60 and 90 projects each (to help understand the dimension of the problem). The most generic solution contained shared libraries and had the least number of projects; the others refer to different application components.

Constraints were applied to simplify some aspects of the process:

  * Any project that would have shareable library outputs were packaged into NuGet packages. Package generation was aided by [OctoPack][1], applied through NuGet.
  * Two NuGet repositories were used: a local repository on the developer&#8217;s machine &#8211; a dev could at any moment create a package and consume it without affecting other devs efforts during development; a second feed is used for CI built and published packages. These are generally considered official.
  * NuGet configuration was contained in the solution, by use of nuget.config file in the .nuget solution folder. LocalRepo folder was a relative path \ sibling directory to the solution folder. This way, pulling from the repo would be enough to get dev machines to connect to the repos.
  * SemVer was used as .Net assemblies permit, using only the first 3 parts (major, minor and patch). Revision number was generally 0, except when we needed to regenerate a package. I seriously wanted to use SemVer because it makes so much sense, but there are problems with that, which I hope to highlight shortly.
  * To simplify package management and dependency control within a solution, ALL projects shared the same version number, stored in the project&#8217;s AssemblyVersion.cs file, and was updated using [Zero29][2].
  * Because we used OctoPack, assembly and package version numbers were identical to the assembly versions and consistent throughout the solution.
  * EVERY packaged project has a nuspec file, to enforce file inclusion, dependency requirements, and metadata (except for version number), Dependencies can be VERY problematic and correct care must be taken to get this up to date consistently.
  * A &#8220;tools&#8221; solution folder was added to each solution, with a &#8220;BuildAndPack&#8221; folder. This folder contained a series of .bat files to simplify some steps, such as version increment tasks, nuspec file management, and inter solution package updates (which I&#8217;ll get to shortly).

Devs became responsible for controlling the versioning process. Any time a change was made in a solution, the dev would need to increment the version number, build and pack locally. With these local packages, he could update the downstream projects with the changes in the dependency packages and validate the integration. This would allow him to test his changes and effects in the dowstream project (which in many ways commands changes in upstream projects).

Still, we tried our best to simplify and automate each step, to minimize the number of decision and manual steps required. Also, there was a strong attempt to keep the solution self contained, in order for any member to get all the necessary tooling components and scripts on a pull from source control.

## Evolution

The solution in place isn&#8217;t tuned yet to what we as a team require. It&#8217;s a start, but needs many fixes and solutions to the many pain points that are still felt. I relate them mainly to tooling and change concurrency.

I mention tooling, because NuGet has a set of default behaviors that are somewhat problematic. For instance, consider:

  * a project A that consumes a package B and C.
  * B defines C as a dependency, and says in it&#8217;s nuspec to work with any version of C from 1.0 to 2.
  * project A currently consumes v1.0 of B and C.
  * A new version of B and C exists at v1.1.

If you update package B in project A, you would probably expect C to get updated. Unfortunately, it isn&#8217;t. A already has a version of C, and sticks with the lowest version available of C. You would end up with B at 1.1 and C at 1.0. Two ways to get C up to date are to update C explicitly or change B&#8217;s nuspec to demand C at 1.1 as a minimum. NuGet doesn&#8217;t support this out of the box and so I had to develop a tool to iterate the nuspecs in a solution and update minimum package versions, especially on dependencies within a solution, because all packages generated from a solution will have the same version.

Concurrency-wise, when 2 devs update the version of the assemblies within a solution, every AssemblyInfo.cs file is changed on the same lines, and concurrent changes generate merge conflicts in source control. If changes are infrequent, it is more or less quick and easy; if they are frequent, devs get stuck in merge resolution. Same thing happens to .csproj files, since the path to the package contains the version number.

## Series of Problems and Solutions

I want to tackle these problems in a series of posts, finding workarounds or solutions for them. As they get written I&#8217;ll add the links to the list:

  * Version increments and minimizing merge conflicts
  * Keeping intra-solution package dependencies aligned
  * Keeping inter-solution package dependencies aligned
  * Keeping things aligned in a NuGet package update

I&#8217;ll also try to keep the tool set used in a GitHub repo @ <https://github.com/MiguelAlho/NuSpecDependencyUpdater> and eventually as NuGet packages (once I figure out how the public feed works).

&nbsp;

 [1]: https://github.com/OctopusDeploy/OctoPack
 [2]: https://www.nuget.org/packages/Zero29