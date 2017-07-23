# GIT TUTORIALS
### Add Existing Repository to GITHUB
```sh
git remote add origin <GIHUB REPOSITORY URL>
git remote -v
git push -u origin master
```
#### Handling non-fast-forward-errors
```sh
git fetch origin
git merge origin master
```
or
```sh
git pull origin master
```
