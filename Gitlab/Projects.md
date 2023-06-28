### Delete via CLI

```bash
docker exec -it gitlab bash

# enter CLI
gitlab-rails console

# select
project = Project.find_by_full_path('slayo/gitlab-example')

# delete
project.destroy
```