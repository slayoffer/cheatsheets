In Azure DevOps, you can prevent a pipeline from triggering by including `[skip ci]` or `[ci skip]` in your commit message. This will cause the pipeline to ignore the commit and not start the build.

Here's an example of a commit message that will prevent the pipeline from triggering:

```
Update README.md [skip ci]
```

Or:

```
Update README.md [ci skip]
```

Keep in mind that `[skip ci]` and `[ci skip]` will prevent the pipeline from running for the specific commit they are included in. If you want to disable the pipeline trigger for a longer period or permanently, you will need to modify the pipeline's trigger settings in the `azure-pipelines.yml` file or through the Azure DevOps web interface.