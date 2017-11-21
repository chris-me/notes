- [Undoing Changes](#undoing-changes)



# Undoing Changes

## git revert

Create a new commit in which the changes from commit B to HEAD are undone, keeping git history intact.

```bash
# revert all commits from B to HEAD, inclusively
$ git revert --no-commit B..HEAD  
$ git commit -m 'message'
```

## Blow away a complete commit

Best done before having pushed a commit to remote!

see also https://stackoverflow.com/questions/927358/how-to-undo-the-last-commits-in-git

```bash
# git reset --hard HEAD~1
```
