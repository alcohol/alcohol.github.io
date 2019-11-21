# alcohol.github.io

Some randomly collected snippets

### homebrew

``` shell
# stuff I like to install
brew install \
  vim coreutils findutils gnu-tar gnu-sed gnupg rename tmux tree watch \
  --with-default-names
```

### git

``` shell
# delete branches that have been merged into origin/develop (excluding develop/master)
git branch --remote --merged origin/develop \
  | grep -v master | grep -v develop | sed 's/origin\///' \
  | xargs --no-run-if-empty git push origin --delete --dry-run
```

### ansible

``` shell
# be able to diff vault files
vimdiff \
  <(ansible-vault view --vault-password-file=.vault_password \
    <(git show origin/master:inventory/group_vars/production/secrets)) \
  <(ansible-vault view --vault-password-file=.vault_password \
    <(git show origin/develop:inventory/group_vars/production/secrets))
```

### jenkins

``` groovy
// decrypt a secretbytes string (ssh-key/file credential)
println(new String(com.cloudbees.plugins.credentials.SecretBytes.fromString(secret).getPlainData(), "ASCII"))

// decrypt a regular secret
println(hudson.util.Secret.fromString(secret).getPlainText())
```

### gcr

``` bash
#!/usr/bin/env bash

function list_roots
{
  gcloud container images list --repository "$1" --format json | \
  jq --raw-output '.[].name'
}

function list_images
{
  gcloud container images list --repository "$1" --format json | \
  jq --raw-output '.[].name'
}

function list_tags
{
  gcloud container images list-tags "$1" --filter 'timestamp.datetime < 2019-08-01' --format json | \
  jq --raw-output '.[].digest as $digest | "'$1'@" + $digest'
}

export -f list_roots list_images list_tags

parallel --no-run-if-empty list_roots ::: eu.gcr.io/project-1234 | \
parallel --no-run-if-empty list_images | \
parallel --no-run-if-empty list_tags | \
parallel --no-run-if-empty --jobs 50 --bar --eta --no-keep-order \
  gcloud container images delete {} --quiet --force-delete-tags
```
