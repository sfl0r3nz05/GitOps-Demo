# Deploy a Service using kuztomize

1. Apply the kustomization file into a running cluster by running the following command:

    ```console
    kubectl apply --dry-run=client -o yaml -k ./
    ```

    > **Consideration:**

    ```console
    --dry-run='none':
        Must be "none", "server", or "client". If client strategy, only print the object that would be sent, without
        sending it. If server strategy, submit server-side request without persisting the resource.
    ```

2. Apply the kustomization file into a running cluster (without --dry-run) by running the following command:

    ```console
    kubectl apply -o yaml -k ./
    ```

3. Service verification:

    ```console
    kubectl get all -n pacman
    ```

    - Expected output:

    ```console
    NAME                               READY   STATUS    RESTARTS   AGE
    pod/pacman-kikd-6958b44cb6-nrjf9   1/1     Running   0          17s

    NAME                  TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
    service/pacman-kikd   ClusterIP   10.100.64.78   <none>        8080/TCP   17s

    NAME                          READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/pacman-kikd   1/1     1            1           17s

    NAME                                     DESIRED   CURRENT   READY   AGE
    replicaset.apps/pacman-kikd-6958b44cb6   1         1         1       17s
    ```  