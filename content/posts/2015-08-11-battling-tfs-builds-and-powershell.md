---
title: Battling TFS Builds and Powershell
author: Miguel Alho
type: post
date: 2015-08-11T22:19:22+00:00
url: /battling-tfs-builds-and-powershell/
---
My last few days have been about getting DNX builds to work in TFS. I&#8217;ve got a couple of projects / solutions of different types ongoing, between web apps, web APIs and class libraries. I consider a good build system as a fundamental component in the development process, to aid in continuous integration and enable continuous deployment.

TFS vNext builds in TFS 2015 present a new setup, and as with any new tech problems arise, and solutions aren&#8217;t always easy to find. Existing tasks don&#8217;t always cater to my needs, so I&#8217;ve been creating a bunch of simple Powershell scripts to fill in the gaps. And since I&#8217;m an absolute Powershell noob, well, lots of problems appeared.

So, what follows are some problem-solution scenarios I found along the way.

### TFS build + Powershell : Persist vars between steps

**The Context:**  
I&#8217;ve opted to give GitVersion a try. One of my solutions is composed of class libraries that are reusable in multiple other solutions (base classes, interfaces, abstractions). My distribution mechanism of choice is an internal nuget server. As such versioning the packages is key. I prefer to follow [SemVer][1] since it explicits what changes occur, and also garantees an incremental version number on each change.

Previously (pre-dnx), I&#8217;ve built a bunch of command line tools for my team to use to control the version numbers. This required devs to run a command pre-commit to increment the number in the `AssemblyInfo.cs` files (or in our case a single shared file). Increments were driven by the dev, who would choose to increment the major, minor or patch component. .Net assemblies include a fourth component &#8211; revision, which we would allow to increment if no changes were made but we simply wanted a new build / package (because something went wrong). NuGet updates in projects / solutions require incremental numbers so this was useful to us.

Though not difficult, the manual increment step is still a manual step. [GitVersion][2] automates this, guaranteeing at least a patch increment on each new build. Eventually, git commit message can drive minor and major increments (using &#8220;feature&#8221; and &#8220;breaking&#8221; in the commit message). Alternatively, the `GitVersion.yaml` file has a next-version property that can be used for that too.

**The problem:**  
In my build, I have a step (before dnu restore and build) to get the new version number and add it to an environment variable. The idea would be to persist the variable&#8217;s value throughout the various build steps (and not across builds or permanently). Unfortunately, using an `$env:varname` type of attribution in the Powershell script does not store the variables. Calling `Get-Variables` in the next steps does not present the variable in the list of available variables. This may be possible in the future, and should be possible in VSO (using specific variables), but not yet in TFS 2015. I open an issue that has a short discussing associated: <https://github.com/Microsoft/vso-agent-tasks/issues/375> .

**The workaround:**  
To solve this, I took the &#8220;easy&#8221; but somewhat inelegant route of serializing the object that GitVersion creates to file, and reading / de-serializing the file contents to object wherever I need it. For this too work, I had to put the `GitVersion.yaml` next to the .git folder (otherwise `GitVersion.exe` would not produce the correct output). Since the PS scripts are run from the solution folder, I serialized the object to a `gitVersionOutput.json` file, which shouldn&#8217;t interfere with anything else (and will only exist on the build server).

`$GitVersionOutput | Out-File gitVersionOutput.json`

Once I&#8217;ve gotten the new version number for the project / solution, I can run through each `project.json` file in the solution and substitute the version property&#8217;s value.

### Powershell : ConvertTo-Json might not produce the result you want

**The Context:**  
As mentioned in the previous step, once I&#8217;ve got the new SemVer version number, I can substitute the value in each of the `project.json` files in the solution. This will allow the build to use that value for each of the generated packages. Package and assembly version numbers will automaticaly be incremented, NuGet packages will be correctly versioned and the Git repo can then be tagged. Binaries, packages and repo will be synced to match specific point-in-tine inputs to outputs.

**The problem:**  
What would seem to be a simple step /powershell script turn out to be a load of work. The idea was to read each project.json with a combination of the `Get-Content` and the `ConvertFrom-Json` cmdlet, change the properties value, and rewrite the file with the `ConvertTo-Json` and `Out-File` cmdlets. Unfortunately it&#8217;s not straightforward. `ConvertTo-Json` won&#8217;t serialize correctly &#8211; deep trees will see some subtrees, like the dependencies subtrees converted to string instead of a tree.

In other words, running

`Get-Content -Raw project.json | ConvertFrom-Json | ConvertTo-Json | Out-File project.json`

doesn&#8217;t produce the correct output.

What was read as (shortened for brevity to the relevant parts)

`<br />
{<br />
"frameworks": {<br />
"dotnet": {<br />
"dependencies": {<br />
"System.ComponentModel": "4.0.0-beta-23109",<br />
"System.Data.Common": "4.0.0-beta-23109",<br />
"System.Runtime": "4.0.20-beta-23109",<br />
"System.Runtime.InteropServices": "4.0.20-beta-23109",<br />
"System.Runtime.Serialization.Primitives": "4.0.10-beta-23109"<br />
}<br />
}<br />
}<br />
}<br />
` 

Gets written as

`<br />
{<br />
"frameworks": {<br />
"dotnet": {<br />
"dependencies": "@{System.ComponentModel=4.0.0-beta-23109; System.Data.Common=4.0.0-beta-23109; System.Runtime=4.0.20-beta-23109; System.Runtime.InteropServices=4.0.20-beta-23109; System.Runtime.Serialization.Primitives=4.0.10-beta-23109}"<br />
}<br />
}<br />
}<br />
` 

Notice that dependencies is no longer an object with properties but a single string.

**The solution:**  
Two things should be considered. To read the json, `Get-Content` should have the -Raw switch to avoid using any other piped step (such as `Out-String`); `Get-Content -Raw project.json | ConvertFrom-Json` works correctly.  
The other thing, when serializing, is to include the `--depth` argument before writing the file : `ConvertTo-Json --depth 999 | Out-File project.json` , to avoid the deeper parts of the tree to be written as a single string value.

### Deployment &#8211; IIS might be looking for x64 apps because of the app pool

**The context:**  
This one drove a colleague of mine, who has never used IIS, mad. The context was a simple one. Deploying (x-copy) an app to a IIS based server. Our build doesn&#8217;t have this step setup yet (but hopefully it will, soon, with OctopusDeploy), and it&#8217;s also something kinda one-shot-ish since we were trying to deploy a branch&#8217;s snapshot with some mocks for validation. Esetially we had an X-copy to the server, and reference to an x86 version of a dnx beta.

**The problem:**  
After small changes to IIS (basic stuff), one last error was keeping the the site / app from displaying. IIS was looking for `dnx-clr-win-x64.1.0.0-beta5`, and not the x86 variant. We did not have, anywhere, a ref to x64.

**The solution:**  
This was solved with an AppDomain setting. In the used app Domain, there is a setting that allows 32bit apps to run. Changing the flag value to true corrected the problem. Unfortunately, this one was pretty hard to find. To Get there:

  1. In IIS choose the appdomain being
  2. Choose used and Advanced settings
  3. Change &#8220;Activate 32 bit applications&#8221; in the General properties group to `true`

### Powershell &#8211; Hidden vars don&#8217;t go out to the command line

**The context:**  
As previously mentioned, GitVersion is being used to calculate a new SemVer version of the artifacts being generated. The last step, after building, testing and pushing packages is to tag the repo, on the commit that generated the version, with the version number. This is generally done using a `git push` 

**The problem:**  
The origin argument in the command is a URL to the repo, and authentication info is necessary. In the case of TFS repos, a username/password combination is required. `git push` at the command line generally requests the user and password values in separate interactions in the shell. To avoid these interactions (since in the build it&#8217;s not possible or convenient), a single command should be used with all the required info. in this case, the url for origin can contain the user and pass before the url using the following format:

`git push https://:@` 

Passwords are sensitive things and we don&#8217;t want them coded in the script. We can use build variables (defined in the variables tab of the build job). TFS supports &#8220;hidden&#8221; variables that allow you to include sensitive info, and hide them from the UI and logs. These vars are generally available as environment variables throughout the build. While you can access normal variables through `${env:variableName}` references, hidden vars won&#8217;t get substituted correctly in the script, failing the authentication.

**The Solution:**  
Build variables can generally be referenced through the `${env:varname}` syntax, but hidden vars can&#8217;t. They can, though, be passed as arguments to a script. In this case their value is used internally as expected and still hidden (substituted by \****) in the logs. Script arguments are declared in the start of the script and defined in the arguments textbox in the step definition. in this case, the reference is in the form $(varName).

So for my case, I have 3 vars I define in the build &#8211; `tagUser`, `tagPass`, `tagRepoUrl` &#8211; where `tagPass` is hidden. At the very beginning of my powershell script I accept 3 arguments (by ordinal reference) which can be locally accessed as `$tagUser`, `$tagPass`, and `$tagRepoUrl`

`<br />
[string]$tagUser = $args[0]<br />
[string]$tagPass = $args[1]<br />
[string]$tagRepoUrl = $args[2]`

&nbsp;

In the arguments textbox I reference the build vars in order: `$(tagUser) $(tagPass) $(tagRepoUrl)`

[<img class="aligncenter size-full wp-image-1970" src="http://www.miguelalho.pt/wp-content/uploads/2015/08/args.png" alt="args" width="532" height="73" srcset="http://www.miguelalho.pt/wp-content/uploads/2015/08/args.png 532w, http://www.miguelalho.pt/wp-content/uploads/2015/08/args-300x41.png 300w" sizes="(max-width: 532px) 100vw, 532px" />][3]

&nbsp;

There&#8217;s also a GitHub issue for this one: <https://github.com/Microsoft/vso-agent-tasks/issues/388>

**Extra:**  
I probably can get the repo url through some other mechanism (maybe gitVersion or .git folder files or something) which I may explore and get rid of the `tagRepoUrl` variable all together. Seems redundant, just haven&#8217;t invested the time for that.

 [1]: http://semver.org/
 [2]: https://github.com/GitTools/GitVersion/
 [3]: http://www.miguelalho.pt/wp-content/uploads/2015/08/args.png