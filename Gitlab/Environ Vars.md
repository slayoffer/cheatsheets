### Node.js

If you need to add variables from GitLab to your Node.js `.env` file, there are a couple of approaches you could take.

1. **Use GitLab CI/CD environment variables:** GitLab has built-in support for defining environment variables that can be used in your CI/CD pipelines. You can define these variables in the GitLab UI, and they'll be available to your application when it runs in the pipeline. This is often used for sensitive data like API keys that you don't want to store in your repository.

   To use these environment variables in your Node.js application, you can access them using `process.env.<VARIABLE_NAME>`. You don't need to use `.env` file in this case. The environment variables set in GitLab CI/CD will be directly accessible in your Node.js application.

2. **Use a script to generate the .env file:** If you still want to use a `.env` file, you can write a script that gets run as part of your pipeline. This script could use `envsubst` or similar to substitute the GitLab variables into your `.env` file before your application starts. 

   In this case, you'd want to make sure your `.env` file is in your `.gitignore` to prevent it from being committed to your repository, because it will contain environment-specific values. 

Remember that any method that writes secrets to a file has potential security implications, so always ensure that any files containing sensitive data are adequately protected and cleaned up after use.

Sure, here's an example of a bash script that could be used to generate a `.env` file from environment variables:

```bash
#!/bin/bash

# create .env from environment variables
echo "
DB_HOST=$DB_HOST
DB_USER=$DB_USER
DB_PASS=$DB_PASS
" > .env
```

In this script, `$DB_HOST`, `$DB_USER`, and `$DB_PASS` are placeholders for the actual environment variables you have in your environment. Replace these with the actual names of your environment variables.

To use this script, you would run it in your GitLab CI/CD pipeline before starting your application. This would create a `.env` file with the current values of the environment variables, which your Node.js application can then read.

However, please be aware that this script would overwrite the `.env` file each time it's run. If you have other values in the `.env` file that you want to preserve, you would need to modify the script to append to the file instead of overwriting it, or handle those values separately.

As always, be careful when writing sensitive data to files, and make sure the `.env` file is included in your `.gitignore` file to prevent it from being committed to your repository.