# Intro

This repository includes tooling and scripts for running growteer locally with as least effort as possible.

It sets up all of its dependencies in a minikube cluster, including Postgres and a Ceramic Node.

The Helm Chart for the Ceramic Node is based on https://github.com/ceramicstudio/simpledeploy/tree/main/k8s/base/ceramic-one.

# Prerequisites

## Install the Following Tools

- [Taskfile](https://taskfile.dev/installation/)
- [minikube](https://minikube.sigs.k8s.io/docs/start/)
- [helm](https://helm.sh/docs/intro/install/)
- [docker](https://docs.docker.com/get-started/get-docker/)
- [composedb cli](https://www.npmjs.com/package/@composedb/cli)
  - This one requires [nodejs/npm](https://nodejs.org/en/download/) be installed first

# Required Configurations

## Ceramic

### Add Your Own Secrets

Generate a new private key via `composedb did:generate-private-key`

Generate a did based on that private key via `composedb did:from-private-key <your private key>`

Copy _./ceramic/secrets.example.yaml_ to _./ceramic/secrets.yaml_ and replace the placeholders for private key and did with the ones you just generated.

### Set Up Access to the Ceramic Anchor Service

#### Step 1: Email Verification

```bash
curl --request POST \
  --url https://cas-qa.3boxlabs.com/api/v0/auth/verification \
  --header 'Content-Type: application/json' \
  --data '{"email": "<your email address>"}'
```

#### Step 2: Check Your Email

Copy the verification code from the email you received.

#### Step 3: Register Your DID

```bash
curl --request POST \
  --url https://cas-qa.3boxlabs.com/api/v0/auth/did \
  --header 'Content-Type: application/json' \
  --data '{
    "email": "<your email address used in Step 1>",
    "otp": "<your verification code copied in Step 2>",
    "dids": [
		  "<your did generated earlier>"
	  ]
  }'
```

# Firing it Up

The repo provides a couple of taskfile tasks to spinning up the environment, most notably the following:

`task boot` starts the minikube cluster with the configured kubernetes version and sets your kube context to this cluster

`task expose` simply wraps `minikube tunnel`, exposing the cluster services to localhost

`task postgres` installs Postgres

`task ceramic` installs Ceramic

## Further Utilities

For all services that can be installed to the cluster, there are two accompanying tasks:

1. Prefixing the name with "un-" will uninstall the component, e.g. `task un-postgres`
2. Prefixing the name with "fw-" will forward the service to localhost, e.g. when running `task fw-postgres` you can then access the database at _localhost:5432_
