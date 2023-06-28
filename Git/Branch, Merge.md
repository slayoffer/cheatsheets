### Merge branch

```bash
git merge existing_branch_name

# for merge commit
git merge --no-ff existing_branch_name
```

### List out branches

```bash
git branch

# list remote branches
git branch -a
```

### Create a new branch

```bash
git branch awesome

# to rename master branch to main
git checkout master
git branch -M main

# create a new branch and move into it
git checkout -b awesome
```

### Rename local branch

```bash
git checkout old-branch
git branch -m <new_name>

#  rename a branch while pointed to any branch
git branch -m <oldname> <newname>
git branch -a 
```

### Rename remote branch

```bash
git branch -a
git push origin --delete old-branch-name
git push origin -u new-branch-name

# or
git push origin :old-name new-name
git push origin -u new-name
```

### Delete branch

```bash
# safe delete
git branch -d awesome

# forced delete (!)
git branch -D awesome

# remove remote branch
git push origin -d remotebranchname
```

### Move into a branch

```bash
git checkout awesome

# move into prev branch in case forgot the name of it
git checkout -
```

### Push branch to remote repo

```bash
git push -u origin branchname
```

### Squash

```bash
git merge --squash feature-1 # squash commits from feature-1 into one commit and merge with current branch
```

### Rebase

```bash
git rebase main # rebase from current branch into main

# interactive mode
git rebase -i commit_id
```

### Merge conflicts

```bash
git branch feature
# make some changes then
git commit -am "awesome branch stuff"

git checkout master
# make some changes to same code then
git commit -am "master branch stuff"

git merge feature
# CONFLICT!
```

The easiest way to fix the merge conflict is use the editor to choose between the incoming changes (feature) or the existing changes (master). Then create a new *merge* commit with the changes you want to keep.

```bash
# choose preferred code on master branch
git commit -am "resolved merge conflict"

# abort a conflicting merge
git merge --abort
```