- [Misc](#misc)
- [Undoing Changes](#undoing-changes)

# Misc

## fast-forward + push branch without checkout

```bash
# example: fast forward master to development
$ git fetch . development:master
# push branch
$ git push origin master
```


# Undoing Changes

## git revert

Create a new commit in which the changes from commit B to HEAD are undone, keeping git history intact.

```bash
# revert all commits from B to HEAD, inclusively
$ git revert --no-commit B..HEAD  
$ git commit -m 'message'
```

## Blow away a complete commit

see also https://stackoverflow.com/questions/927358/how-to-undo-the-last-commits-in-git

Reset the current branch to a specific commit

```bash
git reset --hard HEAD~1
# or
git reset --hard master~1
```

Force overwrite the remote branch:

```bash
git push -f
```


