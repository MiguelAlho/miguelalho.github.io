---
title: Merging assemblies with ILRepack
author: Miguel Alho
type: post
date: 2017-12-18T15:55:05+00:00
url: /merging-assemblies-with-ilrepack/
---
Typically, in .NET, to merge assemblies you would use ILMerge. [ILRepack][1] is another project you can use and that has had updates recently (and a lot of activity on Github).

I&#8217;ve found it useful to merge the lot of Dlls and Exe that can compose a simple app into a single executable file (think containers but without Docker), which facilitates using and distributing it. The new `.csproj` file format is a great simplification and allows for tasks (as in the previous versions) in a smaller more understandable file.

To use ILRepack in a project, here are the steps you can perform to include the task during your build:

1 &#8211; add the `ILRepack` package to the project you wish to merge files in  
2 &#8211; validate that the `AssemblyName` element is defined in the property groups in your `.csproj` file  
3 &#8211; in the `.csproj`, add the following target element

<pre>&lt;Target Name="ILRepack" AfterTargets="Build"&gt;
&lt;!-- The ILRepack task will grab all build ouput assemblies
and merge them into a single exe file for easy distribution and integration
--&gt;
&lt;ItemGroup&gt;
&lt;ILRepackPackage Include="$(NuGetPackageRoot)\ilrepack\*\tools\ilrepack.exe" /&gt;
&lt;NetStandardLocation Include="$(NuGetPackageRoot)NETStandard.Library\2.0.1\build\netstandard2.0\ref" /&gt;
&lt;/ItemGroup&gt;

&lt;Error Condition="!Exists(@(ILRepackPackage-&gt;'%(FullPath)'))" Text="You are trying to use the ILRepack
 package, but it is not installed or at the correct location" /&gt;
&lt;Exec Command="@(ILRepackPackage-&gt;'%(fullpath)') /out:$(OutputPath)merged\$(AssemblyName).exe 
/wildcards /lib:@(NetStandardLocation) /target:exe $(OutputPath)$(AssemblyName).exe $(OutputPath)*.dll" /&gt;
&lt;/Target&gt;
</pre>

3.1 &#8211; the `NetStandardLocation` element is only required if you are targeting it or including a package that targets it.  
3.2 &#8211; the `/lib:@(NetStandardLocation)` argument in the Exec command is only required by 3.1  
3.3 &#8211; adjust the `/out` argument in the command to whatever you require.

4 &#8211; If you want to include the output in a package, define the package elements in a propertyGroup and include:

<pre>&lt;GeneratePackageOnBuild&gt;true&lt;/GeneratePackageOnBuild&gt;
&lt;!--we do not want build outputs integrated into the resulting package, only the IL merged file --&gt;
&lt;IsTool&gt;True&lt;/IsTool&gt;
&lt;IncludeBuildOutput&gt;False&lt;/IncludeBuildOutput&gt;
&lt;/PropertyGroup&gt;

&lt;ItemGroup&gt;
&lt;!-- nuget package files info: --&gt;
&lt;!--we do not want build outputs integrated into the resulting package, only the IL merged file --&gt;
&lt;Content Include="$(OutputPath)merged\$(AssemblyName).exe" PackagePath="tools\" /&gt;
&lt;/ItemGroup&gt;
</pre>

And that&#8217;s pretty much it. If you run build, ILRepack will be executed and build the merged file, and MSBuild will also wrap it in a NuGet package if you include the packaging step.

 [1]: https://github.com/gluck/il-repack