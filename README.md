# alcohol.github.io

## commandlinefu

Some commandlinefu oneliners I find myself using regularly.

### cleanup merged branches

```
git remote prune origin --dry-run
git branch --remote --merged origin/develop \
    | grep -v master | grep -v develop | sed 's/origin\///' \
    | xargs git push origin --delete --dry-run
```
