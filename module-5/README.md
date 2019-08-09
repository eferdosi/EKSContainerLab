# Module 5 - Enable .NET Core Application Compatibility

**Time to complete:** 20 minutes

### Overview

In this module, we introduce a fix for the observed .NET Core compatibility issues.  Also, let's experiment with tooling that will provide us with useful compatibility insights, not only on this sample project but with much more complex migration scenarios.

There are several tools available when [porting existing code to .NET Core](https://docs.microsoft.com/en-us/dotnet/core/porting/index).  

* Use the [Microsoft.Windows.Compatibility](https://docs.microsoft.com/en-us/dotnet/core/porting/windows-compat-pack) NuGet package to import .NET extension classes that resolve numerous .NET Core incompatibilities.

* Use the [API Analyzer](https://devblogs.microsoft.com/dotnet/introducing-api-analyzer/) to identify issues when targeting a cross-platform migration (e.g. when moving the code from Windows to Linux).

* Use the [.NET Portability Analyzer](https://docs.microsoft.com/en-us/dotnet/standard/analyzers/portability-analyzer) to discover incompatibilities when targetting multiple frameworks and platforms.


## Import the Microsoft.Windows.Compatibility NuGet Package

This package will help to resolve a myriad of class library issues including the *System.Data.DataTable.AsEnumerable()* error observed in the last module.

1. Open the *Package Manager Console* from the **Tools** --> **NuGet Package Manager** menu. The Package Manager Console should appear as a tab'ed window alongside the Output tab within Visual Studio.

2. At the PM prompt, enter the following command.

``` shell
PM> Install-Package Microsoft.Windows.Compatibility
```

3. Note that the package installs as a NuGet dependency within the project as illustrated.

![WindowsCompatibilityPackage](/images/module-5/MicrosoftWindowsCompatibilityNuget-1.jpg)


## Rebuild and Test the Solution

1. Right-click the Solution node within Solution Explorer and select *Rebuild*.

2. Note that the previously seen build error message has disappeared.  

3. Now, press **F5** to launch the Debugger.  Note the opening of a Debugger Console window and transformation of the sample input file.

![Debug Output](/images/module-5/DebugOutput-1.jpg)


## Configure the .NET Portability Analyzer

1. Recall that you previously installed the *.NET Portability Analyzer* Visual Studio extension during module 3.  Now, let's configure this extension to investigate our solution for code that would be incompatible with various *.NET Core* target versions.  

2. Right-click the *VlanMigrate* project name within the Solution Explorer.  Then select the **Portability Analyzer Settings** menu option.

![Open Portability Analyzer](/images/module-5/OpenPortabilityAnalyzerSettings-1.jpg)

3. Configure the options corresponding to the following image.  Ensure that only the **html** output format option is selected.

![Configure Portability Analyzer](/images/module-5/PortabilityAnalyzerConfigure-2.jpg)


## Run the .NET Portability Analyzer

1. Right-click the *VlanMigrate* project name within the Solution Explorer. Then select the **Analyze Project Portability** menu option.

![Analyze Portability](/images/module-5/AnalyzePortability-1.jpg)

2. Upon completion, a results Windows will appear.  Press the *Open Report* button.

![Open Report](/images/module-5/OpenPortabilityAnalyzerReport-1.jpg)

3. An HTML formated report should appear within your web browser.  Note that even with the fix provided, there remain incompatibilities with specific framework versions.  

![Portability Results](/images/module-5/PortabilityResults-1.jpg)


## Next

Please proceed to the next lesson.

**[Proceed to Module 6](../module-6/README.MD)**


### [AWS Developer Center](https://developer.aws)
