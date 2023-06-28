### Rollback to a previous commit, BUT keep your changes in the working directory

```bash
git reset --soft <commit-ID>
```

### Rollback to a previous commit AND discard all changes (be careful with this one!)

```bash
git reset <commit-ID> --hard

# hard reset - substitute local code with remote code
git reset --hard origin/master
```

### Revert commit with a new commit (when already pushed)

```bash
git revert <commit-ID> -m "reverting some commit"

# revert last commit
git revert HEAD

# revert particular commit
git revert 1af17e
```


### Rebase

### Short Version

- Merge takes all the changes in one branch and merges them into another branch in one commit.
- Rebase says I want the point at which I branched to move to a new starting point

So when do you use either one?

### Merge

- Let's say you have created a branch for the purpose of developing a single feature. When you want to bring those changes back to master, you probably want **merge** (you don't care about maintaining all of the interim commits).

### Rebase

- A second scenario would be if you started doing some development and then another developer made an unrelated change. You probably want to pull and then **rebase** to base your changes from the current version from the remote repository.

```bash
git checkout feature

# rebase local code with code from remote
git rebase master

# to rebase with autosquash
git rebase -i --autosquash

# rebase last three commits
git rebase -i HEAD~3
```

### A safe way to rebase

- Create a sub-branch to your feature branch and make rebase on it. If all works fine - rebase at your original feature branch

### To combine all commits into a single message (squash)

```bash
git checkout -b test-rebase

git rebase master --interactive

# it will then open commit editor where you need to change all the needed "pick" strings of commits to "squash"
```