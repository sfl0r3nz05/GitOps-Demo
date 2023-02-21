# GitOps experimental repository for demonstration purposes.

- The purpose of this repository is to carry out demonstrations as an example of GitOps use, considering the following steps:

    1. App source code repository
    2. CI pipeline creating a container image
	3. Container image registry
	4. Kubernetes manifests repository
    5. GitOps engine syncing manifests to one or more clusters and detecting drifts

    ![image](https://user-images.githubusercontent.com/6643905/220208966-654a7cde-d638-4960-ab13-cdf9f112cefd.png)

## Requirements

1. [Install a demo cluster based on Minikube is a requierements for all tests](./documentation/minikube.md)

## Repository structure:

```bash
|_README.md
|_/documentation
|_/source-code
|_/docker-build-shipwright-kaniko
|_/docker-build-kaniko
```

- `/documentation`: contains *.md files with project information.
- `/source-code`: contains the source code to be used in the other folders.
- `/docker-build-shipwright-kaniko`: contains the .yaml files to send a container to a public registry using shipwright and kaniko.
- `/docker-build-kaniko`: contains the .yaml file to send a container to a public registry using kaniko.

## Building a Container Using Shipwright and kaniko in Kubernetes

   > **Pre-requisites:** A deployed cluster. E.g.: Minikube.

   1. [Prepare the cluster to support Shipwright and select Kaniko as build strategy](./documentation/shipwright-installation.md)
   2. [Follow these steps to build a container using kaniko as Shipwright build strategy.](./documentation/docker-build-shipwright-kaniko.md)

## Building a Container using kaniko

   > **Pre-requisites:** A deployed cluster. E.g.: Minikube.

  1. [Follow these steps to build a container using kaniko.](./documentation/docker-build-kaniko.md)

## Deploy a Service using kuztomize

   > **Pre-requisites:** A deployed cluster. E.g.: Minikube.

  1. [Follow these steps to deploy a service using kuztomize.](./documentation/kuztomize-deployment.md)