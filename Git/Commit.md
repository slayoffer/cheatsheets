### Add files to the staging area

```bash
git add .
# "." - all the files in the working directory
```

### Git commit

```bash
git commit -m "initial commit 🚀"

# or with git add
git commit -a -m "additional commit"
# or
git commit -am "additional commit"
# if new files present
git add . && git commit -am "message"

# Preview or try a commit before making it official
git commit -am "additional commit" --dry-run
```

### Commit correction

```bash
# to correct just the commit message
git commit --amend -m "better message"

# amend - add new files to commit and dont correct the commit message
git add <your-file>
git commit --amend --no-edit

# ALERT !!! Don’t amend public commits.
```

### Compare the changes before commit

```bash
git diff

# changes in staged
git diff --staged

# one file changes
git diff somefile.js
```