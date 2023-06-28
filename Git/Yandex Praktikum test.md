### Task 1

```bash
git merge <тематическая ветка>.

git merge feature
# CONFLICT!

# The easiest way to fix the merge conflict is use the editor to choose between the incoming changes (feature) or the existing changes (master). Then create a new *merge* commit with the changes you want to keep.

# choose preferred code on master branch
git commit -am "resolved merge conflict"

# If you’re not sure, you can abort
git merge --abort
```

## Solution

```bash
git merge origin/american

# remove remote branch
git push origin -d american
```



### Task 2

I would suggest using instead something like this:

```
git reset --soft <commit_hash>
```

Unless you want it to remove all the changes up to that point, in that case use --hard instead of --soft, it would get you to the desired point in the tree WITHOUT trowing away all of the changes made in the commits.

While I was reading I tried with --HARD and for my misfortune, it removed all my changes up to that point. So, pay attention to whether you want them removed or not.

so, if you are unlucky as me try this! : `git revert <commit_hash>`



ust reset it to the commit hash that was prior to the 2 commits:

```
git reset <commit-hash>
```

This will delete the commits after `commit-hash` and bring the changes to your workspace. If you don't want the changes you can pass a `--hard` option.



Taking into account your example, you'd have to do this (assuming you're on branch 'master'):

```
git revert master~3..master
```

or `git revert B...D` or `git revert D C B`

This will create a new commit in your local with the inverse commit of B, C and D (meaning that it will undo changes introduced by these commits):

```
A <- B <- C <- D <- BCD' <- HEAD
```

## Solution:

```bash
git revert 4880a81613 607373271d
```

### Task 3 solution

Let's say you want to merge commit `756ead8e40` from branch new_button to main.

```bash
git checkout main
git cherry-pick 756ead8e40
git push

# safe local branch delete
git branch -d new_button

# remote branch delete
git push origin -d new_button

# forced delete (!)
git branch -D new_button
```