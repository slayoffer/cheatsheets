To use your Nexus NuGet registry inside a Dockerfile to build a .NET Core application, follow these steps:

1. Create a NuGet.config file in your project:

   In your project's root directory, create a `NuGet.config` file with the following content:

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <configuration>
     <packageSources>
       <add key="Nexus" value="https://your-nexus-instance.com/repository/nuget-group/" />
     </packageSources>
   </configuration>
   ```

   Replace `https://your-nexus-instance.com/repository/nuget-group/` with the URL of your Nexus NuGet registry. This configuration file tells NuGet to use your Nexus registry as a package source.

2. Update your Dockerfile:

   Modify your Dockerfile to include the following steps:

   - Set environment variable with Nexus API key.
   - Copy the `NuGet.config` file into the Docker image.
   - Restore NuGet packages using the configured Nexus registry.
   - Build and run your .NET Core application.

   Here's an example of how to do this in your Dockerfile:

   ```Dockerfile
   # Use the official .NET SDK image as your base image
   FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build-env

   # Set your working directory
   WORKDIR /app

   # Copy your csproj file(s)
   COPY *.csproj ./

   # Set the Nexus API key as an environment variable
   ARG NEXUS_API_KEY
   ENV NUGET_CREDENTIALS="{\"https://your-nexus-instance.com/repository/nuget-group/\":{\"Username\":\"\",\"Password\":\"${NEXUS_API_KEY}\"}}"

   # Copy the NuGet.config file
   COPY NuGet.config ./

   # Restore NuGet packages using the configured Nexus registry
   RUN dotnet restore

   # Copy the rest of your application code
   COPY . ./

   # Build your application
   RUN dotnet publish -c Release -o out

   # Use the official .NET Runtime image as your runtime image
   FROM mcr.microsoft.com/dotnet/aspnet:5.0

   # Set your working directory
   WORKDIR /app

   # Copy the build output from the build-env image
   COPY --from=build-env /app/out .

   # Expose the application port and start the application
   EXPOSE 80
   ENTRYPOINT ["dotnet", "your-app.dll"]
   ```

   Replace `https://your-nexus-instance.com/repository/nuget-group/` with the URL of your Nexus NuGet registry, and `your-app.dll` with the DLL name of your .NET Core application.

3. Build your Docker image:

   When building your Docker image, pass the Nexus API key as a build argument:

   ```
   docker build --build-arg NEXUS_API_KEY=my-api-key -t my-image .
   ```

   Replace `my-api-key` with your Nexus API key, and `my-image` with your desired Docker image name.

With these changes, your Dockerfile will use the Nexus NuGet registry to restore packages during the image build process. The API key will be stored as an environment variable inside the Docker image. If you want to remove it from the final image, you can use multi-stage builds to copy the build output to a new image without the environment variable.

To build a Docker image that uses your Nexus NuGet registry in Azure DevOps, follow these steps:

1. Configure your Nexus NuGet registry and obtain the API key, as mentioned in the previous answers.

2. Store the API key in Azure DevOps as a secret variable:

   - Go to your Azure DevOps project.
   - Click on "Pipelines" in the left-hand menu, then select your pipeline.
   - Click "Edit" on the top right.
   - Click the "Variables" tab in the pipeline editor.
   - Click "+ Add" and enter the variable name (e.g., `NEXUS_API_KEY`), set the value, and mark it as "Secret".

3. Create or update your `azure-pipelines.yml` file:

   Add a new job to your pipeline to build and push the Docker image. Make sure you have the Docker extension installed in your Azure DevOps instance. Update your `azure-pipelines.yml` file with the following content:

   ```yaml
   trigger:
     branches:
       include:
       - main

   pool:
     vmImage: 'ubuntu-latest'

   variables:
     imageName: 'your-image-name'

   steps:
   - task: Docker@2
     displayName: 'Build Docker image'
     inputs:
       containerRegistry: 'your-container-registry-service-connection'
       repository: '$(imageName)'
       command: 'build'
       Dockerfile: '**/Dockerfile'
       buildContext: '.'
       buildArgs: 'NEXUS_API_KEY=$(NEXUS_API_KEY)'

   - task: Docker@2
     displayName: 'Push Docker image'
     inputs:
       containerRegistry: 'your-container-registry-service-connection'
       repository: '$(imageName)'
       command: 'push'
       tags: |
         $(Build.BuildId)
         latest
   ```

   Replace `your-image-name` with the desired name for your Docker image, and `your-container-registry-service-connection` with the name of your container registry service connection (e.g., Azure Container Registry, Docker Hub, etc.).

4. Create a Container Registry Service Connection:

   If you haven't already, create a service connection in Azure DevOps for your container registry. This allows Azure DevOps to authenticate with your container registry and push the built Docker images.

   - Go to your Azure DevOps project.
   - Click on "Project Settings" in the bottom left corner.
   - Under "Pipelines," select "Service connections."
   - Click "New service connection" and choose the type of your container registry (e.g., Docker Registry, Azure Container Registry).
   - Fill in the required information and click "Save."

With this setup, Azure DevOps will build the Docker image using your Nexus NuGet registry and push the image to your specified container registry when the pipeline is triggered. The Nexus API key will be passed as a build argument during the Docker build process.