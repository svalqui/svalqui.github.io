== Token Scope requirement
![Scope-requirement](https://.png)

# Set your git username and email

## Globally 
On your home directory on ~/.gitconfig

```
git config --list
git config --global user.name "My Name"
git config --global user.email my@email@users.noreply.github.com # for github project
```
## Per Project 

On your project directory, on file .git/config
```
git config --list
git config user.name "My Name"
git config user.email my.email@for.this.project.org 
```

## even better per repository, set one folder per repository

https://gist.github.com/bgauduch/06a8c4ec2fec8fef6354afe94358c89e

# Setup your token PAT Personal Access Token

```
# cache the token locally
$ git config --global credential.helper cache

# remove it if not longer needed
$ git config --global --unset credential.helper
$ git config --system --unset credential.helper
```

# Clone a repository
```
/Projects$ git clone https://github.com/project-name-here.git

# Clone to a given directory
/Projects$ git clone  https://github.com/project-name-here.git <target-folder>

# Clona a branch
/Projects$ git clone -b <branch-name> https://github.com/project-name-here.git <target-folder>

```
## issues cloning, debugging 
```
$ GIT_SSH_COMMAND="ssh -vvv" git clone ssh://user@yourdomain.org@server.org.au:<port-number>/project-directory
```
# Maintenance

## Check my remotes
```
git remote -v
```
## Check if your local branch is up to date with remote master
```
git remote show origin
```

## Check your local commits not pushed yet
"Your branch is ahead of 'origin/master' by ?? commits."
https://stackoverflow.com/questions/2016901/how-to-list-unpushed-git-commits-local-but-not-on-origin
```
git log origin/master..HEAD
or
git diff origin/master..HEAD
```

## Update your working branch on Github
```
git checkout issue_1
git commit -m “fixed issue 1”
git push
```

## Update your local branch with remote master
```
git pull origin master
```

## Reset you projest as Master branch, lose all your local progress
```
git fetch
git reset --hard origin/master
```
or
```
git fetch --all
git reset --hard origin/master
git pull origin master
```

## Reset a file to be as master deleting all local changes
```
git checkout origin/master <filename>
```

## Set remote origin
```
git remote -v
git remote set-url origin https://repo.git
git remote -v
```

## Check ssh connects to repo

```
ssh -vvvT -p <portnumber> <username>@<repo-domain>
```
# Branch
```
# List branches
git branch -a
# Update your Branches
git remote update origin -prune
# Delete a local branch
$ git branch -d <branch_name>
# List local branches deleted on Github
$ git remote prune origin --dry-run
# Remove references to deleted branches
$ git remote prune origin
```

# History
See the commit history
```
# Short header, time, author name, description, since
$ git log --pretty=format:"%h %cs %an %s" --graph --since="2018-05-09"
# Short header, time, author name, description, since, include file names of changed files
$ git log --pretty=format:"%h %cs %an %s" --graph --since="2018-05-09" --stat
# show it on reverse order
$ git log --pretty=format:"%h %cs %an %s" --since="2018-05-09" --stat --reverse

# Only header and description
$  git log --pretty=format:"%h %s" --graph
# Full commit id
$  git log --pretty=format:"%H %s" --graph

#show only the name of the files changed
$ git log --name-only --oneline

#show on reverse order
$ git log --reverse

#show commits since a given commit
$ git rev-list <since_hash>..HEAD
#show coomits since a given commit including id, author and description
$ git rev-list --pretty=format:"%h %an %s" --graph <since_hash>..HEAD
#show coomits in reverse aorder
$ git rev-list --reverse --pretty=format:"%h %an %s" <since_hash>..HEAD

```
Search for commits containing
```
$ git log --all --grep='text-to-look-for'
```
See the changes made by a single commit
```
git diff <commit-hash>~ <commit-hash>
or
git show --color --pretty=format:%b <commit-hash>
```
See the only the files changed by a commit
```
git diff-tree --no-commit-id --name-only -r <commit-has>
or
git diff-tree -p <commit-hash>
or
git show --pretty="" --name-only <commit-hash>
```
Commits by user
```
git shortlog -s
```
# Troubleshooting
```
GIT_SSH_COMMAND="ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -vvv" git pull
```
# Gerrit
Install Git Review
```
$ pip install git-review
```
Initialize
```
$ git review -s
```
Configure git-review
```
$ git config --global gitreview.remote origin
```
Clone a project to work on
```
$ git clone ssh://gerrit-site/project01
```
Set the email for the project
```
/project01$ git config user.email <email-allowed-on-gerrit-project>
```
check email
```
$ git config user.email
```
Update master
```
$ git pull origin master
```
Create a branch
```
$ git checkout -b name_of_branch origin/master # That checkout of master branch, otherwise indicate the brach you want to checkout of
```
Change to branch
```
$ git branch <branch-name>
```

Making changes to remote branch
```
# Checkout a remote branch
$ git checkout remotes/origin/my-remote-branch # This could leave you in a detached HEAD state, watch out

# Make your changes

# push to the remote branch
$ git push origin HEAD:my-remote-branch
or
$ git review -r my-remote-branch
```


Logging a git-review ("pull request")
- create your new working branch: $ git checkout -b name_of_branch origin/master
- edit/ create the files you need
- add the files: git add ...
- commit: $ git commit -a -m "Change..." 
- git review: $ git review

Updating a git-review
- edit the files needing change
- add the files: git add files
- amend: $ git commit --amend
- git review: $ git review

# References
https://stackoverflow.com/questions/7693249/how-to-list-commits-since-certain-commit
