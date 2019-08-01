# Module 6 - Dockerize the Sample Application


**Time to complete:** 20 minutes

**Services used:**


### Overview

In this module, we will add Docker support to our app and containerize it. 

If you are new to Docker or using Docker with .NET, it is recommended to read [this first](https://docs.microsoft.com/en-us/dotnet/core/docker/intro-net-docker) to give you a quick overview of Docker basics. You can skip the Azure section. :-)

Firstly, install [Visual Studio Tools for Docker](https://docs.microsoft.com/en-us/dotnet/standard/containerized-lifecycle-architecture/design-develop-containerized-apps/visual-studio-tools-for-docker) which will allow you to access Docker tooling from within Visual Studio.

Now, rightclick on the 'VLanMigrate' project and select "Add > Docker Support" and from the options in the list select "Windows". 
![Adding Docker support from Visual Studios](/images/module-6/AddDockerSupport-2.jpg)

Visual Studio should have created the Dockerfile under the project that has the following lines:

``` shell
#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM mcr.microsoft.com/dotnet/core/runtime:2.1-nanoserver-1809 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/core/sdk:2.1-nanoserver-1809 AS build
WORKDIR /src
COPY ["VLanMigrate.csproj", ""]
COPY ["../GenericParsingCore/GenericParsingCore.csproj", "../GenericParsingCore/"]
RUN dotnet restore "./VLanMigrate.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "VLanMigrate.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "VLanMigrate.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "VLanMigrate.dll"]
```
_To understand the DockerFile structure and why those commands are required in the file, Refer to  <a href="#appendix-b">**Appendix B**</a> for more information._

The next step is to run the application in a container and first thing to do is to create the container image file for the project, using the docker file. From your Prompt, go to the project folder where the Dockerfile resides and run the following command:

```shell
docker build -t vlanimage -f Dockerfile .
```

Docker will process each line in the Dockerfile. The . in the docker build command tells Docker to use the current folder to find a Dockerfile. This command builds the image and creates a local repository named "vlanimage" that points to that image. After this command finishes, run docker images to see a list of images installed:

```shell
docker images
```

Tips:

_The above build command will throw error. The docker command is unable to find the project dependency. Think about how you this issue can be fixed. Refer to  <a href="#appendix-a">**Appendix A**</a> for the solution._

_You might see errors like 'duplicate assembly information', which is caused by the presence of an AssemblyInfo.cs file in your project having updated from .NET to .NET Core. You should delete this to prevent them from being included in the build._

## Next

Please proceed to the next lesson.

**[Proceed to Module 7](/module-7)**

<a id='appendix-a'></a>
## Appendix A : How to fix the docker build  issue?

When running the docker build command, it expects to find all project dependencies in the running context, which in our example is the project folder. To solve this issue, we can move the Dockerfile to one folder up and run the command from that folder. This will resolve the first issue but introduces new errors due to the relative path in the Dockerfile. So, you need to change 2 lines in the Docker file per following:

```shell
COPY ["vlanmigrate/VLanMigrate.csproj", ""]
COPY ["GenericParsingCore/GenericParsingCore.csproj", "../GenericParsingCore/"]
```
Now, running the docker build command should work and the image is created per instruction.

<a id='appendix-b'></a>
## Appendix B : DockerFile Structure

### [AWS Developer Center](https://developer.aws)
