# Using .NET Standard from .NET Framework

This repo has sample projects that show how to use .NET Standard from a .NET
Framework application.

## Requirements

Generally speaking, using libraries targeting .NET Standard in an application
targeting .NET Framework requires the application project to include a NuGet
reference for .NET Standard (`NETStandard.Library`). This ensures that the right
set of assemblies are included with the application.

In Visual Studio 2015, the default way of consuming NuGet packages from .NET
Framework projects is via `packages.config`. I don't recommend this path as
this means that all assemblies are directly injected into the application
project, which will significantly bloat your project file. Instead, I recommend
you use `project.json`. To do this, perform the following steps:

1. Uninstall all packages (if you're still using `packages.config`)
2. Delete the empty `packages.config`
3. Add `project.json` file with this content:

   ```json
   {
     "dependencies": {
       "NETStandard.Library": "1.6.0"
     },
     "runtimes": {
       "win": {}
     },
     "frameworks": {
       "net462": {}
     }
   }
   ```

Please note that you can generally depend on the latest version of the
`NETStandard.Library` package, but you need to make sure to keep the framework
moniker in sync with the version of .NET Framework your app is targeting, i.e.
when you're targeting .NET Framework 4.6.1, you need to make sure to use
`net461` instead.

## This feels clumsy

Yes it is. We're planning on addressing this in two ways:

* We're replacing `project.json` with an MSBuild based solution in Visual
  Studio 2017. You'll still need to add the reference to `NETStandard.Library`,
  but you no longer have to mess with the way packages are being represented nor
  having to manually keep targeting information in sync.

* We're planning to update .NET Framework so that future versions of it come
  with built-in support for .NET Standard, in which case the reference will no
  longer be needed.
