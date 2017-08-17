# Git Manual
Git manual and related topics such as branching process.

## <a id="toc">TOC
* [Installation](#user-content-installation)
* [Basics](#user-content-basics)
* [Info](#user-content-info)
* [Exports](#user-content-exports)
* [Branching](#user-content-branching)
* [Undoing](#user-content-undoing)
* [Remotes](#user-content-remotes)
* [Submodules](#user-content-submodules)
* [Additional resources](#user-content-additional-resources)

### <a id="installation"></a>Installation
* [Pro Git](http://git-scm.com/book)
* [Set tortoisegit with SSH private key](http://serverfault.com/questions/194567/how-to-i-tell-git-for-windows-where-to-find-my-private-rsa-key)

### Branching model
* [Successful git branching model](http://nvie.com/posts/a-successful-git-branching-model/)

```
    origin/feature/update_layout
    origin/hotfix/r20130305-1.1
    origin/master
    origin/develop
```
<br/>

### <a id="basics"></a>Basics
* [10 commands](http://niklasschlimm.blogspot.com/2011/07/top-10-git-commands-for-newbie.html|)
* [Git SVN crash course](http://git.or.cz/course/svn.html)

```
    # pull rebase create nicer merge tree, but be careful with it
    git stash # stash any change
    git pull --rebase
    git stash pop # stash pop any change

    git fetch -p # Bring the repository up to date without executing merge on the current branch

    git add -u # add modified files but not new files
    git add -A # add all untracked files

    # Diff with remote
    git diff master origin/master
```
<br/>

#### Tagging

```
    git tag -a <tagname> -m "Tag message"
    
    # delete tag on remote
    git tag --delete <tagname>
    git push origin :<tagname>
```
<br/>


### <a id="info"></a>Info

#### Find commits where files were deleted

```
    git log --diff-filter=D --summary --stat
```
<br/>

#### Checkout deleted file in the working tree

```
    git checkout <sha1>^ <file>
```
<br/>

#### Search for a file in project commit history

```
    git log --all --full-history -- **/<filename>.*
```
<br/>


#### Only show the content of a file from a specific revision

```
    git show <sha1>:<file>
```
<br/>

#### Diff 2 files at specific revisions or branches

```
    git diff <revision_1>..<revision_2> -- <file>
    git diff <branch_1> <branch_2> -- <file>
    git diff <branch_2> -- <file>
```
<br/>

#### Show changes on a branch that is not merged upstream

```
    git cherry <upstream_branch> <new_branch>
    git log <upstream_branch>..<new_branch>
```
<br/>

#### Show log with changed files

```
    git log --name-only
    git log --name-status
    git log --stat
    git log --decorate --graph --oneline --date-order # better visual
```
<br/>

#### Get latest tag in the current branch

```
    git describe --exact-match --abbrev=0
    git describe --abbrev=0 --tags
```
<br/>

#### Get tag with messages

```
    git tag -l -n9
```
<br/>

#### Show log graph

```
    git log origin master # Show log of a specific branch on remote
    git log origin master --graph --decorate # to show colorful text and graph and the branch name of each log.

    # A beautiful version
    git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit

    # Show git log then exit
    git log | cat -

    # Show log of a particular hash
    git show <hash>
    git show <hash> --stat # show only oneline for each change
```
<br/>

#### Get current URL

```
    git remote show origin
```
<br/>

#### Get brief info about branches

```
    git branch -lvv
```
<br/>

#### Finding what branch a commit came from

```
    git branch --contains <commit>
```
<br/>

#### Finding who committed which line

```
    git blame <file>
```
<br/>

#### Applying a range of commits

```
    # A should be older than B, this DOES NOT include A
    git cherry-pick A..B

    # Include A
    git cherry-pick A^..B
```
<br/>

### <a id="exports"></a>Exports

```
    # From a repository
    git checkout-index -a -f --prefix=/destination/path/

    # Export remote
    git archive --format=tar --remote=ssh://remote_server/remote_repository master | tar -xf -
```
<br/>


[TOC](#user-content-toc)

### <a id="branching"></a>Branching

#### Branch and Create new branch
```
    git checkout -b experimental
```
<br/>

#### Delete unused branch
```
    git branch -d experimental
    git push origin --delete newfeature
```
<br/>

#### Rename a local branch
```
    git branch -m <oldname> <newname>
```
<br/>

[TOC](#user-content-toc)

### <a id="undoing"></a>Undoing

#### Undo a merge or pull
Check out [git reset](http://www.kernel.org/pub/software/scm/git/docs/git-reset.html) for great explanation and [examples](http://www.kernel.org/pub/software/scm/git/docs/git-reset.html#_examples).

```
    git reset --hard
```
<br/>

#### Undo a merge or pull inside a dirty work tree
Check out [git reset](http://www.kernel.org/pub/software/scm/git/docs/git-reset.html) for great explanation and [examples](http://www.kernel.org/pub/software/scm/git/docs/git-reset.html#_examples).

```
    git reset --merge ORIG_HEAD
```
<br/>

#### Revert a bad commit
```
    git revert <sha1>

    # Revert single file
    git checkout -- filename

    # Revert all files in current folder
    git checkout .

    # Remove all new files or folder
    git clean -df

    # Revert to a commit with a new commit
    git revert --no-commit xxxxxx..HEAD
    git commit -m "revert to xxxxxx"
    git diff HEAD xxxxxx
```
<br/>

#### Checkout a deleted file into the work tree
```
    git checkout <sha1>^ -- <file>
```
<br/>

#### Rewrite author/commiter name and email
```
    git filter-branch --commit-filter '
            if [ "$GIT_COMMITTER_NAME" = "Ha.Minh" ];
            then
                    GIT_COMMITTER_NAME="Ha.Minh";
                    GIT_AUTHOR_NAME="Ha.Minh";
                    GIT_COMMITTER_EMAIL="minhhh@minhhuyha.info";
                    GIT_AUTHOR_EMAIL="minhhh@minhhuyha.info";
                    git commit-tree "$@";
            else
                    git commit-tree "$@";
            fi' HEAD
```
<br/>

[TOC](#user-content-toc)


### <a id="remotes"></a>Remotes

#### Create local branch then push to the remote (without tracking !!!)

```
    git checkout -b <branch_name>
    git push origin <branch_name>
```
<br/>

#### Crete a new local branch by pulling a remote branch

```
    git pull origin <branch_name>                                 # without tracking
    git checkout --track -b <branch_name> origin/<branch_name>    # with tracking
```
<br/>

#### Track a remote branch with an existing local

```
    git branch --set-upstream <branch_name> origin/<branch_name>
```
<br/>

#### Delete remote branch

```
    git push origin :heads/<branch_name>
```
<br/>

or

```
    git push origin :<branch_name>
```
<br/>

#### Prune remote-tracking branches that are deleted from a remote repo

```
    git remote prune origin
```
<br/>

#### Change remote URL

```
    git remote set-url origin http://new-example.com/repo.git
```
<br/>

#### Merge upstream from fork repo

```
    git checkout master
    git pull https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git BRANCH_NAME
```
<br/>

#### Push to upstream using specified ssh-key

```
    GIT_SSH_COMMAND='ssh -i ~/.ssh/yourprivatekey' git push --set-upstream origin develop
```
<br/>

[TOC](#user-content-toc)


### <a id="submodules"></a>Submodules

```
    # Add submodule to subdirectory
    git submodule add <git@github ...> snipmate-snippets/snippets/

    # update submodule
    git submodule update --recursive

    # Update submodules

    git submodule foreach 'git checkout master && git pull origin master'

    # Update submodule's URL
    # Edit the *.gitmodules* file, then run:
    git submodule sync

    # Delete submodule
    git submodule deinit asubmodule
    git rm asubmodule

    # Note: asubmodule (no trailing slash)
    # or, if you want to leave it in your working tree
    git rm --cached asubmodule

    #Get submodule hash
    git ls-tree a9a796a [submodule_dir]
```
<br/>

[TOC](#user-content-toc)

#### Migrate from bitbucket to github

```
    cd <path_to_repo>
    git remote rename origin bitbucket
    git remote add origin <new_repo_url> # add the new origin
    git push -u origin --all # pushes up the repo and its refs for the first time
    git push -u origin --tags # pushes up any tags
```
<br/>

#### Rename github repo
Create a new git repo with the new name

```
    cd <path_to_repo>
    git remote rename origin old
    git remote add origin <new_repo_url>
    git push -u origin --all # pushes up the repo and its refs for the first time
    git push -u origin --tags # pushes up any tags
```
<br/>

### <a id="additional-resources"></a>Additional resources
* [What is origin in GIT](http://stackoverflow.com/questions/9529497/what-is-origin-in-git) - By saying `git push origin branchname` you're saying to push to the origin repository. There's no requirement to name the remote repository origin, and there can be multiple remote repositories.

* [Fetch and Merge](http://longair.net/blog/2009/04/16/git-fetch-and-merge/)
* [Distributed Version Control Systems and the Enterprise](http://stackoverflow.com/questions/5683253/distributed-version-control-systems-and-the-enterprise-a-good-mix/5685757#5685757)
    * Explain how git fits into the enterprise environment
    * It seems that the only way to have fine-grained access control in git is to add another layer, such as `gitolite`
* [How to make a git repository read-only?](http://stackoverflow.com/questions/1662205/how-to-make-a-git-repository-read-only)
    * Seems that there's no way to limit read access at folder level.
* [Difference between git pull and fetch](http://stackoverflow.com/questions/2602546/how-do-i-git-fetch-and-git-merge-from-a-remote-tracking-branch-like-git-pu)
* [Github flow](http://scottchacon.com/2011/08/31/github-flow.html)

[TOC](#user-content-toc)

