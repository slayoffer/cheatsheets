### Test

```bash
docker exec -it gitlab bash

gitlab-rails console

Notify.test_email('slayoffer@gmail.com', 'Hello World', 'This is a test message').deliver_now
```