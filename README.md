## Is a Local Merge Different from Merge on Github?

Students reported that merging on Github first merges *into* the feature branch and then back into main/master.

To see this, I created this repository with two divergent branches `main` and `dev`.  Then merged twice: once locally, once on Github.

Initial files:
- Add a README (like this one) to `main`.

## `dev` branch

- Create a `dev` branch.

- Switch to `dev` and modify README. Commit the changes.

- Make more changes and commit again.

`dev` is now ahead of `main` by 2 commits.

## `main` branch

- Switch to `main` and add these lines to README. Commit the changes.

- Add more lines and commit again.

## Graph of branches with divergent changes

After committing those changes, the repo looks like:

![Graph of divergent branches](images/before-merge.png)

The graph shows that `main` and `dev` have diverged. 

## Push Everything to Github So We Can Repeat Merge Later

Before merging, I pushed this repo to Github so I can repeat
the merge on Github and see how it compares.

## Local Merge

After pushing the pre-merged files on both branches to Github, I ran:

```
git checkout main
git merge dev

Conflict!

git mergetool     # I use a visual mergetool to fix conflicts
```
After resolving the conflicts I committed README.md to `main`
```
git add README.md
git commit -m "merged locally"
```
Now the repo looks like this. Notice that the merge commit is on **main**. (This is what we expect.)

![Repository after local merge](images/after-local-merge.png)

## Merge with PR on Github

On the pre-merged repository on Github, I created a Pull Request to merge `dev` into `main`, as suggested by Github.  

Github reports conflicts and offers to open a "conflict resolution editor" like this:

![Conflict Resolution Editor](images/github-conflict-resolver.png)

Notice the text at top: **Github is merging main into dev**

After resolving the conflict and commiting, the repo on Github looks like this:

![After Conflict Resolution](images/after-github-conflict-resolver.png)

Notice the new commit on `dev` where the merge occurred.

## PR Merges `dev` into `main`

*After* resolving and commiting the conflict, Github presents the 
usual PR web page for a merge of `dev` -> `main`.

![PR after conflict resolution](images/github-pr-after-conflict-resolution.png)

On a team project you would wait for the team to comment and approve.

I clicked "Merge pull request", and it merged `dev` into `main`. So finally the repository on Github looks like:

![after PR and merge on Github](images/after-github-merge-into-main.png)

## Why Does Github Do This?

Github wants to protect the `main`/`master` branch from buggy, untested code.  So, it resolves conflicts on *another* branch first.

You can then test and review that branch.  Or use CI to run tests.

This avoids introducing untested code and possible errors in main.

## Summary

If you open a PR and Github find conflicts, the Github conflict resolver merges the changes into a branch rather than main. This projects the main or master branch from untested code that may contain errors.

After conflicts are resolved, you should re-test your code, and then proceed with the PR as usual.

We can do this locally, too. Instead of merging `dev` -> `main`, we can checkout `dev` and merge `main` -> `dev`. Then test and review it to verify the merge did not produce any errors.

Finally, if everything looks good then merge `dev` into `main`, which will be a simple "fast forward" merge.

