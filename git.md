# Git

## Getting started

### Installing

```
sudo add-apt-repository ppa:git-core/ppa
sudo apt-get install git
```

### Configuring

```
git config --global user.name "minhtaile2712"
git config --global user.email "minhtaile2712@gmail.com"
git config --global init.defaultBranch 'main'
git config --global credential.helper store
git config --global alias.loga 'log --oneline --graph --all'
```

### Cloning

Cloning into a different named directory

```
git clone https://github.com/minhtaile2712/Notes my-notes
```

Naming the origin

```
git clone -o tai-main https://github.com/minhtaile2712/Notes my-notes
```

## 2. Staging

Checking the states of files

```
git status
```

Add new files for tracking

```
git add <file-name> ...
git add .
```

See what you’ve changed but not yet staged

```
git diff
```

View the changes you staged for the next commit

```
git diff --cached

or

git diff --staged
```

Unstage a staged file

```
git restore --staged file1
```

Unmodifying a modified file

```
git restore file1
```

## Files

### Remove / Delete

Remove the file from your staging area but keep the file in your working tree

Keep the file on your hard drive but not have Git track it anymore

```
git rm --cached file1
```

Remove files from your working directory and your tracked files

```
git rm file1 file2
```

### Rename

```
git mv filefrom fileto
```

## Commits

```
git commit -m "first commit"
```

Stage and commit every file that is already tracked

```
git commit -a -m "stage every file that is already tracked"
```

Undoing Things

```
git commit --amend
```

## Branches

Create a new branch and swith to it

```
git branch testing
git checkout testing

or

git switch -c testing

or

git checkout -b testing
```

Return to your previously checked out branch

```
git switch -
```

Delete branch

```
git branch -d hotfix
```

List all branches

```
git branch
```

List all branches with last commit

```
git branch -
```

```
### See all the branches that haven’t yet been merged in

$ git branch --no-merged

### Changing a branch name

$ git branch --move bad-name new-name

### To see the corrected branch on the remote, push it

$ git push --set-upstream origin corrected-branch-name
$ git push origin --delete bad-branch-name

$ git branch --move master main
$ git push --set-upstream origin main
$ git push origin --delete master

## Remote Branches

### Get a full list of remote references

$ git ls-remote <remote>
$ git remote show <remote>

## Merging (test to main)

$ git checkout main
$ git merge test
```

## Remotes

```
### Show your remotes

$ git remote
$ git remote -v

### Add new remote

$ git remote add origin2 https://github.com/minhtaile2712/test2.git

### Fetches any data from server that you don’t yet have

git fetch

## Logging

### Show commit history for the desired branch

$ git log testing

### Print out the history of your commits,

### showing where your branch pointers are

### and how your history has diverged.

$ git log --oneline --decorate --graph --all
$ git log --oneline --decorate

## find dangling commit

git fsck --lost-found

## Tracking

git checkout --track origin/serverfix
git checkout -b sf origin/serverfix
```

## Submodules

### Add submodule

```
git submodule add https://github.com/minhtaile2712/minhtaile2712 path/to/subm
git submodule init
git submodule update
```

### Remove submodule

Remove the submodule entry from .git/config

```
git submodule deinit -f path/to/submodule
```

Remove the submodule directory from the superproject's .git/modules directory

```
rm -rf .git/modules/path/to/submodule
```

Remove the entry in .gitmodules and remove the submodule directory located at path/to/submodule

```
git rm -f path/to/submodule
```

### Clone repository with submodules

```
git clone --recurse-submodules https://github.com/chaconinc/MainProject
```

#### For already cloned project

```
git submodule init
git submodule update
```

Or

```
git submodule update --init
```

Or for nested submodules

```
git submodule update --init --recursive
```

### Fetch and update submodules

```
git submodule update --remote
```
