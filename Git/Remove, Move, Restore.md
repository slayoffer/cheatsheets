### Remove file already added to Git

```bash
git rm --cached .env
```

### Remove file from tracking

```bash
git rm dirname/somefile.js

# or several files
git rm dirname/*.html

# remove already tracked files (for gitignore)
git rm -r --cached ansible/host_vars # will untrack all files in the dir
```

### Move files

```bash
git mv sourcedir1/somefile.js destination_dir2 # dir2 must be present
```

### Restore file

```bash
git checkout somefile.js

# restore staged file
git reset HEAD somefile.js

# unstage all files
git reset HEAD
```

### Reset files in the staging area

```bash
git reset .
```