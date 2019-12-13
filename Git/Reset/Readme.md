# Personal Documentation
## Git / Reset

### Index
- [Reset your Repo](#Reset-your-repo)
- [Reset last Commit](#Reset-last-commit)
- [](./)
- [](./)


### Reset your Repo
```
git reset --hard origin/master
```


### Reset last commit
```
git reset 'commitnummer' --hard
```
Next step, Check if your on the right commit:
```
git log
```
Push this to the remote Repo
```
git push --force
```