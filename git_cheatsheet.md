# Rebase feature branch into develop branch

If you follow [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/), you will have to merge feature branches into develop from time to time. To make a beautiful linear tree in the develop branch, you would rebase instead of just merging. The following guide will outline the rebase process from beginning to end.

## Rebasing one branch into develop
Suppose your feature branch is `feature/add_css`

```
    # This will merge the latest develop to feature/css
    git checkout develop
    git pull
    git checkout feature/add_css
    git pull
    git merge develop
    git push

    # Create a temporary rebase branch
    git checkout develop
    git checkout -b temp

    # Merge feature branch and resolve a lot of conflicts
    git merge feature/add_css

    # Perform the rebase, you may have to resolve the conflict again
    git rebase develop

    # NOTE: Do not use git checkout (--theirs|–ours) path/to/file
    # as it will cause trouble when you resolved a bad conflict then
    # the subsequent merge may not be correct.

    # This will merge the rebased item to develop.
    git checkout develop
    git merge feature/something_rebase

    # (Important) Checks that there are no difference. This should resolve in no difference in the ideal case
    git diff develop..feature/something

    # (Important) Checks that the logs have been correctly integrated.
    git log --graph

    # Push once all checks have been completed.
    git push

    # Delete the temporary merge branch
    git branch -D temp
```
<br/>

## Rebasing multiple branches into develop
The correct way to do this is to rebase each branch one by one, but the next one will have to rebase based on the result of the last rebase. Suppose you're going to merge these 2 branches: `feature/add_css` and `feature/add_html`

```
    # This will merge the latest develop to feature/css
    git checkout develop
    git pull
    git checkout feature/add_css
    git pull
    git merge develop
    git push

    # Create a temporary rebase branch
    git checkout develop
    git checkout -b main_rebase

    # Merge feature branch and resolve a lot of conflicts
    git merge feature/add_css

    # Perform the rebase, you may have to resolve the conflict again
    git rebase main_rebase

    # Push this branch to remote so we can rebase other branch based on it
    git push

    # Now checkout another temporary branch
    git checkout -b temp

    # Merge feature branch and resolve a lot of conflicts
    git merge feature/add_css

    # (IMPORTANT) Perform the rebase based on the main rebase branch
    git rebase main_rebase

    # Merge the rebase items into main rebase branch
    git checkout main_rebase
    git merge temp

    # (IMPORTANT) Checks that the logs have been correctly integrated.
    git log --graph

    # (IMPORTANT) Push main rebase branch to remote
    git push

    # Delete the temporary merge branch
    git branch -D temp
```
<br/>

Then repeat this process with the next branches one by one.

