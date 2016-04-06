# For developers

## Branching Model

using [a simple git branching model](https://gist.github.com/jbenet/ee6c9ac48068889b0912#file-simple-git-branching-model-md)

1. `master` must always be deployable.
1. **all changes** made through feature branches (pull-request + merge)
1. rebase to avoid/resolve conflicts; merge in to `master`

## The workflow

```bash
# everything is happy and up-to-date in master
git checkout master
git pull origin master

# let's branch to make changes
git checkout -b my-new-feature

# go ahead, make changes now.
$EDITOR file

# commit your (incremental, atomic) changes
git add -p
git commit -m "my changes"

# keep abreast of other changes, to your feature branch or master.
# rebasing keeps our code working, merging easy, and history clean.
git fetch origin
git rebase origin/my-new-feature
git rebase origin/master

# optional: push your branch for discussion (pull-request)
#           you might do this many times as you develop.
git push origin my-new-feature

# optional: feel free to rebase within your feature branch at will.
#           ok to rebase after pushing if your team can handle it!
git rebase -i origin/master

# merge when done developing.
# --no-ff preserves feature history and easy full-feature reverts
# merge commits should not include changes; rebasing reconciles issues
# github takes care of this in a Pull-Request merge
git checkout master
git pull origin master
git merge --no-ff my-new-feature


# optional: tag important things, such as releases
git tag 1.0.0-RC1
```

## DOs and DON'Ts

No DO or DON'T is sacred. You'll obviously run into exceptions, and develop 
your own way of doing things. However, these are guidelines I've found
useful.

### DOs

- **DO** keep `master` in working order.
- **DO** rebase your feature branches.
  - **DO** pull in (rebase on top of) changes
  - **DO** tag releases
  - **DO** push feature branches for discussion
  - **DO** learn to rebase


### DON'Ts

  - **DON'T** merge in broken code.
  - **DON'T** commit onto `master` directly.
    - **DON'T** hotfix onto master! use a feature branch.
    - **DON'T** rebase `master`.
    - **DON'T** merge with conflicts. handle conflicts upon rebasing.

## Useful configure
-  basic short cut to command tasks.

```bash
    git config --global alias.co checkout
    git config --global alias.br branch
    git config --global alias.ci commit
    git config --global alias.st status
    git config --global alias.df 'diff --color'
```
- unstage alias:

```bash
    git config --global alias.unstage 'reset HEAD --'
```
  So following commands have same effect:

```bash
    git unstage fileA 
    git reset HEAD fileA
```

- see last change

```bash
    git config --global alias.last 'log -1 HEAD'
```

- pretty log format

```bash
    git config --global alias.lg "log --color --graph \
        --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset \
        %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```
you can use `git lg` or `git log -p` to see the pretty formated log.

- autosetup rebase

```bash
    # autosetup rebase so that pulls rebase by default
    git config --global branch.autosetuprebase always
```

![image](http://nvie.com/img/git-model@2x.png)