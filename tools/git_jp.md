# Git

### Refreshing [[git]] repos

Found this command to go through multiple [[git]] repos and refresh them from their respective remotes.

```bash
find . -mindepth 1 -maxdepth 1 -type d -print -exec git -C {} pull \;
```


If you want to pull from a specific branch: ( ie. pulling from the remote named origin, and the branch named main )

```bash
find . -mindepth 1 -maxdepth 1 -type d -print -exec git -C {} pull origin main \;
```


