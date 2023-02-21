# Deploy a Service using kuztomize

1. Apply the kustomization file into a running cluster by running the following command:

    ```console
    kubectl apply --dry-run=client -o yaml -k ./
    ```
