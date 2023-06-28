### Reset settings via CLI

```bash
docker exec -it gitlab bash

# enter CLI
gitlab-rails console

# reset
ApplicationSetting.first.delete

# check
ApplicationSetting.first
```