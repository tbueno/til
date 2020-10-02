# Remove local branches that were already merged

Since in most projects that I work on rely on pull-requests using Github, the merge of my branches are usually done via the Github interface when the PR is accepted. This also takes care of removing the remote branch used for the changes, but has the downside of keeping my local list of branches out of sync.

Luckily git has already a simple command to list all local branches that were already merged

````bash
 ➜ git branch --merged
````

The output of this command can be parsed and used in combination with `git branch -D`.

However, as [pointed out in this discussion](https://stackoverflow.com/a/26152574/4721514), `git branch` is a command whose output can be customized by the user's git configuration, making not a reliable command to be combined with others. A better solution is to make use of `for-each-ref` command, which is more appropriate for parsing.


By piping its output to the branch delete command it is possible to delete a list of local branches already merged in the remote master.

````bash
 ➜ ➜ git for-each-ref --format '%(refname:short)' refs/heads | grep -v master | xargs git branch -D
error: Cannot delete branch 'abc-foo' checked out at '/Users/bueno/dev/my-proj'
Deleted branch XYZ/bar (was 7748bbe).
Deleted branch another-branch (was 1112428).
Deleted branch PROJ/bug-fix (was 2d9c44d).

````
