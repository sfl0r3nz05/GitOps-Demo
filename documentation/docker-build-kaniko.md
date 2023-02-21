# Building a Container using kaniko

1. Go into the proper folder

    ```console
    cd ~/gitops-demo/docker-build-kaniko
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
    pod/kaniko created
    ```

4. Test during build process

    ```console
    kubectl logs pod/kaniko
    ```

    - Expected output:

    ```console
    Enumerating objects: 14, done.
    Counting objects: 100% (14/14), done.
    Compressing objects: 100% (11/11), done.
    Total 14 (delta 0), reused 14 (delta 0), pack-reused 0
    INFO[0000] Using dockerignore file: /kaniko/buildcontext/source-code/.dockerignore 
    INFO[0000] Retrieving image manifest python:3.9.7-alpine3.14 
    INFO[0000] Retrieving image python:3.9.7-alpine3.14 from registry index.docker.io 
    INFO[0000] Built cross stage deps: map[]                
    INFO[0000] Retrieving image manifest python:3.9.7-alpine3.14 
    INFO[0000] Returning cached image manifest              
    INFO[0000] Executing 0 build triggers                   
    INFO[0000] Building stage 'python:3.9.7-alpine3.14' [idx: '0', base-idx: '-1'] 
    INFO[0000] Unpacking rootfs as cmd RUN pip install flask requires it. 
    INFO[0002] RUN pip install flask                        
    INFO[0002] Initializing snapshotter ...                 
    INFO[0002] Taking snapshot of full filesystem...        
    INFO[0002] Cmd: /bin/sh                                 
    INFO[0002] Args: [-c pip install flask]                 
    INFO[0002] Running: [/bin/sh -c pip install flask]      
    Collecting flask
      Downloading Flask-2.2.3-py3-none-any.whl (101 kB)
    Collecting itsdangerous>=2.0
      Downloading itsdangerous-2.1.2-py3-none-any.whl (15 kB)
    Collecting click>=8.0
      Downloading click-8.1.3-py3-none-any.whl (96 kB)
    Collecting Werkzeug>=2.2.2
      Downloading Werkzeug-2.2.3-py3-none-any.whl (233 kB)
    Collecting importlib-metadata>=3.6.0
      Downloading importlib_metadata-6.0.0-py3-none-any.whl (21 kB)
    Collecting Jinja2>=3.0
      Downloading Jinja2-3.1.2-py3-none-any.whl (133 kB)
    Collecting zipp>=0.5
      Downloading zipp-3.14.0-py3-none-any.whl (6.7 kB)
    Collecting MarkupSafe>=2.0
      Downloading MarkupSafe-2.1.2-cp39-cp39-musllinux_1_1_x86_64.whl (29 kB)
    Installing collected packages: zipp, MarkupSafe, Werkzeug, Jinja2, itsdangerous, importlib-metadata, click, flask
    Successfully installed Jinja2-3.1.2 MarkupSafe-2.1.2 Werkzeug-2.2.3 click-8.1.3 flask-2.2.3 importlib-metadata-6.0.0 itsdangerous-2.1.2 zipp-3.14.0
    WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment  instead: https://pip.pypa.io/warnings/venv
    WARNING: You are using pip version 21.2.4; however, version 23.0.1 is available.
    You should consider upgrading via the '/usr/local/bin/python -m pip install --upgrade pip' command.
    INFO[0006] Taking snapshot of full filesystem...        
    INFO[0007] WORKDIR /app                                 
    INFO[0007] Cmd: workdir                                 
    INFO[0007] Changed working directory to /app            
    INFO[0007] Creating directory /app                      
    INFO[0007] Taking snapshot of files...                  
    INFO[0007] COPY app.py .                                
    INFO[0007] Taking snapshot of files...                  
    INFO[0007] ENTRYPOINT ["python", "app.py"]              
    INFO[0007] Pushing image to sflorenz05/kaniko-demo-image:latest 
    INFO[0009] Pushed index.docker.io/sflorenz05/kaniko-demo-image@sha256:cc9de49b71b62137e667d86a373c355d4e35afd0571e9482646d3186c4154896
    ```

    5. Pod status change

    ```console
    kubectl get all
    ```

    - Expected output:

    ```console
    NAME         READY   STATUS      RESTARTS   AGE
    pod/kaniko   0/1     Completed   0          2m38s
    ```