# infra: A cli tool to create/scale/delete k8s clusters and deploy manifest files.

Currently it supports GKE, but it is designed in a way that adding more providers should be easy.

### Parsing of files

Files passed to `infra` will be parsed using golang templates, to skip parsing and load the file as is, use `noparse` suffix.

Eg. `somefile.yaml` will be parsed, whereas `somefile_noparse.yaml` will not be parsed.

## Usage and examples:

[embedmd]:# (infra-flags.txt)
```txt
usage: infra [<flags>] <command> [<args> ...]

The prometheus/test-infra deployment tool

Flags:
  -h, --help           Show context-sensitive help (also try --help-long and
                       --help-man).
  -f, --file=FILE ...  yaml file or folder that describes the parameters for the
                       object that will be deployed.
  -v, --vars=VARS ...  When provided it will substitute the token holders in the
                       yaml file. Follows the standard golang template formating
                       - {{ .hashStable }}.

Commands:
  help [<command>...]
    Show help.

  gke info
    gke info -v hashStable:COMMIT1 -v hashTesting:COMMIT2

  gke cluster create
    gke cluster create -a service-account.json -f FileOrFolder

  gke cluster delete
    gke cluster delete -a service-account.json -f FileOrFolder

  gke nodepool create
    gke nodepool create -a service-account.json -f FileOrFolder

  gke nodepool delete
    gke nodepool delete -a service-account.json -f FileOrFolder

  gke nodepool check-running
    gke nodepool check-running -a service-account.json -f FileOrFolder

  gke nodepool check-deleted
    gke nodepool check-deleted -a service-account.json -f FileOrFolder

  gke resource apply
    gke resource apply -a service-account.json -f manifestsFileOrFolder -v
    PROJECT_ID:test -v ZONE:europe-west1-b -v CLUSTER_NAME:test -v
    hashStable:COMMIT1 -v hashTesting:COMMIT2

  gke resource delete
    gke resource delete -a service-account.json -f manifestsFileOrFolder -v
    PROJECT_ID:test -v ZONE:europe-west1-b -v CLUSTER_NAME:test -v
    hashStable:COMMIT1 -v hashTesting:COMMIT2


```

### Building Docker Image

```
docker build -t prominfra/infra:master .
```