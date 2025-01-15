# Intro

This repository includes tooling and scripts for running growteer locally with as least effort as possible.

It sets up all of its dependencies in a minikube cluster.

# Prerequisites

## Install the Following Tools

- [Taskfile](https://taskfile.dev/installation/)
- [minikube](https://minikube.sigs.k8s.io/docs/start/)
- [helm](https://helm.sh/docs/intro/install/)
- [docker](https://docs.docker.com/get-started/get-docker/)

# Firing it Up

The repo provides a couple of taskfile tasks to spinning up the environment, most notably the following:

`task boot` starts the minikube cluster with the configured kubernetes version and sets your kube context to this cluster

`task expose` simply wraps `minikube tunnel`, exposing the cluster services to localhost

## Further Utilities

For all services that can be installed to the cluster, there are two accompanying tasks:

1. Prefixing the name with "un-" will uninstall the component, e.g. `task un-mongo`
2. Prefixing the name with "fw-" will forward the service to localhost, e.g. when running `task fw-mongo` you can then access the database at _localhost:5432_
