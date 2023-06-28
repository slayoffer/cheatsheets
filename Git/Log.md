### Log to see the history of commits

```bash
git log

# one liner
git log --oneline

# list last 3 commits
git log -3
# or
git log -n 3

# with commits graphs and decoration
git log --graph --oneline --decorate

# for all branches
git log --all --graph --oneline --decorate

# for detailed changes
git log -p

# list changed files
git log --name-only
```

### Reflog (god mode)

```bash
# show history
git reflog
# then can rollback reset, etc
```

### Show commit

```bash
git show 1af17e73721dbe0c40011b82ed4bb1a7dbe3ce29

# or short
git show 1af17e
```