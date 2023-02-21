# Prepare the cluster to support Shipwright and select Kaniko as build strategy

1. Install the Tekton dependency:

    ```console
    kubectl apply -f https://storage.googleapis.com/tekton-releases/pipeline/previous/v0.30.0/release.yaml
    ```

2. Install Shipwright

    ```console
    kubectl apply -f https://github.com/shipwright-io/build/releases/download/v0.7.0/release.yaml
    ```

3. Install Shipwright build strategies

    ```console
    kubectl apply -f  https://github.com/shipwright-io/build/releases/download/v0.7.0/sample-strategies.yaml
    ```

4. Test installations

    ```console
    kubectl get cbs
    ```

    - Expected output:

    ```console
    NAME                     AGE
    buildah                  22s
    buildkit                 22s
    buildpacks-v3            22s
    buildpacks-v3-heroku     22s
    kaniko                   22s
    kaniko-trivy             22s
    ko                       22s
    source-to-image          22s
    source-to-image-redhat   22s
    ```
