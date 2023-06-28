### Install

```bash
sudo yum install git
# or
sudo apt install git-all

# check version
git version
```

### Fast commit

```bash
git add . && git commit -am "updates" && git push 
```

### Init repo

```bash
git init
# with creating dir
git init myrepo
```

### Check repo status

```bash
git status
```

### Git commands Alias

```bash
# make an alias for "commit -am"
git config --global alias.ac "commit -am"

git ac "Done!"
```

### Check commits one by one and find good/bad ones - Bisect

```bash
git bisect start

git bisect bad

git bisect good
```

### Git clean to remove useless files

```bash
git clean -df
```

### Remove git folder

```bash
rm -rf .git
```

