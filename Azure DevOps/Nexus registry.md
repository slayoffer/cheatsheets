### Use

```yaml
steps:  
- task: DockerCompose@0  
displayName: 'Docker Compose Build'  
inputs:  
containerregistrytype: 'Container Registry'  
dockerRegistryEndpoint: nexus-cr  
dockerComposeFile: 'docker-compose.yml'  
dockerComposeFileArgs: BACKEND_BUILD_NUMBER=$(Build.BuildNumber)  
action: 'Build services'  
arguments: '--force-rm'

   
- task: DockerCompose@0  
displayName: 'Docker Compose Push'  
inputs:  
containerregistrytype: 'Container Registry'  
dockerRegistryEndpoint: nexus-cr  
dockerComposeFile: 'docker-compose.yml'  
dockerComposeFileArgs: BACKEND_BUILD_NUMBER=$(Build.BuildNumber)  
action: 'Push services'
```