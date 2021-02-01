# weekly28


**Reference:**
https://www.youtube.com/watch?v=X48VuDVv0do

### Intro to k8s: 
- k8s Architecture
- k8s Components
- minikube and kubectl - local setup
- main kubectl commands - k8s cli
- k8s yaml
- hands on demo

### Advanced Concepts
- k8s namespaces
- k8s ingress
- helm - pkg manager
- k8s volumes
- k8s stateful set components
- k8s services

**Problems Solved: Monolitic to Microservices**

- HA
- Scalability
- Disaster Recovery

Pod: 
- Smallest unit in K8s
- Abstraction over container
- Each Pod gets its own IP address ( internal )
- New ip address on re-creation

Service:
- permanent ip address on each Pod
- lifecycle of Pod and Service are NOT connected
- External Services or Internal Services

Ingress ( like nginx )
- https://myapp.com

ConfigMap:
- External configuration of your app ( DB_URL=..)

Secret:
- Used to store secret data ( encrypted )
- base64 encoded

Volumes:
- local or remote

- Replicate everything connecting to Services
- Service: permanent IP
- Service: load balancer

Deployment: ( StateLESS )
- blueprint for myapp pods
- you create Deployments
- abstraction of Pods

StatefulSet:
- for Stateful Apps ( mongodb, postgres, elasticsearch.. )

Workers:
- Kubelet - interacts with the container and node
- Kubelet - starts the pod
- container runtime
- Kube Proxy

Master Nodes:
- Api Server ( cluster gateway & authentication )
- Scheduler ( decides where to start the instance )
- Controller manager ( detect cluster state changes )
- etcd ( key, value, cluster brain )

### Install Minikube in Local Machine ( linux )
https://minikube.sigs.k8s.io/docs/start/

- 1 
```
root@instrument:~# curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
root@instrument:~# dpkg -i minikube_latest_amd64.deb
```
- 2
```
$ groups
maximilianou adm cdrom floppy audio dip video plugdev netdev bluetooth scanner lpadmin docker
$ minikube start

😄  minikube v1.17.1 on Debian 10.7
✨  Using the docker driver based on existing profile
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
🔥  Creating docker container (CPUs=2, Memory=3900MB) ...
🐳  Preparing Kubernetes v1.20.2 on Docker 20.10.2 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔎  Verifying Kubernetes components...
🌟  Enabled addons: storage-provisioner, default-storageclass
💡  kubectl not found. If you need it, try: 'minikube kubectl -- get pods -A'
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```
- 3
```
$ minikube kubectl -- get po -A
    > kubectl.sha256: 64 B / 64 B [--------------------------] 100.00% ? p/s 0s
    > kubectl: 38.37 MiB / 38.37 MiB [----------------] 100.00% 8.17 MiB p/s 5s
NAMESPACE     NAME                               READY   STATUS    RESTARTS   AGE
kube-system   coredns-74ff55c5b-fjb5f            1/1     Running   0          34m
kube-system   etcd-minikube                      1/1     Running   0          35m
kube-system   kube-apiserver-minikube            1/1     Running   0          35m
kube-system   kube-controller-manager-minikube   1/1     Running   0          35m
kube-system   kube-proxy-lddsm                   1/1     Running   0          34m
kube-system   kube-scheduler-minikube            1/1     Running   0          35m
kube-system   storage-provisioner                1/1     Running   0          35m
```

```
$ minikube dashboard
🔌  Enabling dashboard ...
🤔  Verifying dashboard health ...
🚀  Launching proxy ...
🤔  Verifying proxy health ...
🎉  Opening http://127.0.0.1:36547/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/ in your default browser...
Opening in existing browser session.
```



