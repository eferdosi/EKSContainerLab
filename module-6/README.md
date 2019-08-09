# Module 6 - Dockerize the Sample Application


**Time to complete:** 20 minutes

**Services used:**


### Overview

In this module, we will add Docker support to our app and containerize it. 

If you are new to Docker or using Docker with .NET, it is recommended to read [this first](https://docs.microsoft.com/en-us/dotnet/core/docker/intro-net-docker) to give you a quick overview of Docker basics. You can skip the Azure section. :-)

Firstly, install [Visual Studio Tools for Docker](https://docs.microsoft.com/en-us/dotnet/standard/containerized-lifecycle-architecture/design-develop-containerized-apps/visual-studio-tools-for-docker) which will allow you to access Docker tooling from within Visual Studio.

Now, rightclick on the 'VLanMigrate' project and select "Add > Docker Support" and from the options in the list select "Linux". 
![Adding Docker support from Visual Studios](/images/module-6/AddDockerSupport-2.jpg)

Visual Studio should have created the Dockerfile under the project that has the following lines:

``` shell
FROM mcr.microsoft.com/dotnet/core/runtime:2.1-stretch-slim AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/core/sdk:2.1-stretch AS build
WORKDIR /src
COPY ["VLanMigrate.csproj", ""]
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

Before building the container image, we need to include the input file that is required for the application to run, in the build process. In Visual Studio, in Solutions Explorer, go to the 'TEST-DATA' folder and select the 'SampleVlans.txt' file. then in the project window change the 'Build Action' and 'Copy to output directory' attributes to 'Content' and 'Copy Always'. 

![Adding Input Text File](/images/module-6/AddContentFiles.JPG)

The next step is to run the application in a container and first thing to do is to create the container image file for the project, using the DockerFile. In Visual Studio, right click on the DockerFile and select 'Build Docker Image'. You can check the output window to see the progress of the Docker build command.

![Build Docker Image](/images/module-6/BuildDockerImage.jpg)

Docker will process each line in the Dockerfile. This command builds the image named "vlanimage" in the local repository. After this command finishes, run docker images in the command prompt window to see a list of images installed:

```shell
docker images
```

## Next

Please proceed to the next lesson.

**[Proceed to Module 7](/module-7)**

<a id='appendix-b'></a>
## Appendix B : DockerFile Structure
The Dockerfile file is used by the docker build command to create a container image. This file is a plaintext file named Dockerfile that does not have an extension. Let's see what each line in the DockerFile for the example project does:

The first two commands, "FROM", tells Docker to pull down the .Net Core images that contains the .NET Core runtimes, libraries and SDKs for building and running the application. The images are optimized for running .NET Core apps in production. It's important to make sure that the .Net Core version is matched with the version that has been used by project i.e. Core 2.1

```shell
FROM mcr.microsoft.com/dotnet/core/runtime:2.1-stretch-slim AS base
FROM mcr.microsoft.com/dotnet/core/sdk:2.1-stretch AS build
```

Then on the following lines, docker sets a folder as the base and copies the project into that folder.
```shell
WORKDIR /src
COPY ["VLanMigrate.csproj", ""]
RUN dotnet restore "./VLanMigrate.csproj"
COPY . .
WORKDIR "/src/."
```
Now, using dotnet CLI, it builds and publishes the project in release mode and copies the output into the "app" directory
```shell
RUN dotnet build "VLanMigrate.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "VLanMigrate.csproj" -c Release -o /app
```
And finally, copies the publish artifacts built in previous step, into the root folder of the app and set the main assembly file as the entrypoint for the image.
```shell
FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "VLanMigrate.dll"]
```


### [AWS Developer Center](https://developer.aws)
