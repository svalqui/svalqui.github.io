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
