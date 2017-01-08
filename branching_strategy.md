# BRANCHING STRATEGY

Branching strategy is an important part of using git correctly. Without a proper branching strategy, even with a few people in the project, you will create a mess in no time.

## Branch Description
### master
Master branch represents what's in live. It should not be modified directly.

When it is deployed, it will be tagged so that we can revert to a certain deployment later.

Lifetime: forever

### release/xxx

After finishing a feature, make a release branch off the develop branch, merge all the necessary feature branches and do final test. Bugs found in this final QA phases can also be hotfixed in the same branch. After everything is complete, we will rebase this branch over `develop` branch, then the rebase branch will be merged into the `master` branch for release in Live environment.

Lifetime: Feature Finish ~ Release in Live

### hotfix/xxx

If a bug is found in production, we wil branch hotfix branch off master.

Lifetime: Bugfix ~ Release in Live

### develop

This is the main branch where feature branches should be branched off. Trivial changes can be directly applied to develop if you are sure it should go into the next version. Major feature/bugfix branches have to go through PR process to be merged into develop.

Lifetime: forever

### feature/xxx
Feature branch should be branched off develop. While working on feature branches, developers can choose to merge develop into it. Once QA is completed on the feature branch, developers can send PR.

Lifetime: Feature development ~ PR merge

### art
Art branch lives forever. Artists only need to deal with this branch.

Lifetime: forever

## Role Descriptions
### Engineer / UIDesigner
- develop -> feature
- feature -> develop (PR)

### Artist
- art -> art

### Lead Engineer
- develop -> release
- release -> develop
- master -> hotfix
- hotfix -> master
- hotfix -> develop
