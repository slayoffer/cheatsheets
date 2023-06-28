### Check git users

```bash
git config --list 
```

### Change username and mail

```bash
git config --global user.name "Jeff Delaney"
git config --global user.email "hello@fireship.io"
```

### Enable credentials caching

```bash
git config --global credential.helper 'cache --timeout=86400'
# Set Git to use the credential memory cache
```

### To set specific SSH key for git repo

```bash
git config --add --local core.sshCommand 'ssh -i C:/Users/slayo/.ssh/gitlab_id_rsa'
git push
```

### Add remote with ssh key
```bash
git config core.sshCommand "ssh -i ~/.ssh/github_id_ed25519 -o IdentitiesOnly=yes -F /dev/null"
git remote add origin git@github.com:slayoffer/gitlab-example.git
```

### Set up ssh config and pull

```bash
vim ~/.ssh/config
# add
Host github
  HostName github.com
  User git
  IdentityFile ~/.ssh/github_id_ed25519
  IdentitiesOnly yes
# then
git remote add origin github:slayoffer/test-example.git
```

### How to reset stored user & password

```bash
# check helper settings
git config -l

# unset
git config --global --unset credential.helper
git config --system --unset credential.helper

# set
git config credential.helper store

# check
git pull
```

