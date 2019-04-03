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
