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

GCR_HOST=${GCR_HOST:-eu.gcr.io}
GCR_PROJECT=${GCR_PROJECT:-your-project-1234}
GCR_IMAGE=${GCR_IMAGE:-vendor/name}

# fetch all tags using list-tags, extract using jq and pass to gnu parallel for deletion
endpoint="${GCR_HOST}/${GCR_PROJECT}/${GCR_IMAGE}"
gcloud container images list-tags ${endpoint} --format json \
| jq --raw-output '.[].digest' \
| parallel --bar --no-keep-order gcloud container images delete ${endpoint}@{} --force-delete-tags
```
