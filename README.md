nuget.restore.issue
===================

This illustrates a simple problem with NuGet restore and VisualStudio.

To reproduce:

1. Clone this project
2. Open the solution in VisualStudio
3. Build => NuGet restore kicks in and restores packages
4. Attempt to edit code => completion is broken
4. Restart VisualStudio
5. Build => OK and now completion is OK

## Links

* [NuGet tracker](https://nuget.codeplex.com/workitem/4187)
* [WebSharper tracker](https://bitbucket.org/IntelliFactory/websharper/issue/81/nuget-build-without-committing-packages)

## Rationale

Suppose we created a NuGet package (BuildPack) with `build/*.targets`. The intent is to
extend build logic of projects importing this package.

Then, we commit one such project (Lib/Lib.csproj) to source control. But we do not commit
packages/ as NuGet package restore should take care of that.

However, what happens on actual clone+open, is this: VisualStudio first imports the project,
skips loading BuildPack targets, and then does the package restore.  So while packages/ project
gets populated, targets are not imported until the next build.  Hence the need for restart.

