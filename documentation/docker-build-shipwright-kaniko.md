# Building a Container using Shipwright setting up kaniko as build strategy

1. Go into the proper folder

    ```console
    cd ~/gitops-demo/docker-build-shipwright-kaniko
    ```

2. First create a kubernetes secret with docker registry credentials

    ```console
    kubectl create secret docker-registry regcred --docker-server=<your-registry-server> --docker-username=<your-name> --docker-password=<your-pword> --docker-email=<your-email>
    ```

    - Expected Output:

    ```console
    secret/regcred created
    ```

3. Deploy the pod


    ```console
    kubectl create -f build-kaniko.yaml
    ```

    - Expected Output:

    ```console
    build.shipwright.io/kaniko-build created
    ```

4. Test during build process

    ```console
    kubectl get build
    ```

    - Expected output:

    ```console
    NAME           REGISTERED   REASON      BUILDSTRATEGYKIND      BUILDSTRATEGYNAME   CREATIONTIME
    kaniko-build   True         Succeeded   ClusterBuildStrategy   kaniko              38s
    ```

    5. Run the build process

    ```console
    kubectl create -f buildrun.yaml
    ```

    - Expected output:

    ```console
    buildrun.shipwright.io/kaniko-buildrun-lwq2w created
    ```

    6. Verify the status changed

    ```console
    kubectl get all
    ```

    - Expected output:

    ```console
    NAME                                  READY   STATUS      RESTARTS   AGE
    pod/kaniko-buildrun-lwq2w-xpwc9-pod   0/3     Completed   0          28s
    ```

    6. Checking the logs from the containers used by this pod to run the build in multiple steps:

    - Step 1: step-source-default

      ```console
      kubectl logs pod/kaniko-buildrun-lwq2w-xpwc9-pod -c step-source-default
      ```

      - Expected output:

      ```console
      Defaulted container "step-source-default" out of: step-source-default, step-build-and-push, step-results, place-tools (init), working-dir-initializer (init)
      2023/02/21 06:56:08 Info: ssh (/usr/bin/ssh): OpenSSH_8.0p1, OpenSSL 1.1.1k  FIPS 25 Mar 2021
      2023/02/21 06:56:08 Info: git (/usr/bin/git): git version 2.27.0
      2023/02/21 06:56:08 Info: git-lfs (/usr/bin/git-lfs): git-lfs/2.11.0 (GitHub; linux amd64; go 1.14.4)
      2023/02/21 06:56:08 /usr/bin/git clone -h
      2023/02/21 06:56:08 /usr/bin/git submodule -h
      2023/02/21 06:56:08 /usr/bin/git clone --quiet --no-tags --single-branch --depth 1 -- https://github.com/sfl0r3nz05/gitops-demo /workspace/source
      2023/02/21 06:56:08 /usr/bin/git -C /workspace/source submodule update --init --recursive --depth 1
      2023/02/21 06:56:08 /usr/bin/git -C /workspace/source rev-parse --abbrev-ref HEAD
      2023/02/21 06:56:08 Successfully loaded https://github.com/sfl0r3nz05/gitops-demo (master) into /workspace/source
      2023/02/21 06:56:08 /usr/bin/git -C /workspace/source rev-parse --verify HEAD
      2023/02/21 06:56:08 /usr/bin/git -C /workspace/source log -1 --pretty=format:%an
      2023/02/21 06:56:08 /usr/bin/git -C /workspace/source rev-parse --abbrev-ref HEAD
      ```

    - Step 2: step-build-and-push

      ```console
      kubectl logs pod/kaniko-buildrun-lwq2w-xpwc9-pod -c step-build-and-push
      ```

      - Expected output:

      ```console
      INFO[0000] Using dockerignore file: /workspace/source/source-code/.dockerignore 
      INFO[0000] Retrieving image manifest python:3.9.7-alpine3.14 
      INFO[0000] Retrieving image python:3.9.7-alpine3.14 from registry index.docker.io 
      INFO[0000] Built cross stage deps: map[]                
      INFO[0000] Retrieving image manifest python:3.9.7-alpine3.14 
      INFO[0000] Returning cached image manifest              
      INFO[0000] Executing 0 build triggers                   
      INFO[0000] Unpacking rootfs as cmd RUN pip install flask requires it. 
      INFO[0003] RUN pip install flask                        
      INFO[0003] Taking snapshot of full filesystem...        
      INFO[0004] cmd: /bin/sh                                 
      INFO[0004] args: [-c pip install flask]                 
      INFO[0004] Running: [/bin/sh -c pip install flask]      
      Collecting flask
        Downloading Flask-2.2.3-py3-none-any.whl (101 kB)
      Collecting importlib-metadata>=3.6.0
        Downloading importlib_metadata-6.0.0-py3-none-any.whl (21 kB)
      Collecting itsdangerous>=2.0
        Downloading itsdangerous-2.1.2-py3-none-any.whl (15 kB)
      Collecting click>=8.0
        Downloading click-8.1.3-py3-none-any.whl (96 kB)
      Collecting Werkzeug>=2.2.2
        Downloading Werkzeug-2.2.3-py3-none-any.whl (233 kB)
      Collecting Jinja2>=3.0
        Downloading Jinja2-3.1.2-py3-none-any.whl (133 kB)
      Collecting zipp>=0.5
        Downloading zipp-3.14.0-py3-none-any.whl (6.7 kB)
      Collecting MarkupSafe>=2.0
        Downloading MarkupSafe-2.1.2-cp39-cp39-musllinux_1_1_x86_64.whl (29 kB)
      Installing collected packages: zipp, MarkupSafe, Werkzeug, Jinja2, itsdangerous, importlib-metadata, click, flask
      Successfully installed Jinja2-3.1.2 MarkupSafe-2.1.2 Werkzeug-2.2.3 click-8.1.3 flask-2.2.3 importlib-metadata-6.0.0 itsdangerous-2.1.2 zipp-3.14.0
      WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment      instead: https://pip.pypa.io/warnings/venv
      WARNING: You are using pip version 21.2.4; however, version 23.0.1 is available.
      You should consider upgrading via the '/usr/local/bin/python -m pip install --upgrade pip' command.
      INFO[0012] Taking snapshot of full filesystem...        
      INFO[0013] WORKDIR /app                                 
      INFO[0013] cmd: workdir                                 
      INFO[0013] Changed working directory to /app            
      INFO[0013] Creating directory /app                      
      INFO[0013] Taking snapshot of files...                  
      INFO[0013] COPY app.py .                                
      INFO[0013] Taking snapshot of files...                  
      INFO[0013] ENTRYPOINT ["python", "app.py"]              
      INFO[0014] Pushing image to sflorenz05/kaniko-demo-image:latest 
      INFO[0015] Pushed image to 1 destinations 
      ```

7. The image is built and pushed to the registry, and you can check the result:

    ```console
    kubectl get buildruns
    ```

    - Expected output:

    ```console
    NAME                    SUCCEEDED   REASON      STARTTIME   COMPLETIONTIME
    kaniko-buildrun-lwq2w   True        Succeeded   7m48s       7m24s
    ```