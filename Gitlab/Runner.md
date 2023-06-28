### Register

```bash
# most probably deprecated
gitlab-runner register

# for new GitLab versions use
gitlab-runner deploy # will need auth token for that
```

### Run

```bash
gitlab-runner start

gitlab-runner run
```

### Check

```bash
gitlab-runner verify

gitlab-runner status

gitlab-runner -version
```

### Delete Runner from Gitlab via CLI

```bash
docker exec -it gitlab bash

gitlab-rails console

# delete all runners
Ci::Runner.delete_all

# check
Ci::Runner.all
```

### Pull policy

The GitLab Runner is designed to always pull the Docker image for each job by default. This ensures that the most recent version of the image is always used for jobs, which could be important if the image is being updated regularly.

However, if your Docker images are large or the network connection is slow, this could cause a significant slowdown in your job run times. If you're using a Docker image that doesn't change often, you may want to consider changing the pull policy for your Docker executor.

In the configuration file (`config.toml`) for your GitLab Runner, you can set the `pull_policy` for your Docker executor. By default, it's set to `"always"`, which means it will always attempt to pull a newer version of the image. If you change this to `"if-not-present"` or `"never"`, it will only pull the image if it's not already available locally or never pull the image respectively.

Here is a small example of how you might set this in your `config.toml`:

```toml
[[runners]]
  name = "My Runner"
  url = "https://gitlab.example.com/"
  token = "TOKEN"
  executor = "docker"
  [runners.docker]
    tls_verify = false
    image = "alpine:latest"
    privileged = false
    disable_cache = false
    volumes = ["/cache"]
    pull_policy = "if-not-present" # or "never"
    shm_size = 0
```

This will reduce the number of image pulls, but it also means you'll need to manually update the image if a new version is released that you want to use. So, you should consider the trade-offs before changing this setting.

Remember to restart the GitLab Runner service after modifying the `config.toml` file for the changes to take effect.