### Delete a local branch with merged code changes

Below is the syntax for the command that deletes a local branch with merged code changes.

```
git branch -d feature/a_new_feature feature/another_new_feature
```

The command above will delete the feature branches _feature/a_new_feature_ and _feature/another_new_feature_. You can specify 1 or more branch names you want to delete.

The **-d** option is an alias for **--delete**, which will delete only specified branches whose code changes have all been merged into the branch you are on i.e. _develop_ branch in this case.

### Delete a local branch with unmerged code changes

Below is the syntax for the command that deletes a local branch with unmerged code changes.

```
git branch -D feature/a_new_feature feature/another_new_feature
```

The command above will delete the feature branches _feature/a_new_feature_ and _feature/another_new_feature_. You can also specify 1 or more branch names that you want to delete.

The **-D** option is an alias for **--delete --force** i.e. **-d -f**, which will delete all specified branches regardless of whether the code changes have all been merged or not.

Please note that the git command `_**git branch feature/a_new_feature -d**_ **-f**`, similar to the one highlighted above, also deletes any specified branch whether or not it has merged or unmerged code changes.

## How to delete remote git branches

Now you have successfully deleted the old feature branches on your local git repository. But then, you think the remote might also be cluttered with unnecessary branches which could be eating up storage space.

So, to satisfy your curiosity, you ran the command below:

```
git ls-remote --heads <remote_repo_alias>  
```

The command above shows the branches that exist on the remote. The **--head** option ensures only branch references are listed. _remote_repo_alias_ is an alias for the remote repository which can be called _origin_ for example.

You see a couple of old feature branches in the output of the command above created by you that still exists on the remote. There are two methods you can use to delete such branches.

### Method 1:

The first method’s command has the following syntax:

**`git push <remote_repo_alias> -d <remote_repo_branch_name>`**

The example below demonstrates how to use the command:

```
git push origin -d feature/a_new_feature
```

The command above will delete the remote branch _feature/a_new_feature_.

The **-d** option is an alias for **--delete**.

### Method 2:

```
git push origin :feature/a_new_feature
```

In the example above, the _origin_ is the alias given to the remote repo and _feature/a-new-feature_ is the name of the feature branch you are deleting.

Another alternative to deleting a remote branch is via the remote repository’s (e.g. Github) web console _by clicking the delete icon_ from the list of branches on the remote repository.

## How to delete remote-tracking branches

A remote-tracking branch is a local reference to remote branches on the remote repository. They exist on your local git repository to track the state of associated remote branches. This kind of branch is automatically created in several ways. Some examples are:

-   When you clone a remote repository via _`git clone`_,
-   When you download changes from a remote repository via _`git pull`_ or _`git fetch`_,
-   When you push your local branch code changes to a remote repository via _`git push`_,
-   When you switch to a remote branch via _`git switch`_ or _`git checkout`_, and so on.

The **`git branch -a`** command gives you a list of your local branches and remote-tracking branches. **-a** is an alias for **--all**.

You can also run the **`git branch -r`** command to see a list of only remote-tracking branches that exist on your local repository. **-r** is an alias for **--remotes**.

To delete a remote-tracking branch, you can use the same command used to delete a remote git branch. It also removes the associated remote-tracking branch which exists on your local repository.

Alternative ways to delete remote-tracking branches are seen below:

```
git remote prune <remote_repo_alias>
```

This command deletes remote-tracking branches on your local git repository for the specified remote i.e. _remote_repo_alias_ which can be named _origin_ for example.

```
git fetch <remote_repo_alias> --prune
```

This command fetches the latest updates (i.e new branches or commits ) from the remote specified by _remote_repo_alias_ to update the associated remote-tracking branches and then deletes any stale remote-tracking branches on your local git repository which no longer exist on the specified remote. **-p** is an alias for **--prune**.

```
git branch -d -r <remote_repo_alias>/<remote_repo_branch_name>
```

### Error deleting the same branch you switched to

```
error: Cannot delete branch 'feature_branch_name' checked out at 'C:/git/delete'
```

The error above occurs because, on your local git repository, you are trying to delete the same local branch you are currently on. Git will prevent you from performing this operation as it will result in a _detached HEAD state_. When you are in a _detached HEAD state_, you are not on any branch. This means any code changes you make will not be tracked and will be lost.

To resolve the error above,

-   First, switch to another branch via _`git switch`_ or _`git checkout`_
-   Then, you can go ahead to delete your local branches as discussed in the previous section.

### Error deleting a branch with the same tag name

```
error: dst refspec feature_branch_name matches more than one.
error: failed to push some refs to 'https://remote_repository_url'
```

The error above happens because the _feature_branch_name_ reference is the same name as another reference in the remote repository i.e. there are multiple references (e.g. branches, tags) in the remote repository with the same name you specified.

To resolve the error above, run the command below:

```
git push origin --delete refs/heads/<feature_branch_name>
```

This will delete the specified _feature_branch_name_ from the remote repository specified as _origin_ in the above example. Git uses **refs/heads/** as a prefix to reference branches.

Alternatively, you can run the command below which does the same thing:

```
git push origin :refs/heads/<feature_branch_name>
```

In addition to the errors discussed above, you may have seen other errors depending on your use case. Feel free to comment below if you encounter any errors we have not covered in this article.

### What if you accidentally delete your local branch, is there a way to recover it?

You can use the _`**git reflog**`_ command to do so. The steps to do so are below:

-   After the accidental deletion, run the command **`git reflog`** which lists all operations performed in your git repository e.g. deleting a branch, committing a change, reverting a change, etc.,
-   From the output of the _`git reflog`_ command, identify the commit hash or SHA1 associated with the latest commit on that branch,
-   Then, create a new branch using _**`git branch <branch_name> <commit_hash>`**_, which will now contain the branch code changes you accidentally lost.