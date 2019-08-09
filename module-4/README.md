# Module 4 - Build the Sample .NET Application


**Time to complete:** 10-15 minutes


### Overview

In this module, we will download our sample application project files, demo the solution, and begin the process of migrating to .NET Core from .NET Framework.

Let's begin with a discussion of the project. Our sample is derived from a real-world enterprise project in which a Data-Center Network Operations team sought to replace a legacy Juniper routing infrastructure with new Cisco routers.  Of course, replacing a network fabric requires a great deal of planning and concise step-wise execution.  A key concern was how to move and transform hundreds of Virtual LAN (VLAN) configurations from Juniper to Cisco script format.  Ultimately, the network team asked for a custom application that would take as input Juniper VLAN configuration statements and transform those into new Cisco VLAN configuration statements, accounting for transformation rules and for router redundancy considerations. The resultant auto-generated VLAN scripts could then be merged with full-device configuration scripts and thus accomplish a reliable, repeatable, router configuration. 

The following image illustrates this specific enterprise problem-solution scenario.

![Solution Overview](/images/module-4/ApplicationScenario-1.jpg)


### Download the Source Files

You should already have a Remote Desktop session open onto your Windows Server 2019 Dev Workstation (if not, see module 3).

1. Open a `C:\Cmder\Cmder.exe` command shell and create a project folder.

``` shell
mkdir c:\myprojects
```
``` shell
cd /d c:\myprojects
```

2. Download and unzip the project files. 

``` shell 
curl -L -o VLanMigrationProject.zip https://github.com/RecursiveLoop/EKSContainerLab/blob/master/module-4/source/VLanMigrationProject.zip?raw=true
```
``` shell
unzip VLanMigrationProject.zip
```


### Open the Visual Studio Solution File

1. Use VLanMigrate solution folder.

``` shell
"C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\Common7\IDE\devenv.exe" vlanmigrate.sln
``` 

### Run the Demo Build

The solution files include a `DEMO` subfolder with a prebuilt version of this application.  This is a .NET Framework 4.x version build of the application.

1. Within Visual Studio Solution Explorer, right-click on the `DEMO` folder and select *Open Folder in File Explorer* from the menu.

![Open the DEMO Folder](/images/module-4/RightClickDemoFolder-1.jpg)

2. Click on the `CLICK-HERE` short-cut to open a Command window displaying application usage instructions.

![Open short-cut](/images/module-4/ClickOnCLickHere-1.jpg)

3. Read the usage instructions.  Then, select the example command line and copy it to the command prompt as illustrated.

![Copy the Command Line](/images/module-4/LaunchDemo-2.jpg)

4. The application should parse the `SampleVLANs.txt` file and produce output corresponding to the following.  See the output files within the  auto-generated `.\SampleVLANs` folder.  Two files are generated, one for each router pair.

![View Results](/images/module-4/LaunchDemo-3.jpg)


### Build the Solution as Configured

1. Within Visual Studio Solution Explorer, right-click the `VLanMigrate` project name (it should appear bolded).  Select the *Properties* menu item.  
 
2. Note that the *Target Framework* property is currently set to *.NET Core 2.1*.  At the time of this writing, *.NET Core 2.2* was the latest version released.  But also, our target platform, the **.NET Core 2.1 with Amazon Linux 2 - Version 1.0** AWS AMI is configured to support only *.NET Core 2.1*.  Hence, we will target the *.NET Core 2.1* SDK version.

![View Properties](/images/module-4/ViewSolutionProperties-1.jpg)

3. Press `F5` to build the Solution.  Or, right-click the *Solution 'VLanMigrate'* node in the Solution Explore and select *Rebuild Solution*.

4. Observe the resultant build error list. 

![View Build Errors](/images/module-4/ViewBuildErrorList-2.jpg)

5. Click on the error within the Error List and see the resultant code-section that is incompatible with the *.NET Core 2.x* System.Data class library.

![View Incompatible Code](/images/module-4/ViewIncompatibleCode-2.jpg)

6. This is the easy case with .NET Core migrations.  Analyzing the code at line 188 in file *Program.cs* indicates that the *System.Data.DataTable.AsEnumerable()* function is missing from the *.NET Core 2.x* class library. Unfortunately, similar *Rows & Columns* type code is common-place with enterprise applications.  In the next module, we will introduce a fix for this build issue.

## Next

Please proceed to the next lesson.

**[Proceed to Module 5](../module-5/README.MD)**


### [AWS Developer Center](https://developer.aws)
