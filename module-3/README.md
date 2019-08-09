# Module 3: Configure your Windows Server 2019 Dev Environment

**Time to complete:** 20 minutes

**Services used:**
* [AWS EC2](https://aws.amazon.com/ec2/)
* [AWS CloudFormation](https://aws.amazon.com/cloudformation/)
* [AWS Identity and Access Management (IAM)](https://aws.amazon.com/iam/)

## Overview

In this module, we will use AWS EC2 to launch and configure our Windows Server 2019 Dev Environment. You can perform all followings on your local Windows desktop or laptop and skip provisioning the new EC2 instance.

<a id='dev-env'></a>
### Your Windows Development Environment
Your Windows workstation environment needs to have the following tools configuration.

**A Dev-Environment with the following development tools.** 
1. Visual Studio 2019 Community Edition (https://visualstudio.microsoft.com/vs/community)
2. .NET Core SDK (https://dotnet.microsoft.com/download/visual-studio-sdks)
3. AWS Tools for Visual Studio (https://aws.amazon.com/developer/language/net/tools/)
4. AWS Command Line Tools (aws)  (https://aws.amazon.com/cli/)
5. .NET Portability Analyzer (https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer)
6. Cmder (https://cmder.net) 


**Your Windows Workstation - A Remote AWS Windows Server 2019 AMI Instance**

1. For this lab, we utilize an EC2 hosted **Microsoft Windows Server 2019 Base with Containers** bare-metal instance. This Amazon Machine Image (AMI) is preconfigured to support Docker Containers. We will use bare-metal because we need the physical machine to run the Docker with Linux Daemon. 
2. Don't know how to create an EC2 instance?  Please refer to step-wise documentation at (https://aws.amazon.com/getting-started/tutorials/launch-a-virtual-machine/). 
3. When creating your AMI instance, **be sure to choose an attached disk size of 60 GByte**.  The default of 30 GByte may be too small to contain our software packages.
4. When creating your AMI instance, be sure to select the **Microsoft Windows Server 2019 Base with Containers** AMI-type.  We recommend utilizing a **i3.metal** host-machine type. 
5. Refer to  <a href="#appendix-a">**Appendix A**</a> herein for instructions on accessing your AMI instance via Remote Desktop Protocol (RDP). 

**Proceed with installation of the following tools only after completion of the above EC2 instance creation. Lab instructions are to be executed from a RDP client application connection to that EC2 instance.**

![Windows Server 2019 AMI](/images/module-3/WindowsServer2019-AMI-1.jpg)


## Update and configure your Windows Workstation

We herein provide guidance on manual configuration of your Windows Server 2019 workstation.  This approach could be automated, but there is instructive value in a manual configuration as several tools have dependencies that must be established for development purposes.

### Windows Updates

1. Login to your Windows Server 2019 Dev Environment per <a href="#appendix-a">**Appendix A**</a>. 
2. Select the `Start` menu and type `update`.  
3. Follow the *Check for Update* control panel instructions.

### Install Visual Studio 2019 Community Edition

1. Browse to *https://visualstudio.microsoft.com/vs/community* and download Visual Studio for Windows.
2. Within the Visual Studio Installer application, select the following features:
    * ASP.NET and Web development
    * .NET Desktop development
    * .NET Core Cross Platform development

### Install Extensions for Visual Studio

1. Open Visual Studio.  Select the *Extensions* menu and the *Manage Extensions* menu-item.
2. Select the *Online* and *Visual Studio Marketplace* category from the left-side of Manage Extensions window.
3. Search for **AWS Toolkit for Visual Studio 2017 and 2019**.  Select the resultant extension package.
4. Search for **.NET Portability Analyzer**.  Select the resultant extension package.
5. Close Visual Studio in order to start the installation of these extensions.

![VisualStudioExtension](/images/module-3/WindowsAMI-VStudioExtensions.jpg)

### Configure Visual Studio with your AWS Account Credentials

1.  See how to configure the AWS Toolkit for Visual Studio with your AWS Account Credentials [here](https://docs.aws.amazon.com/toolkit-for-visual-studio/latest/user-guide/credentials.html).

### Install the AWS Command Line Tools

1. Browse to https://aws.amazon.com/cli and select the 64-bit option from the Windows download section (upper right-side of the page).
2. Open a Command Shell (or open Cmder.exe) and configure the AWS CLI with your AWS account credentials.  See guidance [here](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html).
3. Ensure that your selected region matches the one provided within your Visual Studio configuration above.

### Install Docker Desktop
1. Download the installer file from https://download.docker.com/win/stable/Docker%20for%20Windows%20Installer.exe
2. Follow the installation prompts and complete the installation.

### (Optional) Install the Cmder Utility

1. Download the *full package* from https://cmder.net
2. Unzip to *c:\cmder* and configure a Desktop short-cut to *c:\cmder\Cmder.exe*.

### (Optional) Install the FireFox Web Browser

1. Use the Windows Server *Server Manager* application (accessible from the Start menu) which should be displayed automatically upon logon.
2. Select the *Local Server* from the left-side panel.
3. Scroll down to the *Properties* section.  Find the *IE Enhanced Security Configuration* and select the *Off* setting.
4. Open the *Internet Explorer* application.  Browse to *https://www.mozilla.org/firefox* and select the *Download Firefox* option.

That concludes Module 3.

**[Proceed to Module 4](../module-4/README.MD)**


<a id='appendix-a'></a>
## Appendix A : Connect to your Windows Server 2019 Development Environment

### Connect to Your EC2 Development Instance

You should have already created an EC2 instance.  You will use an RDP client (such as Remote Desktop Connection, mstc.exe, included with most versions of Windows) to connect to your server. 

1. Open the *Instances* view within the *AWS EC2 Console*.  Select your Windows Server instance and press *Connect* as illustrated.

![AWS EC2 Console](/images/module-3/WindowsAMI-Connect-1.jpg)

2. From the *Connect to your Instance* dialog window, press *Download Remote Desktop File*.

![Downlaod RDP File](/images/module-3/WindowsAMI-Connect-2.jpg)

3. Subsequently press the *Get Password* button.  Save this password to a text file for subsequent copy/paste usage.

![Get Password](/images/module-3/WindowsAMI-Connect-3.jpg)

If you are running Mac OS X, <a href="#rdp-MACLinux">skip to the MAC specific section below</a>.

### Windows Users - Connect to Your Windows Instance

1. Using Windows Explorer, locate the RDP file previously downloaded. 
2. Click the file to open the Remote Desktop Protocol application.  
3. Provide the previously saved Administrator account password to logon to the remote Windows instance.

<a id='rdp-MACLinux'></a>
### MAC/Linux Users - Connect to Your Windows Server 2019 Development Environment 

You can install the **Jump Desktop** application or the free **Microsoft Remote Desktop** application from the AppStore.  

1. Open your RDP client application, then configure a new RDP connection, entering the following configuration obtained from the EC2 Management Console, *Connect to your Instance* dialog window.

* **Server:** Paste the value of **Public DNS** address of your remote Windows instance.
* **Username:** `\Administrator`
* **Password:** Paste the value of **WindowsPassword**

1. Click **Connect**.

<a href="#dev-env">Return to the top of this file</a>



### [AWS Developer Center](https://developer.aws)

