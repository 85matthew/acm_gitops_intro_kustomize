# acm_gitops_intro_kustomize

# Using nomos to hydrate a repo


### Clone this repo
`git clone https://github.com/85matthew/acm_gitops_intro_kustomize`

### Play around with kustomize to see how it renders the config based on different cluster overlays <br>

_For example_ <br>
`kustomize build cluster/overlays/prod/cluster1`

(View the output)<br>
`kustomize build cluster/overlays/prod/cluster2`

(View the output)<br>
`kustomize build cluster/overlays/staging/cluster1`

(View the output)<br><br>

Note that each yaml file (document) is separated by '---' within the output <br>
This is useful to view quickly from the CLI


### Install Nomos (this is for the current supported- refer to additional documentation to utilize preview features)
`gcloud components install`<br>

Nomos can be used to hydrate DRY repos and make them pure yaml format <br>
Use the below format to 'hydrate' any cluster configuration into a 'wet' directory or repo

### Set vars as an example, these can be changed to render the prod or cluster2 settings as well.
```
ENV=staging
CLUSTER=cluster1

nomos vet --path ./cluster/overlays/$ENV/$CLUSTER --source-format unstructured --keep-output --output ./hydrated-test/$ENV/$CLUSTER
```

### To be explicit, this is what we are running for each of the 3 clusters

```
nomos vet --path ./cluster/overlays/staging/cluster1 --source-format unstructured --keep-output --output ./hydrated-test/staging/cluster1
nomos vet --path ./cluster/overlays/prod/cluster1 --source-format unstructured --keep-output --output ./hydrated-test/prod/cluster1
nomos vet --path ./cluster/overlays/prod/cluster2 --source-format unstructured --keep-output --output ./hydrated-test/prod/cluster2
```

### Take a look at the hydrated cluster configurations under the ./hydrated-test/$ENV/$CLUSTER directory
```
user@gmattw-workstation:~/code/acm_gitops_intro_kustomize$ tree hydrated-test/
hydrated-test/
|-- prod
|   |-- cluster1
|   |   `-- config-management-system
|   |       |-- rootsync_prod-gto-global-rootsync.yaml
|   |       `-- rootsync_prod-gto-sitea-rootsync.yaml
|   `-- cluster2
|       `-- config-management-system
|           |-- rootsync_prod-gto-global-rootsync.yaml
|           `-- rootsync_prod-gto-siteb-rootsync.yaml
`-- staging
    `-- cluster1
        `-- config-management-system
            |-- rootsync_staging-gto-global-rootsync.yaml
            `-- rootsync_staging-gto-sitea-rootsync.yaml

```
### Note that variable are set appropriately for each cluster specific setting, site settings, and global settings
```
user@gmattw-workstation:~/code/acm_gitops_intro_kustomize$ diff hydrated-test/prod/cluster1/config-management-system/rootsync_prod-gto-global-rootsync.yaml hydrated-test/staging/cluster1/config-management-system/rootsync_staging-gto-global-rootsync.yaml
5,8c5
<       environment: prod
---
>       environment: staging
```


