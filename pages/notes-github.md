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
# Clone a repository
```
/Projects$ git clone https://github.com/project-name-here.git
```
## issues cloning, debugging 
```
$ GIT_SSH_COMMAND="ssh -vvv" git clone ssh://user@yourdomain.org@server.org.au:<port-number>/project-directory
```
# Check my remotes
```
git remote -v
```

# Update your working branchon Github
```
git checkout issue_1
git commit -m “fixed issue 1”
git push
```

# Reset you projest as Master branch, lose all your local progress
```
git fetch
git reset --hard origin/master
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
Update master
```
$ git pull origin master
```
Create a branch
```
$ git checkout -b name_of_branch origin/master
```
Change to branch
```
$ git branch
```
Logging a git-review ("pull request")
- create your new working branch: $ git checkout -b name_of_branch origin/master
- edit/ create the files you need
- add the files and commit: $ git commit -a -m "Change..." 
- git review: $ git review

Updating a git-review
- edit the files needing change
- add the files: git add files
- amend: $ git commit --amend
- git review: $ git review

