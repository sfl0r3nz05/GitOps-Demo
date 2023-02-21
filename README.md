# GitOps experimental repository for demonstration purposes.

- The purpose of this repository is to carry out demonstrations as an example of GitOps use, considering the following steps:

    1. App source code repository
    2. CI pipeline creating a container image
	3. Container image registry
	4. Kubernetes manifests repository
    5. GitOps engine syncing manifests to one or more clusters and detecting drifts

    ![image](https://user-images.githubusercontent.com/6643905/220208966-654a7cde-d638-4960-ab13-cdf9f112cefd.png)

## Requirements

1. [Install a demo cluster based on Minikube](./documentation/minikube.md)

## Repository structure:

    ```console
    |_ /source-code     #contains the source code to be used in the other folders
    |_ /docker-build-shipwright-kaniko      #contains the .yaml files to send a container to a public registry using shipwright and kaniko
    |_ /docker-build-kaniko     #contains the .yaml file to send a container to a public registry using kaniko
    ```

## Building a Container Using Shipwright and kaniko in Kubernetes

## Building a Container using kaniko