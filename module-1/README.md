# Module 1: Configure Your Linux Dev Environment

**Time to complete:** 15 minutes

**Services used:**
* [AWS EC2](https://aws.amazon.com/ec2/)
* [AWS CloudFormation](https://aws.amazon.com/cloudformation/)
* [AWS Identity and Access Management (IAM)](https://aws.amazon.com/iam/)

## Overview

In this module, we will use AWS EC2 to launch and configure our Linux Dev Environment.

<a id='dev-env'></a>
### Your Linux Dev Environment

**NOTE, 7/01 : We may need to utilize an Ubuntu based AMI instead in order to utilize the [Kubernetes Dashboard](https://docs.aws.amazon.com/eks/latest/userguide/dashboard-tutorial.html).  I'm having some difficulty getting this application to install on our Windows Server 2019 AMI. **

Your Linux workstation environment needs to have the following tools configuration.

**A Dev-Environment with the following development tools.** 
1. .NET Core (dotnet)  (https://dotnet.microsoft.com/download/dotnet-core)
2. AWS Command Line Tools (aws)  (https://aws.amazon.com/cli/)
4. Docker Tools and Services (docker) (https://www.docker.com/)
5. Docker Compose (docker-compose)  (https://docs.docker.com/compose/overview/)
6. Nginx (https://aws.amazon.com/amazon-linux-2/faqs/#Amazon_Linux_Extras)
7. EKS Cluster Management Client Utility (eksctl) (https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html)
8. EKS Cluster API Client Utility (kubectl) (https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html)

**Your Linux Workstation - A Remote AWS Linux AMI Instance**
1. For this lab, we utilize an EC2 hosted **.NET Core 2.1 with Amazon Linux 2 - Version 1.0** instance.  This Amazon Machine Image (AMI) is preconfigured with both **AWS CLI** and the **dotnet** runtime.  It doesn't include **EKS Utilities** nor **docker**, but we will install those.
2. Don't know how to create an EC2 instance?  Please refer to step-wise documentation at (https://aws.amazon.com/getting-started/tutorials/launch-a-virtual-machine/). 
3. When creating your AMI instance, be sure to select the **.NET Core 2.1 with Amazon Linux 2 - Version 1.0** AMI-type.  We recommend utilizing a **t2.large** or better host-machine type. 
4. Refer to  <a href="#appendix-a">**Appendix A**</a> herein for instructions on accessing your AMI instance via Secure Shell (SSH). 

**Proceed with installation of the following tools only after completion of the above workstation guidance.  Lab instructions are to be executed from a terminal window remoting into your EC2 instance via SSH.**

**Update and configure your Linux Workstation**

1. Using your local Terminal application, login to the remote Lab Workstation.
``` shell
ssh -i MYKEYPAIR.pem ec2-user@myEc2IpAddress
```
```
login examples:
ssh -i ~\downloads\mykeypair.pem ec2-user@ec2-34-219-242-224.us-west-2.compute.amazonaws.com
ssh -i %HOME%\Downloads\mykeypair.pem ec2-user@ec2-34-219-242-224.us-west-2.compute.amazonaws.com
```

2. Update the current image package configuration.
``` shell
sudo yum update -y
```

3. Install Docker Tools and Services for Container-izing our application.
``` shell
sudo yum install -y docker
```
``` shell
sudo service docker start
```

4. Add the **ec2-user** account to the **docker group** to enable use without root privileges (i.e. sudo).
``` shell
sudo usermod -a -G docker ec2-user
```

5. Install the Nginx service supporting our Reverse Proxy configuration.
``` shell
sudo amazon-linux-extras install nginx1.12
``` 

6. Install Docker Compose for testing our containers via localhost. Ensure that `docker-compose` has the execute attribute enabled.
``` shell
curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

7. Logout and login again in order to effect the `ec2-user` user group privilege change made previously.  Use the up-arrow to pull-up a previous `login` command line.
``` shell
logout
```
```
login examples:
ssh -i ~\downloads\mykeypair.pem ec2-user@ec2-34-219-242-224.us-west-2.compute.amazonaws.com
ssh -i %HOME%\Downloads\mykeypair.pem ec2-user@ec2-34-219-242-224.us-west-2.compute.amazonaws.com
```

8. Install the `eksctl` utility for EKS Cluster Management.  Move the extracted utility to `usr/local/bin`.
``` shell
sudo curl --silent --location "https://github.com/weaveworks/eksctl/releases/download/latest_release/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
```
``` shell
sudo mv /tmp/eksctl /usr/local/bin
```

9. Install the `kubectl` utility for communicating with the EKS Cluster API server.
``` shell
sudo curl -L https://amazon-eks.s3-us-west-2.amazonaws.com/1.12.7/2019-03-27/bin/linux/amd64/kubectl -o /usr/local/bin/kubectl 
```
``` shell
sudo chmod +x /usr/local/bin/kubectl
```

10. Test these installed packages.
``` shell
aws --version
```

``` shell
dotnet --version
```

``` shell
docker --version
```
``` shell
docker ps
```

``` shell
docker-compose --version
```

``` shell
eksctl version
```

``` shell
kubectl version --short --client
```

**Configure the AWS CLI**

Prior to invoking AWS Services, ensure that your **AWS CLI** tooling is configured with your AWS account credentials.  

Modify the prompting command line as indicated and also specify your default [region](https://docs.aws.amazon.com/general/latest/gr/rande.html).  
Specifying the region here is important because subsequent commands leverage the `--region` attribute implicitly via your configured **AWS CLI** profile.

``` shell
aws configure
  AWS Access Key ID [None]: <PASTE YOUR ACCESS KEY ID HERE>
  AWS Secret Access Key [None]: <PAST YOUR ACCESS KEY HERE>
  Default region name [None]: <CHOOSE YOUR PREFERRED REGION>
  Default output format [None]: json
```


## Next

Please proceed to the next lesson.

**[Proceed to Module 2](../module-2/README.MD)**


<a id='appendix-a'></a>
## Appendix A : Connect to your AWS Linux AMI Development Environment

### <i class="fab fa-windows" aria-hidden="true"></i> Windows Users: Using SSH to Connect

<i class="fas fa-comment" aria-hidden="true"></i>Steps 1-4 below are for Windows users only.  If you are using a Mac or Linux lab computer, <a href="#ssh-MACLinux">skip to the next section</a>.

For Windows users, there are several options for client terminal (i.e. shell) access to an AWS Linux AMI instance. This guide recommends using the Open Source application, **Cmder**.  Other options include [PuTTY](http://putty.org), or [Git Bash](https://gitforwindows.org). 

1. From your Windows workstation, install the **Cmder** application (https://cmder.net). Be sure to select the "Download Full" option which includes git-for-windows support and Unix style commands. 
2. We recommend that you extract the Cmder.zip file to c:\Cmder and add that folder to your system PATH environment variable or create a short-cut to `C:\Cmder\Cmder.exe` on your desktop.
3. Launch a Cmder session by clicking the `C:\Cmder\Cmder.exe` file from Windows Explorer. This is your terminal window.
4. You will use SSH from a **Cmder** window to logon to your Amazon EC2 instance as follows. Proceed to step number 2 below.

<a id='ssh-MACLinux'></a>
### Windows,<i class="fab fa-windows" aria-hidden="true"></i> Mac, <i class="fab fa-apple" aria-hidden="true"></i> and Linux <i class="fab fa-linux" aria-hidden="true"></i> Users

1. On your local client machine, open a terminal window.
2. Using your AWS Account, logon to the AWS Management Console.
3. Select the **EC2** service.
4. From the EC2 console, see your previously created **Amazon ECS-optimized Amazon Linux 2 AMI** instance within the list of hosted AMI instances.
5. Select your machine image and right-click to open a pop-up menu. Select the **Connect** option.
6. Note the displayed EC2 instance IP Address, URL, and SSH command line example. Copy this SSH command line to a text editor and correctly specify your **PEM** key-pair file name.
7. Execute the following commands from your terminal window.  Your key-pair file-name and Ec2IpAddress will differ from the example.

```shell
chmod 400 MYKEYPAIR.pem
ssh -i MYKEYPAIR.pem ec2-user@myEc2IpAddress

example:
chmod 400 ~\downloads\mykeypair.pem
ssh -i ~\downloads\mykeypair.pem ec2-user@ec2-34-219-242-224.us-west-2.compute.amazonaws.com
```
8. Type `yes` when prompted to allow a first connection to this remote SSH server.

Because you are using a key pair for authentication, you will not be prompted for a password.

<a href="#dev-env">Return to the top of this file</a>

<a id='ssh-after'></a>


### [AWS Developer Center](https://developer.aws)
