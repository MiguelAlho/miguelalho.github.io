---
title: Upgrading vNext MVC6 Apps From Beta4 to Beta5
author: Miguel Alho
type: post
date: 2015-06-05T09:40:29+00:00
url: /upgrading-vnext-mvc6-apps-from-beta4-to-beta5/
categories:
  - 'Code &amp; IT'
tags:
  - ASP.Net vNext
  - MVC6

---
It&#8217;s been a while, but my last few sprints have been focused on working with ASP.NET 5 MVC6, namely evaluating what is going on in order to protoype an app front-end. Working with Beta versions is definitely challenging &#8211; there are plenty of moving parts and a lack of documentation. Fortunately, the term &#8220;community&#8221; is one we can apply more to what is going on with Microsoft&#8217;s iniciative. I&#8217;ve been pretty much googling stuff, deducing concepts, and hogging the [Jabber #AspNetvNext][1] chat room, where, fortunately, I&#8217;m getting answers I need.

I started developing the prototype in Beta4, and decided to migrate to Beta5 before starting to add even more parts to the project. Unfortunately, upgrading isn&#8217;t the easiest thing in the world, but it is possible.

## 1. Upgrade your DNVM and DNU

Your runtime environment should be one that can support Beta5 correctly. At this time, the latest is Beta6-10383. To upgrade, you can run in the command line window:

`@powershell -NoProfile -ExecutionPolicy unrestricted -Command "&{$Branch='dev';iex ((new-object net.webclient).DownloadString('https://raw.githubusercontent.com/aspnet/Home/dev/dnvminstall.ps1'))}"`

This will update dnvm itself, if you are still in Beta4. As of the beta5 or 6 version, a self-update command is available.

With dnvm updated, you should upgrade the dnu versions.

`dnvm upgrade`

should pull and install the most recent stable dnu. You can indicate a specific version also:

`dnvm install 1.0.0-beta5-11904`

or

`dnvm install 1.0.0-beta5-11904 -r coreclr`

if you want to choose/specify the clr type. You can also pull the most recente beta build available using 

`dnvm upgrade -Unstable`

## 2- Update your global.json

In your solution&#8217;s global.json, update the sdk version value:

`"sdk": {<br />
"version": "1.0.0-beta5-11904"<br />
}`

## 3- Update packages in the projects

At this time, you can update package refs in your projects to Beta5 versions. One thing to consider, though, is that from Beta4 to Beta5, a lot of changes have occurred, especially in the namespaces for some class. For instance, pretty much every package set now has an `*.Abstractions` package with base classes and interfaces, separated from their implementations. I would at this point recommend excluding as many projects as it would make sense to and to update gradually, traversing the dependency chain. This is simply to reduce the amount of focus required to analyse output errors.

The main NuGet feed contains a large set of packages, but if you want to try using the latest and greatest, you can pull from the myGet feed. In order to do so, at the same level as your `global.json` and `.sln` file, add a `.nuget/` folder and a NuGet.config file (similar to previous solution types). Add the following :

`<?xml version="1.0" encoding="utf-8"?><br />
<configuration><br />
<packageSources><br />
<!-- Add this repository to the list of available repositories --><br />
<add key="AspNet MyGet" value="https://www.myget.org/F/aspnetvnext/" /><br />
<add key="nuget.org" value="https://www.nuget.org/api/v2/" /><br />
</packageSources><br />
</configuration>`

I prefer having this in the solution, and committed to source control, as the entire team can share this definition in the solution context AND you won&#8217;t have to manually configure VS to use the repository (which would make it available to every solution you use, which you may or may not want).

### 3.1 Abstractions

Alot of things are changing, so some car is required, and code will require updates. The most common change that affected me were namespace changes (as mentioned in https://github.com/aspnet/Announcements/issues/14). Pretty much all of the repos have some Abstractions namespace associated to it, now. That&#8217;s actually a pretty good thing , in my opinion. If you are not tied to a specific implementation of abstract class or interface, the only reference you are required to have in class libs is the abstractions package. That makes use of DI more straight foward in the application project, which would be where the actual implementation is referenced. Other than there, you can always program against the abstraction and Mock as you wish in tests.

Some common abstraction refs I required where:

&nbsp;

<table>
  <tr>
    <td>
      Microsoft.Framework.Caching.Abstractions
    </td>
    
    <td>
      Microsoft.Framework.Caching
    </td>
    
    <td>
      IExperationTrigger
    </td>
  </tr>
  
  <tr>
    <td>
      Microsoft.Framework.FileProviders.Abstractions
    </td>
    
    <td>
      Microsoft.Framework.FileProviders
    </td>
    
    <td>
      IFileInfo, IDirectoryContents
    </td>
  </tr>
  
  <tr>
    <td>
      Microsoft.Framework.DependencyInjection.Abstractions
    </td>
    
    <td>
      Microsoft.Framework.DependencyInjection
    </td>
    
    <td>
      IServiceCollection
    </td>
  </tr>
  
  <tr>
    <td>
      Microsoft.Framework.Runtime.Abstractions
    </td>
    
    <td>
      Microsoft.Framework.Runtime
    </td>
    
    <td>
      ILibraryManager
    </td>
  </tr>
  
  <tr>
    <td>
      Microsoft.AspNet.Http.Abstractions
    </td>
    
    <td>
      Microsoft.AspNet.Http
    </td>
    
    <td>
      IApplicationBuilder
    </td>
  </tr>
</table>

### 3.2 &#8211; Configuration

Configuration classes have changes also (https://github.com/aspnet/Announcements/issues/25 , https://github.com/aspnet/Announcements/issues/26). There is a preference towards using `ConfigurationModel` classes / packages, to tie into the `OptionsModel`, instead of the previous Configuration (though configuration builder is in Configuration). Configuration can be loaded into `IOption` objects in DI by using the following code:

`var applicationEnvironment = services.BuildServiceProvider().GetRequiredService<IApplicationEnvironment>();<br />
var configurationPath = Path.Combine(applicationEnvironment.ApplicationBasePath, "config.json");</p>
<p>var configBuilder = new ConfigurationBuilder()<br />
.AddJsonFile(configurationPath)<br />
.AddEnvironmentVariables();<br />
var configuration = configBuilder.Build();<br />
services.Configure<AppSettings>(configuration.GetConfigurationSection("AppSettings"));`

The path to the config file should now be an absolute path as mentioned in https://github.com/aspnet/Announcements/issues/13

### 3.3 &#8211; _GlobalImport.cshtml

If you use `_GlobalImport.cshtml` for your views, the file name convention has changed to search for `_ViewImports.cshtml` (plural) as mentioned in https://github.com/aspnet/Announcements/issues/27 . That file should therefore be renamed.

### 3.4 &#8211; Configure()

For some unexplained reason, I was having trouble using Use* extension methods on IApplicationBuilder such as UseStaticFiles and UseCookieAuthentication. For some odd reason, the runtime shouldn&#8217;t load the UseMiddleware<>() method those extensions use.

`System.MissingMethodException<br />
Método não encontrado: 'Microsoft.AspNet.Http.Authentication.SignInContext Microsoft.AspNet.Authentication.AuthenticationHandler.get_SignInContext()'.`

[<img class="aligncenter size-large wp-image-1947" src="http://www.miguelalho.pt/wp-content/uploads/2015/06/errorUseMiddleware-1024x601.png" alt="errorUseMiddleware" width="700" height="411" srcset="http://www.miguelalho.pt/wp-content/uploads/2015/06/errorUseMiddleware-1024x601.png 1024w, http://www.miguelalho.pt/wp-content/uploads/2015/06/errorUseMiddleware-300x176.png 300w, http://www.miguelalho.pt/wp-content/uploads/2015/06/errorUseMiddleware.png 1099w" sizes="(max-width: 700px) 100vw, 700px" />][2]

&nbsp;

For some reason some packages were not playing nice. The workarounds I used:

For `UseStaticFiles()`, I just used what the extension actually does:

`//app.UseStaticFiles();<br />
app.UseMiddleware<StaticFileMiddleware>(new StaticFileOptions());`

For `UseCookieAuthentication()`, using `Beta6` packages for `Microsoft.AspNet.Authentication` and `Microsoft.AspNet.Authentication.Cookies` solved the problem.

 [1]: https://jabbr.net/#/rooms/AspNetvNext
 [2]: http://www.miguelalho.pt/wp-content/uploads/2015/06/errorUseMiddleware.png