### Stash git (save project temporarily for later without commiting)

```bash
# to save
git stash

# to load the most recent stashed item
git stash pop
# or with specific name
git stash pop <stash name>

# to save with a specific name
git stash save coolstuff

# list out all stashes
git stash list

# show stash
git stash show <stash name>

# load a stash based on its index
git stash apply 1
```

### Apply stash

```bash
# make stash on current branch
git stash

# switch to other branch and apply stash
git stash apply
```

