### 2FA disable via CLI

```bash
docker exec -it gitlab bash

gitlab-rails console

user = User.find_by(username: 'slayo')

user.update_attribute(:otp_required_for_login, false)
```