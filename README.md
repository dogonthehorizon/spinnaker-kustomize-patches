# spinnaker-kustomize-patches

This repository contains example [kustomize](https://kustomize.io) patch files to configure and deploy Spinnaker using the Spinnaker Operator.

### Disclaimer

The example configurations provided in this repository serve as a starting point for configuring Spinnaker, usually they may need to be adjusted to the environment where Spinnaker is running to work properly. These examples are not exhaustive and don't showcase all available combinations of settings. It's possible that not all configurations work with all versions of Spinnaker. 

### Prerequisites

* You need to have a working Kubernetes cluster, and be able to execute `kubectl` commands against that cluster, with permissions to list and create namespaces.

### Quick start

Run `./deploy.sh`. 

It will deploy Spinnaker Operator to `spinnaker-operator` namespace, and a base Spinnaker instance to `spinnaker` namespace with some default integrations.

### General usage

1. Make a link from `kustomization.yml` to one of the example kustomization files in `recipes` folder depending on your use case.
1. Modify `kustomization.yml` by adding or removing patches depending on what you want to be included in spinnaker. [Kustomization Reference Documentation describes the syntax of this file](https://kubectl.docs.kubernetes.io/pages/reference/kustomize.html).
1. Change any of the kustomize patch files to match your desired configuration. For example changing github username, aws account id, etc.
1. Store secret literals in `secrets/secrets.env` and secret files in `secrets/files` if you want to store spinnaker secrets in Kubernetes. They are ignored by source control.
1. Run `./deploy.sh` to deploy spinnaker. 

* Namespace for the Spinnaker Operator is configured in `operator/kustomization.yml`.
* Namespace for Spinnaker and all its infrastructure is configured in `kustomization.yml`.
* Spinnaker version is configured in `spinnakerservice.yml`.
* Environment variable `SPIN_OP_DEPLOY` can be passed to deploy script to manage operator (default) or not (i.e. `SPIN_OP_DEPLOY=0 ./deploy.sh`)

For adding remote Kubernetes clusters to Spinnaker, the helper script `secrets/create-kubeconfig.sh` can be used to create a Kubernetes service account (with cluster admin role) and its corresponding `kubeconfig` file for spinnaker to use.

### OSS Spinnaker or Armory Spinnaker

All kustomize patch files in this repository are for Armory Spinnaker distribution. For using with OSS Spinnaker you need to change their `apiVersion` by removing `armory` from it. For example, 
```
apiVersion: spinnaker.armory.io/v1alpha2
```
changes to: 
```
apiVersion: spinnaker.io/v1alpha2
```
The script `deploy.sh` automatically does this to deploy OSS Spinnaker when run with the `SPIN_FLAVOR` environment variable:
```bash
SPIN_FLAVOR=oss ./deploy.sh
```
