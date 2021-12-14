# Teleport Commands Cheat Sheet
 Information and shortcuts for Teleport's `tsh` and `tctl` binaries.

If you want to contribute please feel free to fork and submit a PR!

Let's do this!

## Introduction

Teleport has 2 client binaries that are useful for users and admins to manage Teleport resources as well as cluster admin related work.

- `tsh` - This is the main Teleport client binary
- `tctl` - This is the main Teleport admin client binary

## `tsh` Cheat Sheet

- [Official documenation for TSH](https://goteleport.com/docs/server-access/guides/tsh/)
- [CLI reference](https://goteleport.com/docs/setup/reference/cli/)


## Key directories

- Short-lived certificates, etc are normally stored on in your `$HOME/.tsh` directory.

### Common Commands

| Command | Description |
| --- | ---
|`tsh --help`, `tsh help`, `tsh <command> -h` | Shows context help information. Running  a sub-command of `tsh ls help` will not work but `tsh ls --help` will.
| `tsh ls` | List remote SSH nodes/resources
| `tsh ls --format=names` | List SSH node names only. Can also use `--format=json` and `--format=text`
| `tsh ls -f names` | Same as above.
| `tsh login --user=USER --auth=OIDC --proxy=FQDN:443` | Attempts to authenticate a user with selected OIDC/SAML connector via the default browser. The `--auth=` option can be changed to `github`, `okta`, etc.
| `tsh login --user=USER --auth=OIDC --proxy=FQDN:443 --browser=none` | Posts the loopback URL for copy and paste in your browser (does not auto-launch your browser to login)
| `tsh logout` | Logout of Teleport (including K8s and DB)
| `tsh request ls` | List current Access Requests
| `tsh request show <request-id>` | Show details of request
| `tsh status` | List current status of CLI user.

### `tsh` SSH Commands

| Command | Description |
| --- | ---
| `tsh join -l <user> <session-id>` | Joins existing Teleport SSH session with specified user and session-id
| `tsh ssh <user>@<host>` | SSH into managed node
| `tsh ssh <user>@key=value` | SSH into managed node using labels
| `tsh ssh <user>@<host> <command>` | Execute remote command on host. Example: `tsh ssh <user>@<host> teleport version` returns Teleport version


### `tsh` Kubernetes Commands

| Command | Description |
| --- | ---
| `tsh kube --help` | Show additional context help for K8s access
| `tsh kube ls` | List all Kubernetes clusters
| `tsh kube login <k8s cluster name>` | Login to specific K8s cluster

### `tsh` Database Commands

| Command | Description |
| --- | ---
| `tsh db --help` | Show additional context help for DB access
| `tsh db config` | Lists your current DB configuration information
| `tsh db env` | List your current logged in DB environment settings
| `tsh db ls` | List all connected databases
| `tsh db login <db name>` | Login to specific database

### Other Shortcuts

| Command | Description |
| --- | ---
| `tsh ls -f json \| jq -r '.[] \| [.spec.hostname,.metadata.name] \| @tsv'` | Parse node name and uuid using `jq`
| `tsh ls -v \| awk '{print $1, $2}' \| column -t` | Parse Server name and UUID using `awk`

## `tctl` Cheat Sheet

- [Official documenation for TCTL](https://goteleport.com/docs/server-access/guides/tctl/)
- [CLI reference](https://goteleport.com/docs/setup/reference/cli/)

### Common Commands

| Command | Description |
| --- | ---
|`tctl --help`, `tctl help` | Shows context help information. Running  a sub-command of `tsh rm help` will also work but `tsh not --help` will not.
| `tctl auth sign --user=<USER> --out=<NAME> --format=openssh --ttl=<TIME>` | Will export out identity files. Depending on `--format` specified (`file`, `openssh`, `tls`, `kubernetes`, `db`, `mongodb`)
| `tctl get locks --format text` | Shows all locks on the cluster formatted in a table (vs default `.json` output)
| `tctl get rc` | Gets the remote cluster (rc) information ~ `leaf` clusters
| `tctl nodes add --roles=kube --ttl=10000h --format=json \| jq -r '.[0]'` | Creates a `node` join token and outputs the token (requires `jq`)
| `tctl rm nodes/<uuid>` | Remove node from Teleport list
| `tctl rm rc/<cluster_name>` | Removes trusted cluster/remote cluster from `root` cluster
| `tctl status` | Shows cluster status, version information, FQDN, etc
| `tctl top` | Shows a CLI graphical interface to view Teleport cluster statistics and more

### Other Commands

| Command | Description |
| --- | --- |
| `(for i in auth node proxy; do tctl get $i; done ) \| egrep -i 'hostname: \| version' \| grep -vi 'v2'` | Gets all cluster component versions