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


# Clone a repository
```
/Projects$ git clone https://github.com/project-name-here.git
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

## Update your working branch on Github
```
git checkout issue_1
git commit -m “fixed issue 1”
git push
```

## Reset you projest as Master branch, lose all your local progress
```
git fetch
git reset --hard origin/master
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

# History
See your commit history
```
$  git log --pretty=format:"%h %s" --graph
# Full commit id
$  git log --pretty=format:"%H %s" --graph
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

