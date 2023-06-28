### What if you accidentally delete your local branch, is there a way to recover it?

You can use the _`**git reflog**`_ command to do so. The steps to do so are below:

-   After the accidental deletion, run the command **`git reflog`** which lists all operations performed in your git repository e.g. deleting a branch, committing a change, reverting a change, etc.,
-   From the output of the _`git reflog`_ command, identify the commit hash or SHA1 associated with the latest commit on that branch,
-   Then, create a new branch using _**`git branch <branch_name> <commit_hash>`**_, which will now contain the branch code changes you accidentally lost.