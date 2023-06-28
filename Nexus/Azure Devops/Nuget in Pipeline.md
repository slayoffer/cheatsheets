To use your Nexus NuGet registry to pull packages in Azure DevOps, follow these steps:

1. Configure your Nexus NuGet registry:

   Ensure that you have properly set up and configured your Nexus Repository Manager instance to host a NuGet registry. Refer to the official Nexus documentation for details: https://help.sonatype.com/repomanager3/formats/nuget-repositories

2. Obtain the Nexus NuGet registry credentials:

   Get the API key and URL of your Nexus NuGet registry. You'll need these credentials to authenticate with the registry during the pipeline execution.

3. Store credentials in Azure DevOps:

   Store your Nexus NuGet registry credentials as secret variables in your Azure DevOps pipeline. This will keep them secure and make them easily accessible during the pipeline execution.

   - Go to your Azure DevOps project.
   - Click on "Pipelines" in the left-hand menu, then select your pipeline.
   - Click "Edit" on the top right.
   - Click the "Variables" tab in the pipeline editor.
   - Click "+ Add" and enter the required variable names (e.g., `NEXUS_API_KEY`, and `NEXUS_REGISTRY_URL`), set the values, and mark them as "Secret".

4. Add a NuGet.config file to your project:

   Create a `NuGet.config` file in your project's root directory with the following content:

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <configuration>
     <packageSources>
       <add key="Nexus" value="$(NEXUS_REGISTRY_URL)" />
     </packageSources>
     <apikeys>
       <add key="$(NEXUS_REGISTRY_URL)" value="$(NEXUS_API_KEY)" />
     </apikeys>
   </configuration>
   ```

   This configuration file tells NuGet to use your Nexus registry as a package source and provides the API key for authentication.

5. Update your `azure-pipelines.yml` file:

   Modify your `azure-pipelines.yml` file to restore NuGet packages using your Nexus NuGet registry. Add the following steps to your pipeline:

   ```yaml
   - task: NuGetToolInstaller@1
     displayName: 'Install NuGet'

   - task: NuGetCommand@2
     displayName: 'Restore NuGet packages'
     inputs:
       command: 'restore'
       restoreSolution: '**/*.sln'
       feedsToUse: 'config'
       nugetConfigPath: 'NuGet.config'
   ```

   This will install the NuGet tool, restore packages from your Nexus NuGet registry, and use the provided `NuGet.config` file for authentication.

Now, your Azure DevOps pipeline is configured to pull packages from your Nexus NuGet registry. The pipeline will authenticate with Nexus and restore packages from the registry as needed.