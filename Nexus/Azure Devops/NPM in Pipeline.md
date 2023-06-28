To use your Nexus npm registry in an Azure DevOps CI pipeline, follow these steps:

1. Configure your Nexus npm registry:

   First, ensure that you have properly set up and configured your Nexus Repository Manager instance to host an npm registry. Refer to the official Nexus documentation for details: https://help.sonatype.com/repomanager3/node-packaged-modules-and-npm-registries

2. Obtain the Nexus npm registry credentials:

   Get the username, password, and URL of your Nexus npm registry. You'll need these credentials to authenticate with the registry during the CI pipeline.

3. Store credentials in Azure DevOps:

   Store your Nexus npm registry credentials as secret variables in your Azure DevOps pipeline. This will keep them secure and make them easily accessible during the pipeline execution.

   - Go to your Azure DevOps project.
   - Click on "Pipelines" in the left-hand menu, then select your pipeline.
   - Click "Edit" on the top right.
   - Click the "Variables" tab in the pipeline editor.
   - Click "+ Add" and enter the required variable names (e.g., `NEXUS_USERNAME`, `NEXUS_PASSWORD`, and `NEXUS_REGISTRY_URL`), set the values, and mark them as "Secret".

4. Install npm and configure the registry:

   Update your `azure-pipelines.yml` file to configure npm to use your Nexus registry. Add the following steps to your pipeline:

   ```yaml
   - task: NodeTool@0
     displayName: 'Install Node.js'
     inputs:
       versionSpec: '14.x'

   - script: |
       echo "Setting up Nexus npm registry..."
       echo "//$(echo $NEXUS_REGISTRY_URL | awk -F/ '{print $3}')/:_authToken=$(echo $NEXUS_USERNAME):$(echo $NEXUS_PASSWORD)" > .npmrc
       echo "registry=$(echo $NEXUS_REGISTRY_URL)" >> .npmrc
       echo "always-auth=true" >> .npmrc
     displayName: 'Configure Nexus npm registry'
     env:
       NEXUS_USERNAME: $(NEXUS_USERNAME)
       NEXUS_PASSWORD: $(NEXUS_PASSWORD)
       NEXUS_REGISTRY_URL: $(NEXUS_REGISTRY_URL)
   ```

   This will install Node.js, create an `.npmrc` file in the pipeline workspace, and configure the registry and authentication details using the secret variables you stored earlier.

5. Run your npm commands:

   Now that you've configured npm to use your Nexus registry, you can run npm commands in your pipeline. For example:

   ```yaml
   - script: |
       npm ci
     displayName: 'Install dependencies'

   - script: |
       npm run build
     displayName: 'Build project'
   ```

6. Publish your npm package (optional):

   If you want to publish your npm package to the Nexus registry as part of your CI pipeline, you can use the `npm publish` command:

   ```yaml
   - script: |
       npm publish
     displayName: 'Publish npm package'
   ```

That's it! Your Azure DevOps CI pipeline is now configured to use your Nexus npm registry. The pipeline will authenticate with Nexus, install dependencies from the registry, and optionally publish your package to the registry.