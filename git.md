- [Undoing Changes](#undoing-changes)

# Undoing Changes

## git revert

Create a new commit in which the changes from commit B to HEAD are undone, keeping git history intact.

```bash
# revert all commits from B to HEAD, inclusively
$ git revert --no-commit B..HEAD  
$ git commit -m 'message'
```
