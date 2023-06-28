You can override a variable value that is defined in a template by passing a parameter value to the template.

Here is an example:

Suppose you have a template that defines a variable `myVar` like this:

```yaml
# my-template.yaml
parameters:
  myVar:
    type: string
    default: "defaultValue"

trigger:
  - main

variables:
  someOtherVar: "otherValue"

steps:
  - script: echo "myVar = ${{ parameters.myVar }}"
```

To override `myVar` defined in `my-template.yaml`, you can pass a parameter value to the template from the pipeline that refers to it. Here is an example:

```yaml
# azure-pipelines.yaml
trigger:
  - main

pool:
  vmImage: 'ubuntu-latest'

variables:
  myVar: "overriddenValue"

steps:
  - template: my-template.yaml
    parameters:
      myVar: $(myVar)
```

In the above example, the `myVar` variable is defined in the pipeline YAML file and is set to `"overriddenValue"`. Then, when the `my-template.yaml` template is used in the pipeline, the `myVar` parameter is passed to the template with the value of `$(myVar)`, which refers to the value of the `myVar` variable in the pipeline YAML file.

When the pipeline runs, the `myVar` parameter in the `my-template.yaml` template is set to `"overriddenValue"` instead of the default value of `"defaultValue"`.

This method allows you to override the default value of a variable defined in a template with a value defined in the pipeline YAML file.