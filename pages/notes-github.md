== Token Scope requirement
![Scope-requirement](https://.png)

Set your git
```
git config --list
git config --global user.name "My Name"
git config --global user.email my@email@users.noreply.github.com # for github project
```

Check my remotes
```
git remote -v
```

Update your working branchon Github
```
git checkout issue_1
git commit -m “fixed issue 1”
git push
```

Reset you projest as Master branch, lose all your local progress
```
git fetch
git reset --hard origin/master
```
