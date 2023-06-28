### Test SSH

```bash
ssh -T ssh.gitlab.devcraft.app

# or verbose
ssh -Tv gitlab.devcraft.app

# or more verbose
ssh -Tvvv gitlab.devcraft.app
```

### List current remotes in the local repo

```bash
git remote

# to show remotes with links
git remote -v

# show additional info
git remote show origin
```

### Create a new remote

```bash
git config --global init.defaultBranch main
git config --global user.email "anton.evseev@rubius.com"
git config --global user.name "Anton Evseev"

git init --initial-branch=main

git remote add origin git@github.com:slayoffer/gitlab-example.git
# or to add forked remote
git remote add upstream git@github.com:slayoffer/gitlab-example.git

# if different key file name, then set this key
git config core.sshCommand "ssh -i ~/.ssh/github_id_ed25519 -o IdentitiesOnly=yes -F /dev/null"
git remote add origin git@github.com:slayoffer/gitlab-example.git

# rename master branch to main
git branch -M main
git add . && git commit -m "initial commit"

# set permissions
sudo chmod 600 ~/.ssh/id_rsa

# set remote upstream
git push -u origin main

# BETTER set up ssh config
Host github
  HostName github.com
  User git
  IdentityFile ~/.ssh/github_id_ed25519
  IdentitiesOnly yes
  
# then
git remote add origin github:slayoffer/test-example.git
git branch -M main
git add .
git commit -m "test"
git push -u origin main
```

### Upload local code to GitHub

```bash
git push origin master

# "-u" sets origin to upstream remote (it will allow to "git pull" without additional arguments)
git push origin master -u
# or
git push --set-upstream origin master

# DONT DO IT! - force push (will replace all the remote commits with your local )
git push --force
```

### Pull from remote

```bash
# easy way
git pull
# or
git pull --verbose

# hard way
git fetch # 1
git merge origin/master # 2

# to fetch changes from forked repo
git fetch upstream

# to add changes from forked repo on top of your changes
git rebase upstream/main
```

### Clone a remote repository to your local machine and change the name of the directory

```bash
git clone <repo-url> <new-repo-directory-name>
# "git clone" creates remote-tracking branches in the cloned repo

# clone with SSH
git clone git@github.com:slayoffer/ansible.git --config core.sshCommand="ssh -i ~/.ssh/ansible_id_ed25519"
```

### Change from Https to SSH

```bash
# check - should show https link
git remote show
git remote get-url origin

# change existing repo auth method from https to ssh
git remote set-url origin tfs-rubius:/DevSaunaProjects/clean-city/_git/clean-city
git remote get-url origin

# push
git push
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