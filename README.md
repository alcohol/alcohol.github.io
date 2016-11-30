# alcohol.github.io

## commandlinefu

Some commandlinefu oneliners I find myself using regularly.

### git

```shell
# delete branches that have been merged into origin/develop
git branch --remote --merged origin/develop \
    | grep -v master | grep -v develop | sed 's/origin\///' \
    | xargs --no-run-if-empty git push origin --delete --dry-run
```

### docker

```shell
# remove all stopped containers
docker ps -aq | xargs --no-run-if-empty docker rm

# remove all unused volumes
docker volume ls -q | xargs --no-run-if-empty docker volume rm

# remove local volumes  
docker volume ls | awk '/^local/ { print $2 }' | xargs --no-run-if-empty docker volume rm

# delete untagged images
docker images --filter dangling=true -q | xargs --no-run-if-empty docker rmi
```
