# teleport-cheat-sheet
 Information and shortcuts for Teleport's tsh and tctl binaries.

If you want to contribute please feel free to fork and submit a PR!

Let's do this!

## Introduction

Teleport has 2 client binaries that are useful for users and admins to manage Teleport resources as well as cluster admin related work.

- `tsh` - This is the main Teleport client binary
- `tctl` - This is the main Teleport admin client binary

## TSH Cheat Sheet

- [Official documenation for TSH](https://goteleport.com/docs/server-access/guides/tsh/)
- [CLI reference](https://goteleport.com/docs/setup/reference/cli/)

### Common Commands

| Command | Description |
| --- | ---
|`tsh --help`, `tsh help` | Shows context help information. Running  a sub-command of `tsh ls help` will not work but `tsh ls --help` will.
| `tsh ls` | List remote SSH nodes/resources
| `tsh ls --format=names` | List SSH node names only. Can also use `--format=json` and `--format=text`
| `tsh ls -f names` | Same as above.
| `tsh status` | List current status of CLI user.
| `tsh login --user=USER --auth=OIDC --proxy=FQDN:443` | Attempts to authenticate a user with selected OIDC/SAML connector via the default browser. The `--auth=` option can be changed to `github`, `okta`, etc.
| `tsh login --user=USER --auth=OIDC --proxy=FQDN:443 --browser=none` | Posts the loopback URL for copy and paste in your browser (does not auto-launch your browser to login)
| `tsh kube ls` | List all Kubernetes clusters
| `tsh kube login <k8s cluster name>` | Login to specific K8s cluster
| `tsh db ls` | List all connected databases
| `tsh db login <db name>` | Login to specific database
| `tsh logout` | Logout of Teleport (including K8s and DB)
| `tsh ssh <user>@<host>` | SSH into managed node
| `tsh ssh <user>@<host> <command>` | Execute remote command on host. Example: `tsh ssh <user>@<host> teleport version` returns Teleport version
| `tsh join -l <user> <session-id>` | Joins existing Teleport SSH session with specified user and session-id

## TCTL Cheat Sheet

- [Official documenation for TCTL](https://goteleport.com/docs/server-access/guides/tctl/)
- [CLI reference](https://goteleport.com/docs/setup/reference/cli/)

### Common Commands


| Command | Description |
| --- | ---
|`tctl --help`, `tctl help` | Shows context help information. Running  a sub-command of `tsh rm help` will also work but `tsh not --help` will not.
| `tctl status` | Shows cluster status, version information, FQDN, etc
| `tctl auth sign --user=<USER> --out=<NAME> --format=openssh --ttl=<TIME>` | Will export out identity files. Depending on `--format` specified (`file`, `openssh`, `tls`, `kubernetes`, `db`, `mongodb`)
