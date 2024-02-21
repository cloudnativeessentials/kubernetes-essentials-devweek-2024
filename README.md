# Kubernetes Essentials
### Presented at DevWeek 2024

[YouTube Video of Tutorial Walkthrough (macOS)](https://youtu.be/ertKR2nsSXA) <br/>
[YouTube Video of Tutorial Walkthrough (Linux)](https://youtu.be/Knk9AEZI3bs)

##  Tutorial

- [Prerequisites](#1-prerequisites)
- [minikube Prerequisites](#2-minikube-prerequisites)
- [Docker Install](#3-docker-install)
- [minikube Install](#4-minikube-install)
- [Create a minikube Cluster](#5-create-a-minikube-cluster)
- [Explore the Cluster](#6-explore-the-cluster)
- [Deploy a Containerized Application in a Pod](#7-deploy-a-containerized-application-in-a-pod)
- [Deploy the Containerized Application in a Deployment](#8-deploy-the-containerized-application-in-a-Deployment)
- [Use a Service to Expose the Application](#9-use-a-service-to-expose-the-application)
- [Cleanup](#10-cleanup)

### 1. Prerequisites

Local Kubernetes cluster:
- [minikube](https://minikube.sigs.k8s.io/docs/)
- [kind](https://kind.sigs.k8s.io/)
- [k3s](https://k3s.io/)
- [k3d](https://k3d.io)

### 2. minikube Prerequisites

- 2 CPUs or more
- 2GB of free memory
- 20GB of free disk space
- Internet connection
- Container or virtual machine manager, such as: [Docker](https://docs.docker.com/get-docker/), 
[QEMU](https://minikube.sigs.k8s.io/docs/drivers/qemu/), [Hyperkit](https://minikube.sigs.k8s.io/docs/drivers/hyperkit/), [Hyper-V](https://minikube.sigs.k8s.io/docs/drivers/hyperv/), [KVM](https://minikube.sigs.k8s.io/docs/drivers/kvm2/), [Parallels](https://minikube.sigs.k8s.io/docs/drivers/parallels/), [Podman](https://minikube.sigs.k8s.io/docs/drivers/podman/), [VirtualBox](https://minikube.sigs.k8s.io/docs/drivers/virtualbox/), or [VMware Fusion/Workstation](https://minikube.sigs.k8s.io/docs/drivers/vmware/)


### 3. Docker Install

You can install Docker Desktop via appropriate links on [https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/).

_Note:_ There are multiple ways to install Docker, the following are several ways for this tutorial.

#### 3a. Docker Install on MacOS

via Homebrew
1. `brew install --cask docker`
2. `open /Applications/Docker.app`
3. Proceed with the Docker Desktop prompts and wait till Docker Desktop is running

via MacPorts
1. `port install docker`
2. `open /Applications/Docker.app`
3. Proceed with the Docker Desktop prompts and wait till Docker Desktop is running

#### 3b. Docker Install on Windows

via Chocolately
1. `choco install docker`

via binary
1. [https://docs.docker.com/desktop/install/windows-install/](https://docs.docker.com/desktop/install/windows-install/)

#### 3c. Docker Install on Linux Option 1 - via Install Script

1. Run the Docker installer
`wget -qO- https://get.docker.com/ | sh`

2. Add user to the docker group
`sudo usermod -aG docker $USER`

3. Activate changes to the docker group
`newgrp docker`

4. Verify Docker is running
`docker version`

#### 3d. Docker Install on Linux Option 2 - via Package Manager

1. Add dependencies
`sudo yum install -y yum-utils device-mapper-persistent-data lvm2`

2. Add the Docker repository
`sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo`

3. Update your system
`sudo yum update -y`

4. Use a package manager to install Docker
`sudo yum install docker-ce -y`

5. Start the Docker service and enable it to start on boot
`sudo systemctl start docker && sudo systemctl enable docker`

6. Add user to the docker group
`sudo usermod -aG docker $USER`

7. Activate changes to the docker group
`newgrp docker`

8. Verify Docker is running
`docker version`

### 4. minikube Install 
Go to the minikube installation page to determine the installation route for your environment:
[https://minikube.sigs.k8s.io/docs/start/](https://minikube.sigs.k8s.io/docs/start/)

#### 4a. minikube Install on MacOS

via Homebrew:
1. `brew install minikube`

via Release Binaries for Intel Macs
1. `curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64`
2. `sudo install minikube-darwin-amd64 /usr/local/bin/minikube`

via Release Binaries for Apple Silicon Macs:
1. `curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-arm64`
2. `sudo install minikube-darwin-arm64 /usr/local/bin/minikube`

#### 4b. minikube Install on Windows

via Chocolately:
1. `choco install minikube`

via Windows Package Manager:
1. `winget install minikube`

via PowerShell (as Administrator):
1. `New-Item -Path 'c:\' -Name 'minikube' -ItemType Directory -Force`
2. `Invoke-WebRequest -OutFile 'c:\minikube\minikube.exe' -Uri 'https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe' -UseBasicParsing`
3. `$oldPath = [Environment]::GetEnvironmentVariable('Path', [EnvironmentVariableTarget]::Machine)`
4. `if ($oldPath.Split(';') -inotcontains 'C:\minikube'){`
5.  `[Environment]::SetEnvironmentVariable('Path', $('{0};C:\minikube' -f $oldPath), [EnvironmentVariableTarget]::Machine)`
6. `}`
7. Reopen the terminal

#### 4c. minikube Install on Linux via Release Binaries

For AMD64/x86:
1. `curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64`
2. `sudo install minikube-linux-amd64 /usr/local/bin/minikube`


For ARM64:
1. `curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-arm64`
2. `sudo install minikube-linux-arm64 /usr/local/bin/minikube`


### 5. Create a minikube Cluster

1. Use minikube to create a cluster.

Run:
```shell
minikube start
```

Expected output:
```shell
😄  minikube v1.32.0 on Darwin 13.2 (arm64)
✨  Automatically selected the docker driver. Other choices: vmware, ssh
📌  Using Docker Desktop driver with root privileges
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
💾  Downloading Kubernetes v1.28.3 preload ...
    > preloaded-images-k8s-v18-v1...:  341.16 MiB / 341.16 MiB  100.00% 23.79 M
    > gcr.io/k8s-minikube/kicbase...:  410.58 MiB / 410.58 MiB  100.00% 27.94 M
🔥  Creating docker container (CPUs=2, Memory=7793MB) ...
🐳  Preparing Kubernetes v1.28.3 on Docker 24.0.7 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔗  Configuring bridge CNI (Container Networking Interface) ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

2. Verify the cluster is up with `kubectl` bundled with minikube:

Run:
```shell
minikube kubectl get nodes
```
_NOTE:_ Minikube downloads `kubectl` on the first time using `kubectl` bundled with minikube.

Expected output:
```shell
    > kubectl.sha256:  64 B / 64 B [-------------------------] 100.00% ? p/s 0s
    > kubectl:  52.80 MiB / 52.80 MiB [-------------] 100.00% 6.54 MiB p/s 8.3s


NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   23m   v1.28.3
```


### 6. Explore the Cluster

1. Retrieve the Pods in the default namespace, since a namespace is not specified with the `minikube kubectl get pods` commands, the `default` namespace is used.

Run:
```shell
minikube kubectl get pods
```

Expected output:
```shell
No resources found in default namespace.
```

2. Let's take a look at the existing Namespaces. Retrieve all namespaces

```shell
minikube kubectl -- get namespaces 
```

Expected output:
```shell
default           Active   14m
kube-node-lease   Active   14m
kube-public       Active   14m
kube-system       Active   14m
```

3. Since we've seen the existing Namespaces. Let's get Pods from all namespaces
```shell
minikube kubectl get pods --all-namespaces
```

Expected output:
```shell
Error: unknown flag: --all-namespaces
See 'minikube kubectl --help' for usage.
```
Typically with kubectl, `kubectl get pods --all-namespaces` works to get Pods from all namespaces but with `minikube kubectl` we have to use `minikube kubectl -- get pods --all-namespaces`

Run:
```shell
minikube kubectl -- get pods --all-namespaces         
```

Expected output:
```shell
NAMESPACE     NAME                               READY   STATUS    RESTARTS      AGE
kube-system   coredns-5dd5756b68-x6nx5           1/1     Running   0             32m
kube-system   etcd-minikube                      1/1     Running   0             32m
kube-system   kube-apiserver-minikube            1/1     Running   0             32m
kube-system   kube-controller-manager-minikube   1/1     Running   0             32m
kube-system   kube-proxy-cj8cn                   1/1     Running   0             32m
kube-system   kube-scheduler-minikube            1/1     Running   0             32m
kube-system   storage-provisioner                1/1     Running   1 (31m ago)
```


### 7. Deploy a Containerized Application in a Pod

1. Create a new namespace for our application.

Run:
```shell
minikube kubectl create namespace mynamespace
```

Expected output:
```shell
namespace/mynamespace created
```

2. Imperatively, run a Pod with an `nginx` container using the `nginx` container image with the `1.25.4` tag in the `mynamespace` namespace and expose port 80 of the Pod.

Run:
```shell
minikube kubectl -- run nginx --image nginx:1.25.4 --namespace mynamespace --port 80
```

Expected output:
```shell
pod/nginx created
```

3. Get the virtual IP of the `nginx` Pod.

Run:
```shell
minikube kubectl -- get pods --namespace mynamespace -o wide                                                                                           
```

Expected output:
```shell
NAME    READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          55s   10.244.0.3   minikube   <none>           <none>
```

_NOTE:_ The IP may be different in your environment

4. Check the logs of the nginx Pod

Run:
```shell
 minikube kubectl -- logs nginx --namespace mynamespace
``` 

Expected output:
```shell
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2024/02/20 23:11:01 [notice] 1#1: using the "epoll" event method
2024/02/20 23:11:01 [notice] 1#1: nginx/1.25.4
2024/02/20 23:11:01 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14) 
2024/02/20 23:11:01 [notice] 1#1: OS: Linux 6.6.12-linuxkit
2024/02/20 23:11:01 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2024/02/20 23:11:01 [notice] 1#1: start worker processes
2024/02/20 23:11:01 [notice] 1#1: start worker process 29
...
```

5. Delete the nginx Pod

Run:
```shell
minikube kubectl -- delete pod nginx -n mynamespace
```

Expected output:
```shell
pod "nginx" deleted
```

6. That was the imperative (with a command) way to deploy a Pod. Typically, a declarative (via manifests) way to deploy is preffered.

7. Deploy the nginx Pod in a declarative way. Create a manifest for the nginx Pod.

Run:
```shell
cat <<EOF > pod.yaml
apiVersion: v1 
kind: Pod 
metadata: 
  name: nginx 
  namespace: mynamespace
  labels:
    app: nginx  
spec: 
  containers: 
  - name: nginx 
    image: nginx:1.25.4
    ports: 
    - containerPort: 80
EOF
```

Use `minikube kubectl apply -f` to apply the configuration using the pod.yaml manifest

Run:
```shell
minikube kubectl -- apply -f pod.yaml
```

Expected output:
```shell
pod/nginx created
```

Check the status of the nginx Pod.
Run:
```shell
minikube kubectl -- get pods -n mynamespace -o wide
```

Expected output:
```shell
NAME    READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          98s   10.244.0.5   minikube   <none>           <none>
```

8. Delete the nginx Pod to cleanup


Run:
```shell
minikube kubectl -- delete pod nginx -n mynamespace
```

Expected output:
```shell
pod "nginx" deleted
```

### 8. Deploy the Containerized Application in a Deployment

1. Create a manifest for a Deployment that has 2 replicas of the same Pod using an older version of nginx so that we can update later.

Run:
```shell
cat <<EOF > deployment.yaml
apiVersion: apps/v1 
kind: Deployment 
metadata: 
  name: nginx 
  namespace: mynamespace  
  labels:
    app: nginx
spec: 
  replicas: 2
  selector: 
    matchLabels: 
      app: nginx
  template: 
    metadata: 
      labels: 
        app: nginx 
    spec: 
      containers: 
      - name: nginx 
        image: nginx:1.25.1
        ports: 
        - containerPort: 80
EOF
```

2. Use `minikube kubectl apply` to apply the deploment.yaml manifest to the minikube cluster.

Run:
```shell
minikube kubectl -- apply -f deployment.yaml
```

Expected output:
```shell
deployment.apps/nginx created
```

3. Checkout the resources created

Run:
```shell
minikube kubectl -- get all -n mynamespace
```

Expected output:
```shell
NAME                         READY   STATUS    RESTARTS   AGE
pod/nginx-6476dd4bcf-m842c   1/1     Running   0          51s
pod/nginx-6476dd4bcf-xtp2f   1/1     Running   0          51s

NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   2/2     2            2           51s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-6476dd4bcf   2         2         2       51s
```

Notice the Deployment has created a ReplicaSet and the ReplicaSet has created the Pods.
Let's take a farther look into each resource.

4. Describe the nginx Deployment

Run:
```shell
minikube kubectl -- describe deployment nginx -n mynamespace
```

Expected output:
```shell
Name:                   nginx
Namespace:              mynamespace
CreationTimestamp:      Tue, 20 Feb 2024 18:41:51 -0800
Labels:                 app=nginx
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=nginx
Replicas:               2 desired | 2 updated | 2 total | 2 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=nginx
  Containers:
   nginx:
    Image:        nginx:1.25.1
    Port:         80/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   nginx-6476dd4bcf (2/2 replicas created)
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  2m19s  deployment-controller  Scaled up replica set nginx-6476dd4bcf to 2
```

Review the fields and note the `OldReplicaSets` and the `NewReplicaSet`.

There are no `OldReplicaSets` and the `NewReplicaSet` matches the existing ReplicaSet.

5. Describe the ReplicaSet.

Run:
```shell
minikube kubectl -- describe rs $(minikube kubectl -- get rs -n mynamespace -o jsonpath='{.items[0].metadata.name}') -n mynamespace
```

Expected output:
```shell
Name:           nginx-6476dd4bcf
Namespace:      mynamespace
Selector:       app=nginx,pod-template-hash=6476dd4bcf
Labels:         app=nginx
                pod-template-hash=6476dd4bcf
Annotations:    deployment.kubernetes.io/desired-replicas: 2
                deployment.kubernetes.io/max-replicas: 3
                deployment.kubernetes.io/revision: 1
Controlled By:  Deployment/nginx
Replicas:       2 current / 2 desired
Pods Status:    2 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  app=nginx
           pod-template-hash=6476dd4bcf
  Containers:
   nginx:
    Image:        nginx:1.25.1
    Port:         80/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Events:
  Type    Reason            Age   From                   Message
  ----    ------            ----  ----                   -------
  Normal  SuccessfulCreate  10m   replicaset-controller  Created pod: nginx-6476dd4bcf-xtp2f
  Normal  SuccessfulCreate  10m   replicaset-controller  Created pod: nginx-6476dd4bcf-m842c
```

6. Update the application to use a newer version of nginx. You can update the deployment.yaml manifest but let's use an imperative command.

Run:
```shell
minikube kubectl -- set image deployment/nginx nginx=nginx:1.25.4 -n mynamespace
```

Expected output:
```shell
deployment.apps/nginx image updated
```

Checkout the resources created

Run:
```shell
minikube kubectl -- get all -n mynamespace
```

Expected output:
```shell
NAME                         READY   STATUS              RESTARTS   AGE
pod/nginx-6476dd4bcf-m842c   1/1     Terminating         0          18m
pod/nginx-6476dd4bcf-xtp2f   1/1     Running             0          18m
pod/nginx-7ffd9c9dbb-rxkdb   0/1     ContainerCreating   0          0s
pod/nginx-7ffd9c9dbb-sxqzc   1/1     Running             0          1s

NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   2/2     2            2           18m

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-6476dd4bcf   1         1         1       18m
replicaset.apps/nginx-7ffd9c9dbb   2         2         1       1s
```
_Note:_ If you are fast enough then you will see both ReplicaSets and Pods terminating and creating

7. Let's describe the Deployment again.

Run:
```shell
minikube kubectl -- describe deployment nginx -n mynamespace
```

Expected output:
```shell
Name:                   nginx
Namespace:              mynamespace
CreationTimestamp:      Tue, 20 Feb 2024 18:41:51 -0800
Labels:                 app=nginx
Annotations:            deployment.kubernetes.io/revision: 2
Selector:               app=nginx
Replicas:               2 desired | 2 updated | 2 total | 2 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=nginx
  Containers:
   nginx:
    Image:        nginx:1.25.4
    Port:         80/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  nginx-6476dd4bcf (0/0 replicas created)
NewReplicaSet:   nginx-7ffd9c9dbb (2/2 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  20m   deployment-controller  Scaled up replica set nginx-6476dd4bcf to 2
  Normal  ScalingReplicaSet  98s   deployment-controller  Scaled up replica set nginx-7ffd9c9dbb to 1
  Normal  ScalingReplicaSet  97s   deployment-controller  Scaled down replica set nginx-6476dd4bcf to 1 from 2
  Normal  ScalingReplicaSet  97s   deployment-controller  Scaled up replica set nginx-7ffd9c9dbb to 2 from 1
  Normal  ScalingReplicaSet  96s   deployment-controller  Scaled down replica set nginx-6476dd4bcf to 0 from 1
```

Now take a look at the `OldReplicaSets` and `NewReplicaSet` fields.

7. Let's scale the Deployment to 3 replicas. There are multiple ways to scale a Deployment, you can update the manifest, update imperatively, or use an Autoscaler like the [HorizontalPodAutoscaler](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/)

For this excercise, let's use an imperative command to scale the Deployment to 3 replicas.

Run:
```shell
minikube kubectl -- scale deployment/nginx --replicas=3 -n mynamespace
```

Expected output:
```shell
deployment.apps/nginx scaled
```

8. Check the Pods of the Deployment.

Run:
```shell
minikube kubectl -- get pods -n mynamespace
```

Expected output:
```shell
NAME                     READY   STATUS    RESTARTS   AGE
nginx-7ffd9c9dbb-k97k5   1/1     Running   0          40s
nginx-7ffd9c9dbb-rxkdb   1/1     Running   0          5m36s
nginx-7ffd9c9dbb-sxqzc   1/1     Running   0          5m37s
```

Notice the newest Pod.

9. Scale the number of replicas of the Deployment to 1.

Run:
```shell
minikube kubectl -- scale deployment/nginx --replicas=1 -n mynamespace
```

Expected output:
```shell
deployment.apps/nginx scaled
```


### 9. Use a Service to Expose the Application

1. We can create a Service imperatively with the `minikube kubectl -- expose` command but let's use a declarative way to create a Service to expose the nginx application.
Create the manifest for the Service.

Run:
```shell
cat <<EOF > service.yaml
apiVersion: v1 
kind: Service 
metadata: 
  name: nginx-service
  namespace: mynamespace
spec: 
  selector: 
    app: nginx
  ports: 
  - protocol: TCP 
    port: 8080
    targetPort: 80
  type: NodePort
EOF
```

2. Apply the service.yaml manifest to the minikubecluster.

Run:
```shell
minikube kubectl -- apply -f service.yaml
```

Expected output:
```shell
service/nginx-service created
```

3. Check the nginx-service

Run:
```shell
minikube kubectl -- get service -o wide -n mynamespace
```

Expected output:
```shell
NAME            TYPE       CLUSTER-IP    EXTERNAL-IP   PORT(S)          AGE   SELECTOR
nginx-service   NodePort   10.100.0.73   <none>        8080:30358/TCP   20s   app=nginx
```

4. Let's verify the nginx Pod running is the endpoint for the nginx-service.

Run:
```shell
minikube kubectl -- get pods -n mynamespace -o wide
```

Expected output:
```shell 
NAME                     READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
nginx-7ffd9c9dbb-sxqzc   1/1     Running   0          15m   10.244.0.8   minikube   <none>           <none>
```

Run:
```shell
minikube kubectl -- get endpoints -n mynamespace
```

Expected output:
```shell
NAME            ENDPOINTS       AGE
nginx-service   10.244.0.8:80   83s
```

Note the Pod's IP and the endpoint of the nginx-service.

5. Typically we can reach our Pod on port 30358 of the Node but since this is minikube. Use `minikube kubectl -- port-forward` to forward traffic from localhost port 7080 to the nginx-service.

```shell
minikube kubectl -- port-forward service/nginx-service 7080:8080 -n mynamespace &
```

Expected output:
```shell
Forwarding from [::1]:7080 -> 80
```


Press Ctrl + C to return to the command prompt while port-forwarding runs in the background.

6. Try to reach the application with curl (if present)

Run:
```shell
curl -sk http://localhost:7080 | head -n 4
```

Expected output:
```shell
Handling connection for 7080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
```


### 10. Cleanup

1. Delete all minikube clusters.

Run:
```shell
minikube delete --all
```

Expected output:
```shell
🔥  Deleting "minikube" in docker ...
🔥  Removing /Users/<user>/.minikube/machines/minikube ...
💀  Removed all traces of the "minikube" cluster.
🔥  Successfully deleted all profiles
```

🎉 Thank you for completing this workshop. You have successfully deployed a containerized app in Kubernetes in multiple ways, updated the app, scaled the app up and down, exposed the app to outside the cluster.