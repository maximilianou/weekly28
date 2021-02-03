# weekly28


**Reference:**
https://www.youtube.com/watch?v=X48VuDVv0do

---
## Step 1 - Concepts
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

---
## Step 2 - Instalation
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

---
## Step 3 - Command Line ( like docker )
### Command Line Interface
```
$ minikube kubectl get nodes
NAME       STATUS   ROLES                  AGE   VERSION
minikube   Ready    control-plane,master   46m   v1.20.2
```

```
$ minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
timeToStop: Nonexistent
```

```
$ minikube kubectl version
Client Version: version.Info{Major:"1", Minor:"20", GitVersion:"v1.20.2", GitCommit:"faecb196815e248d3ecfb03c680a4507229c2a56", GitTreeState:"clean", BuildDate:"2021-01-13T13:28:09Z", GoVersion:"go1.15.5", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"20", GitVersion:"v1.20.2", GitCommit:"faecb196815e248d3ecfb03c680a4507229c2a56", GitTreeState:"clean", BuildDate:"2021-01-13T13:20:00Z", GoVersion:"go1.15.5", Compiler:"gc", Platform:"linux/amd64"}
```

```
$ minikube kubectl
kubectl controls the Kubernetes cluster manager.

 Find more information at: https://kubernetes.io/docs/reference/kubectl/overview/

Basic Commands (Beginner):
  create        Create a resource from a file or from stdin.
  expose        Take a replication controller, service, deployment or pod and expose it as a new Kubernetes Service
  run           Run a particular image on the cluster
  set           Set specific features on objects

Basic Commands (Intermediate):
  explain       Documentation of resources
  get           Display one or many resources
  edit          Edit a resource on the server
  delete        Delete resources by filenames, stdin, resources and names, or by resources and label selector

Deploy Commands:
  rollout       Manage the rollout of a resource
  scale         Set a new size for a Deployment, ReplicaSet or Replication Controller
  autoscale     Auto-scale a Deployment, ReplicaSet, or ReplicationController

Cluster Management Commands:
  certificate   Modify certificate resources.
  cluster-info  Display cluster info
  top           Display Resource (CPU/Memory/Storage) usage.
  cordon        Mark node as unschedulable
  uncordon      Mark node as schedulable
  drain         Drain node in preparation for maintenance
  taint         Update the taints on one or more nodes

Troubleshooting and Debugging Commands:
  describe      Show details of a specific resource or group of resources
  logs          Print the logs for a container in a pod
  attach        Attach to a running container
  exec          Execute a command in a container
  port-forward  Forward one or more local ports to a pod
  proxy         Run a proxy to the Kubernetes API server
  cp            Copy files and directories to and from containers.
  auth          Inspect authorization
  debug         Create debugging sessions for troubleshooting workloads and nodes

Advanced Commands:
  diff          Diff live version against would-be applied version
  apply         Apply a configuration to a resource by filename or stdin
  patch         Update field(s) of a resource
  replace       Replace a resource by filename or stdin
  wait          Experimental: Wait for a specific condition on one or many resources.
  kustomize     Build a kustomization target from a directory or a remote url.

Settings Commands:
  label         Update the labels on a resource
  annotate      Update the annotations on a resource
  completion    Output shell completion code for the specified shell (bash or zsh)

Other Commands:
  api-resources Print the supported API resources on the server
  api-versions  Print the supported API versions on the server, in the form of "group/version"
  config        Modify kubeconfig files
  plugin        Provides utilities for interacting with plugins.
  version       Print the client and server version information

Usage:
  kubectl [flags] [options]

Use "kubectl <command> --help" for more information about a given command.
Use "kubectl options" for a list of global command-line options (applies to all commands).
```

### Basic Usage kubectl ( docker like )

```
$ minikube kubectl get nodes
NAME       STATUS   ROLES                  AGE   VERSION
minikube   Ready    control-plane,master   52m   v1.20.2

$ minikube kubectl get pod
No resources found in default namespace.
```

```
$ minikube kubectl get services
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   53m
```

- Create Pods
- Create k8s Components

```
minikube kubectl create 
Error: must specify one of -f and -k

Create a resource from a file or from stdin.

 JSON and YAML formats are accepted.

Examples:
  # Create a pod using the data in pod.json.
  kubectl create -f ./pod.json
  
  # Create a pod based on the JSON passed into stdin.
  cat pod.json | kubectl create -f -
  
  # Edit the data in docker-registry.yaml in JSON then create the resource using the edited data.
  kubectl create -f docker-registry.yaml --edit -o json

Available Commands:
  clusterrole         Create a ClusterRole.
  clusterrolebinding  Create a ClusterRoleBinding for a particular ClusterRole
  configmap           Create a configmap from a local file, directory or literal value
  cronjob             Create a cronjob with the specified name.
  deployment          Create a deployment with the specified name.
  ingress             Create an ingress with the specified name.
  job                 Create a job with the specified name.
  namespace           Create a namespace with the specified name
  poddisruptionbudget Create a pod disruption budget with the specified name.
  priorityclass       Create a priorityclass with the specified name.
  quota               Create a quota with the specified name.
  role                Create a role with single rule.
  rolebinding         Create a RoleBinding for a particular Role or ClusterRole
  secret              Create a secret using specified subcommand
  service             Create a service using specified subcommand.
  serviceaccount      Create a service account with the specified name

Options:
      --allow-missing-template-keys=true: If true, ignore any errors in templates when a field or map key is missing in
the template. Only applies to golang and jsonpath output formats.
      --dry-run='none': Must be "none", "server", or "client". If client strategy, only print the object that would be
sent, without sending it. If server strategy, submit server-side request without persisting the resource.
      --edit=false: Edit the API resource before creating
      --field-manager='kubectl-create': Name of the manager used to track field ownership.
  -f, --filename=[]: Filename, directory, or URL to files to use to create the resource
  -k, --kustomize='': Process the kustomization directory. This flag can't be used together with -f or -R.
  -o, --output='': Output format. One of:
json|yaml|name|go-template|go-template-file|template|templatefile|jsonpath|jsonpath-as-json|jsonpath-file.
      --raw='': Raw URI to POST to the server.  Uses the transport specified by the kubeconfig file.
      --record=false: Record current kubectl command in the resource annotation. If set to false, do not record the
command. If set to true, record the command. If not set, default to updating the existing annotation value only if one
already exists.
  -R, --recursive=false: Process the directory used in -f, --filename recursively. Useful when you want to manage
related manifests organized within the same directory.
      --save-config=false: If true, the configuration of current object will be saved in its annotation. Otherwise, the
annotation will be unchanged. This flag is useful when you want to perform kubectl apply on this object in the future.
  -l, --selector='': Selector (label query) to filter on, supports '=', '==', and '!='.(e.g. -l key1=value1,key2=value2)
      --template='': Template string or path to template file to use when -o=go-template, -o=go-template-file. The
template format is golang templates [http://golang.org/pkg/text/template/#pkg-overview].
      --validate=true: If true, use a schema to validate the input before sending it
      --windows-line-endings=false: Only relevant if --edit=true. Defaults to the line ending native to your platform.

Usage:
  kubectl create -f FILENAME [options]

Use "kubectl <command> --help" for more information about a given command.
Use "kubectl options" for a list of global command-line options (applies to all commands).
```

---
## Step 4 - Deployment Create
```
$ minikube kubectl -- create deployment nginx-depl --image=nginx
deployment.apps/nginx-depl created
```

```
$ minikube kubectl -- get deployment
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
nginx-depl   1/1     1            1           63s
```

```
$ minikube kubectl -- get pod
NAME                          READY   STATUS    RESTARTS   AGE
nginx-depl-5c8bf76b5b-gqttj   1/1     Running   0          19h
```

```
$ minikube kubectl -- get replicaset
NAME                    DESIRED   CURRENT   READY   AGE
nginx-depl-5c8bf76b5b   1         1         1       19h
```

---
## Step 5 - Deployment Edit
- Deployment Template Descriptor yml Edit
```
$ minikube kubectl -- edit deployment nginx-depl
```
- Modify yml Deployment Template Descriptor
```
..
  image: nginx:1.16
..
```

```
$ minikube kubectl -- get pod
NAME                          READY   STATUS        RESTARTS   AGE
nginx-depl-5c8bf76b5b-gqttj   0/1     Terminating   0          19h
nginx-depl-7fc44fc5d4-dvwb4   1/1     Running       0          36s
```

```
$ minikube kubectl -- get replicaset
NAME                    DESIRED   CURRENT   READY   AGE
nginx-depl-5c8bf76b5b   1         1         1       19h
nginx-depl-7fc44fc5d4   0         0         0       2m49s
```

---
## Step 6 - Log & Debug
```
$ minikube kubectl -- logs nginx-depl-5c8bf76b5b-2c5t8
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
```
- Create Deployment Template MongoDB
```
$ minikube kubectl -- create deployment mongo-depl --image=mongo
deployment.apps/mongo-depl created
```
- Check MongoDB Pod name and status
```
$ minikube kubectl -- get pod
NAME                          READY   STATUS    RESTARTS   AGE
mongo-depl-5fd6b7d4b4-bjb2z   1/1     Running   0          45s
nginx-depl-5c8bf76b5b-2c5t8   1/1     Running   0          10m
```
- Log Pod MongoDB
```json
$ minikube kubectl -- logs mongo-depl-5fd6b7d4b4-bjb2z
{"t":{"$date":"2021-02-02T17:00:41.703+00:00"},"s":"I",  "c":"CONTROL",  "id":23285,   "ctx":"main","msg":"Automatically disabling TLS 1.0, to force-enable TLS 1.0 specify --sslDisabledProtocols 'none'"}
{"t":{"$date":"2021-02-02T17:00:41.705+00:00"},"s":"W",  "c":"ASIO",     "id":22601,   "ctx":"main","msg":"No TransportLayer configured during NetworkInterface startup"}
{"t":{"$date":"2021-02-02T17:00:41.706+00:00"},"s":"I",  "c":"NETWORK",  "id":4648601, "ctx":"main","msg":"Implicit TCP FastOpen unavailable. If TCP FastOpen is required, set tcpFastOpenServer, tcpFastOpenClient, and tcpFastOpenQueueSize."}
{"t":{"$date":"2021-02-02T17:00:41.706+00:00"},"s":"I",  "c":"STORAGE",  "id":4615611, "ctx":"initandlisten","msg":"MongoDB starting","attr":{"pid":1,"port":27017,"dbPath":"/data/db","architecture":"64-bit","host":"mongo-depl-5fd6b7d4b4-bjb2z"}}
{"t":{"$date":"2021-02-02T17:00:41.706+00:00"},"s":"I",  "c":"CONTROL",  "id":23403,   "ctx":"initandlisten","msg":"Build Info","attr":{"buildInfo":{"version":"4.4.3","gitVersion":"913d6b62acfbb344dde1b116f4161360acd8fd13","openSSLVersion":"OpenSSL 1.1.1  11 Sep 2018","modules":[],"allocator":"tcmalloc","environment":{"distmod":"ubuntu1804","distarch":"x86_64","target_arch":"x86_64"}}}}
{"t":{"$date":"2021-02-02T17:00:41.706+00:00"},"s":"I",  "c":"CONTROL",  "id":51765,   "ctx":"initandlisten","msg":"Operating System","attr":{"os":{"name":"Ubuntu","version":"18.04"}}}
{"t":{"$date":"2021-02-02T17:00:41.706+00:00"},"s":"I",  "c":"CONTROL",  "id":21951,   "ctx":"initandlisten","msg":"Options set by command line","attr":{"options":{"net":{"bindIp":"*"}}}}
{"t":{"$date":"2021-02-02T17:00:41.707+00:00"},"s":"I",  "c":"STORAGE",  "id":22297,   "ctx":"initandlisten","msg":"Using the XFS filesystem is strongly recommended with the WiredTiger storage engine. See http://dochub.mongodb.org/core/prodnotes-filesystem","tags":["startupWarnings"]}
{"t":{"$date":"2021-02-02T17:00:41.707+00:00"},"s":"I",  "c":"STORAGE",  "id":22315,   "ctx":"initandlisten","msg":"Opening WiredTiger","attr":{"config":"create,cache_size=7390M,session_max=33000,eviction=(threads_min=4,threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000,close_scan_interval=10,close_handle_minimum=250),statistics_log=(wait=0),verbose=[recovery_progress,checkpoint_progress,compact_progress],"}}
{"t":{"$date":"2021-02-02T17:00:42.204+00:00"},"s":"I",  "c":"STORAGE",  "id":22430,   "ctx":"initandlisten","msg":"WiredTiger message","attr":{"message":"[1612285242:204409][1:0x7ff29bf9fac0], txn-recover: [WT_VERB_RECOVERY | WT_VERB_RECOVERY_PROGRESS] Set global recovery timestamp: (0, 0)"}}
{"t":{"$date":"2021-02-02T17:00:42.204+00:00"},"s":"I",  "c":"STORAGE",  "id":22430,   "ctx":"initandlisten","msg":"WiredTiger message","attr":{"message":"[1612285242:204468][1:0x7ff29bf9fac0], txn-recover: [WT_VERB_RECOVERY | WT_VERB_RECOVERY_PROGRESS] Set global oldest timestamp: (0, 0)"}}
{"t":{"$date":"2021-02-02T17:00:42.214+00:00"},"s":"I",  "c":"STORAGE",  "id":4795906, "ctx":"initandlisten","msg":"WiredTiger opened","attr":{"durationMillis":507}}
{"t":{"$date":"2021-02-02T17:00:42.214+00:00"},"s":"I",  "c":"RECOVERY", "id":23987,   "ctx":"initandlisten","msg":"WiredTiger recoveryTimestamp","attr":{"recoveryTimestamp":{"$timestamp":{"t":0,"i":0}}}}
{"t":{"$date":"2021-02-02T17:00:42.233+00:00"},"s":"I",  "c":"STORAGE",  "id":4366408, "ctx":"initandlisten","msg":"No table logging settings modifications are required for existing WiredTiger tables","attr":{"loggingEnabled":true}}
{"t":{"$date":"2021-02-02T17:00:42.234+00:00"},"s":"I",  "c":"STORAGE",  "id":22262,   "ctx":"initandlisten","msg":"Timestamp monitor starting"}
{"t":{"$date":"2021-02-02T17:00:42.240+00:00"},"s":"W",  "c":"CONTROL",  "id":22120,   "ctx":"initandlisten","msg":"Access control is not enabled for the database. Read and write access to data and configuration is unrestricted","tags":["startupWarnings"]}
{"t":{"$date":"2021-02-02T17:00:42.240+00:00"},"s":"W",  "c":"CONTROL",  "id":22178,   "ctx":"initandlisten","msg":"/sys/kernel/mm/transparent_hugepage/enabled is 'always'. We suggest setting it to 'never'","tags":["startupWarnings"]}
{"t":{"$date":"2021-02-02T17:00:42.241+00:00"},"s":"I",  "c":"STORAGE",  "id":20320,   "ctx":"initandlisten","msg":"createCollection","attr":{"namespace":"admin.system.version","uuidDisposition":"provided","uuid":{"uuid":{"$uuid":"3b8b6a5e-cb95-47e8-9f1c-17aaa72137a5"}},"options":{"uuid":{"$uuid":"3b8b6a5e-cb95-47e8-9f1c-17aaa72137a5"}}}}
{"t":{"$date":"2021-02-02T17:00:42.253+00:00"},"s":"I",  "c":"INDEX",    "id":20345,   "ctx":"initandlisten","msg":"Index build: done building","attr":{"buildUUID":null,"namespace":"admin.system.version","index":"_id_","commitTimestamp":{"$timestamp":{"t":0,"i":0}}}}
{"t":{"$date":"2021-02-02T17:00:42.253+00:00"},"s":"I",  "c":"COMMAND",  "id":20459,   "ctx":"initandlisten","msg":"Setting featureCompatibilityVersion","attr":{"newVersion":"4.4"}}
{"t":{"$date":"2021-02-02T17:00:42.254+00:00"},"s":"I",  "c":"STORAGE",  "id":20536,   "ctx":"initandlisten","msg":"Flow Control is enabled on this deployment"}
{"t":{"$date":"2021-02-02T17:00:42.254+00:00"},"s":"I",  "c":"STORAGE",  "id":20320,   "ctx":"initandlisten","msg":"createCollection","attr":{"namespace":"local.startup_log","uuidDisposition":"generated","uuid":{"uuid":{"$uuid":"c49eb237-2564-40ee-a514-102be60e5881"}},"options":{"capped":true,"size":10485760}}}
{"t":{"$date":"2021-02-02T17:00:42.269+00:00"},"s":"I",  "c":"INDEX",    "id":20345,   "ctx":"initandlisten","msg":"Index build: done building","attr":{"buildUUID":null,"namespace":"local.startup_log","index":"_id_","commitTimestamp":{"$timestamp":{"t":0,"i":0}}}}
{"t":{"$date":"2021-02-02T17:00:42.269+00:00"},"s":"I",  "c":"FTDC",     "id":20625,   "ctx":"initandlisten","msg":"Initializing full-time diagnostic data capture","attr":{"dataDirectory":"/data/db/diagnostic.data"}}
{"t":{"$date":"2021-02-02T17:00:42.272+00:00"},"s":"I",  "c":"NETWORK",  "id":23015,   "ctx":"listener","msg":"Listening on","attr":{"address":"/tmp/mongodb-27017.sock"}}
{"t":{"$date":"2021-02-02T17:00:42.272+00:00"},"s":"I",  "c":"CONTROL",  "id":20712,   "ctx":"LogicalSessionCacheReap","msg":"Sessions collection is not set up; waiting until next sessions reap interval","attr":{"error":"NamespaceNotFound: config.system.sessions does not exist"}}
{"t":{"$date":"2021-02-02T17:00:42.272+00:00"},"s":"I",  "c":"NETWORK",  "id":23015,   "ctx":"listener","msg":"Listening on","attr":{"address":"0.0.0.0"}}
{"t":{"$date":"2021-02-02T17:00:42.272+00:00"},"s":"I",  "c":"NETWORK",  "id":23016,   "ctx":"listener","msg":"Waiting for connections","attr":{"port":27017,"ssl":"off"}}
{"t":{"$date":"2021-02-02T17:00:42.272+00:00"},"s":"I",  "c":"STORAGE",  "id":20320,   "ctx":"LogicalSessionCacheRefresh","msg":"createCollection","attr":{"namespace":"config.system.sessions","uuidDisposition":"generated","uuid":{"uuid":{"$uuid":"e5cf0db3-ef66-4a7e-ba5f-98afca58aaff"}},"options":{}}}
{"t":{"$date":"2021-02-02T17:00:42.299+00:00"},"s":"I",  "c":"INDEX",    "id":20345,   "ctx":"LogicalSessionCacheRefresh","msg":"Index build: done building","attr":{"buildUUID":null,"namespace":"config.system.sessions","index":"_id_","commitTimestamp":{"$timestamp":{"t":0,"i":0}}}}
{"t":{"$date":"2021-02-02T17:00:42.299+00:00"},"s":"I",  "c":"INDEX",    "id":20345,   "ctx":"LogicalSessionCacheRefresh","msg":"Index build: done building","attr":{"buildUUID":null,"namespace":"config.system.sessions","index":"lsidTTLIndex","commitTimestamp":{"$timestamp":{"t":0,"i":0}}}}
```

- Check MongoDB Pod name and status
```
$ minikube kubectl -- get pod
NAME                          READY   STATUS    RESTARTS   AGE
mongo-depl-5fd6b7d4b4-bjb2z   1/1     Running   0          45s
nginx-depl-5c8bf76b5b-2c5t8   1/1     Running   0          10m
```
- Describe Pod
```
$ minikube kubectl -- describe pod mongo-depl-5fd6b7d4b4-bjb2z
Name:         mongo-depl-5fd6b7d4b4-bjb2z
Namespace:    default
Priority:     0
Node:         minikube/192.168.49.2
Start Time:   Tue, 02 Feb 2021 14:00:00 -0300
Labels:       app=mongo-depl
              pod-template-hash=5fd6b7d4b4
Annotations:  <none>
Status:       Running
IP:           172.17.0.6
IPs:
  IP:           172.17.0.6
Controlled By:  ReplicaSet/mongo-depl-5fd6b7d4b4
Containers:
  mongo:
    Container ID:   docker://b8b5346ad7e0d4d601903179e7b126f3744642b3708ae52077f473945cca6dba
    Image:          mongo
    Image ID:       docker-pullable://mongo@sha256:88e0308671a06d4ee7da41f87944ba66355b4ee3d57d57caf92f5e1938736abd
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Tue, 02 Feb 2021 14:00:41 -0300
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-cvgxx (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  default-token-cvgxx:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-cvgxx
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                 node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  11m   default-scheduler  Successfully assigned default/mongo-depl-5fd6b7d4b4-bjb2z to minikube
  Normal  Pulling    11m   kubelet            Pulling image "mongo"
  Normal  Pulled     11m   kubelet            Successfully pulled image "mongo" in 38.504740341s
  Normal  Created    11m   kubelet            Created container mongo
  Normal  Started    11m   kubelet            Started container mongo

```

---
## Step 7 - Check Inside Pod ( bin/bash inside container )
- Check Exec inside Pod
```
$ minikube kubectl -- get pod
NAME                          READY   STATUS    RESTARTS   AGE
mongo-depl-5fd6b7d4b4-bjb2z   1/1     Running   0          15m
nginx-depl-5c8bf76b5b-2c5t8   1/1     Running   0          25m
```
```
$ minikube kubectl -- exec -it mongo-depl-5fd6b7d4b4-bjb2z -- bin/bash
root@mongo-depl-5fd6b7d4b4-bjb2z:/# 
```

- View Deployment
```
$ minikube kubectl -- get deployment
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
mongo-depl   1/1     1            1           19m
nginx-depl   1/1     1            1           20h
```

- View Pod
```
$ minikube kubectl -- get pod
NAME                          READY   STATUS    RESTARTS   AGE
mongo-depl-5fd6b7d4b4-bjb2z   1/1     Running   0          20m
nginx-depl-5c8bf76b5b-2c5t8   1/1     Running   0          29m
```

- View Replicaset
```
$ minikube kubectl -- get replicaset
NAME                    DESIRED   CURRENT   READY   AGE
mongo-depl-5fd6b7d4b4   1         1         1       20m
nginx-depl-5c8bf76b5b   1         1         1       20h
nginx-depl-7fc44fc5d4   0         0         0       31m
```

## Step 8 - Deployment Delete
- Delete Deployment
```
$ minikube kubectl -- delete deployment mongo-depl
deployment.apps "mongo-depl" deleted
```

```
$ minikube kubectl -- delete deployment nginx-depl
deployment.apps "nginx-depl" deleted
```

```
$ minikube kubectl -- get pod
No resources found in default namespace.
```

```
$ minikube kubectl -- get replicaset
No resources found in default namespace.
```

---
## Step 9 - Deployment Descriptor deployment.yml
### From Command Line to File Descriptor ( docker-compose.yml like )

```
$ minikube kubectl -- apply
error: must specify one of -f and -k
```
### Deployment Descriptor yml
```yml
$ cat nginx-deployment.yml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec: # deployment specification 
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec: # pod specification
      containers:
      - name: nginx
        image: nginx:1.16
        ports:
        - containerPort: 80 
```

```
$ minikube kubectl -- apply -f nginx-deployment.yml 
deployment.apps/nginx-deployment created
```

```
$ minikube kubectl -- get pod
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-644599b9c9-wjspx   1/1     Running   0          72s

$ minikube kubectl -- get deployment
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   1/1     1            1           81s

$ minikube kubectl -- get replicaset
NAME                          DESIRED   CURRENT   READY   AGE
nginx-deployment-644599b9c9   1         1         1       90s
```

- Change nginx-deployment.yml and Apply Changes
```yml
cat nginx-deployment.yml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec: # deployment specification 
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec: # pod specification
      containers:
      - name: nginx
        image: nginx:1.16
        ports:
        - containerPort: 80
```

```
$ minikube kubectl -- apply -f nginx-deployment.yml 
deployment.apps/nginx-deployment configured
```

```
$ minikube kubectl -- get deployment
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   2/2     2            2           3m53s

$ minikube kubectl -- get pod
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-644599b9c9-687g4   1/1     Running   0          39s
nginx-deployment-644599b9c9-wjspx   1/1     Running   0          4m1s

$ minikube kubectl -- get replicaset
NAME                          DESIRED   CURRENT   READY   AGE
nginx-deployment-644599b9c9   2         2         2       4m10s
```
---
## Step 10

### YML Configuration File Kubernetes

- metadata - in the file.yml
- spec - specification - in the file.yml
- status - etcd managment status


**Layers of Abstraction**
- Deployment manages a .. 
- Replicaset manages a .. 
- Pod is an abstraction of ..
- Container

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels: ..
spec:
  replicas: 2
  selector: ..
  template: 
    metadata:        # metadata of a Pod 
      labels:
        app: nginx
    spec:            # bluepring for a Pod
      containers:    # The Containers
      - name: nginx
        image: nginx:1.16
        ports:
        - containersPort: 8080
```

### Connectors - Labels & Selectors

- Deployment
```yml
apiVersion: apps/v1
kind: Deployment
metadata: 
  name: nginx-deployment
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
    spec: ..
```

- Service
```yml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec: 
  selector:
    app: nginx
  ports: ..
```

- Connecting Deployment to Pod

```yml
  labels: 
    app: nginx
```

- Label is matched by the Selector
```yml
  selector:
    matchLabels:
      app: nginx
```
- Connecting Services to Deployments
- Pods belong to that Service

---
## Step 11 - Minimum Configuration of Deployment and Service

- nginx-deployment.yml
```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec: # deployment specification 
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec: # pod specification
      containers:
      - name: nginx
        image: nginx:1.16
        ports:
        - containerPort: 8080
```
- nginx-service.yml
```yml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
```

- Creating Deployment and Services Instances
```
$ minikube kubectl -- apply -f nginx-deployment.yml 
deployment.apps/nginx-deployment configured

$ minikube kubectl -- apply -f nginx-service.yml 
service/nginx-service created
```
- Check Status
```
$ minikube kubectl -- get pod
NAME                               READY   STATUS    RESTARTS   AGE
nginx-deployment-f4b7bbcbc-p2jcc   1/1     Running   0          75s
nginx-deployment-f4b7bbcbc-qwf62   1/1     Running   0          72s

$ minikube kubectl -- get service
NAME            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
kubernetes      ClusterIP   10.96.0.1       <none>        443/TCP   22h
nginx-service   ClusterIP   10.96.108.176   <none>        80/TCP    75s
```

- Inspect service and port
```
$  minikube kubectl -- describe service nginx-service
Name:              nginx-service
Namespace:         default
Labels:            <none>
Annotations:       <none>
Selector:          app=nginx
Type:              ClusterIP
IP Families:       <none>
IP:                10.96.108.176
IPs:               10.96.108.176
Port:              <unset>  80/TCP
TargetPort:        8080/TCP
Endpoints:         172.17.0.7:8080,172.17.0.8:8080
Session Affinity:  None
Events:            <none>
```
- Pod More information -o wide
```
$  minikube kubectl -- get pod -o wide
NAME                               READY   STATUS    RESTARTS   AGE     IP           NODE       NOMINATED NODE   READINESS GATES
nginx-deployment-f4b7bbcbc-p2jcc   1/1     Running   0          5m16s   172.17.0.7   minikube   <none>           <none>
nginx-deployment-f4b7bbcbc-qwf62   1/1     Running   0          5m13s   172.17.0.8   minikube   <none>           <none>
```

### etcd Statuc Inspector
- Status Check ( etcd Status )
```yml
$ minikube kubectl -- get deployment nginx-deployment -o yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "2"
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"apps/v1","kind":"Deployment","metadata":{"annotations":{},"labels":{"app":"nginx"},"name":"nginx-deployment","namespace":"default"},"spec":{"replicas":2,"selector":{"matchLabels":{"app":"nginx"}},"template":{"metadata":{"labels":{"app":"nginx"}},"spec":{"containers":[{"image":"nginx:1.16","name":"nginx","ports":[{"containerPort":8080}]}]}}}}
  creationTimestamp: "2021-02-02T17:35:30Z"
  generation: 3
  labels:
    app: nginx
  managedFields:
  - apiVersion: apps/v1
    fieldsType: FieldsV1
    fieldsV1:
      f:metadata:
        f:annotations:
          .: {}
          f:kubectl.kubernetes.io/last-applied-configuration: {}
        f:labels:
          .: {}
          f:app: {}
      f:spec:
        f:progressDeadlineSeconds: {}
        f:replicas: {}
        f:revisionHistoryLimit: {}
        f:selector: {}
        f:strategy:
          f:rollingUpdate:
            .: {}
            f:maxSurge: {}
            f:maxUnavailable: {}
          f:type: {}
        f:template:
          f:metadata:
            f:labels:
              .: {}
              f:app: {}
          f:spec:
            f:containers:
              k:{"name":"nginx"}:
                .: {}
                f:image: {}
                f:imagePullPolicy: {}
                f:name: {}
                f:ports:
                  .: {}
                  k:{"containerPort":8080,"protocol":"TCP"}:
                    .: {}
                    f:containerPort: {}
                    f:protocol: {}
                f:resources: {}
                f:terminationMessagePath: {}
                f:terminationMessagePolicy: {}
            f:dnsPolicy: {}
            f:restartPolicy: {}
            f:schedulerName: {}
            f:securityContext: {}
            f:terminationGracePeriodSeconds: {}
    manager: kubectl-client-side-apply
    operation: Update
    time: "2021-02-02T18:48:34Z"
  - apiVersion: apps/v1
    fieldsType: FieldsV1
    fieldsV1:
      f:metadata:
        f:annotations:
          f:deployment.kubernetes.io/revision: {}
      f:status:
        f:availableReplicas: {}
        f:conditions:
          .: {}
          k:{"type":"Available"}:
            .: {}
            f:lastTransitionTime: {}
            f:lastUpdateTime: {}
            f:message: {}
            f:reason: {}
            f:status: {}
            f:type: {}
          k:{"type":"Progressing"}:
            .: {}
            f:lastTransitionTime: {}
            f:lastUpdateTime: {}
            f:message: {}
            f:reason: {}
            f:status: {}
            f:type: {}
        f:observedGeneration: {}
        f:readyReplicas: {}
        f:replicas: {}
        f:updatedReplicas: {}
    manager: kube-controller-manager
    operation: Update
    time: "2021-02-02T18:48:39Z"
  name: nginx-deployment
  namespace: default
  resourceVersion: "36562"
  uid: a0b14139-1b82-4b56-9bd1-c6f4c04ed19f
spec:
  progressDeadlineSeconds: 600
  replicas: 2
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: nginx
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx:1.16
        imagePullPolicy: IfNotPresent
        name: nginx
        ports:
        - containerPort: 8080
          protocol: TCP
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status:
  availableReplicas: 2
  conditions:
  - lastTransitionTime: "2021-02-02T17:38:54Z"
    lastUpdateTime: "2021-02-02T17:38:54Z"
    message: Deployment has minimum availability.
    reason: MinimumReplicasAvailable
    status: "True"
    type: Available
  - lastTransitionTime: "2021-02-02T17:35:30Z"
    lastUpdateTime: "2021-02-02T18:48:39Z"
    message: ReplicaSet "nginx-deployment-f4b7bbcbc" has successfully progressed.
    reason: NewReplicaSetAvailable
    status: "True"
    type: Progressing
  observedGeneration: 3
  readyReplicas: 2
  replicas: 2
  updatedReplicas: 2
```

- Status in a nginx-deployent-result.yaml
```
$ minikube kubectl -- get deployment nginx-deployment -o yaml > nginx-deployment-result.yml
```

- Delete Deployment Instance
```
$ minikube kubectl -- delete  -f nginx-deployment.yml 
deployment.apps "nginx-deployment" deleted

$ minikube kubectl -- delete -f nginx-service.yml 
service "nginx-service" deleted
```

---
## Step 12 - Deploy Front and Back in Kubernetes, MongoDB-Express - MongoDB 

- Browser.open( http://localhost:8080/)
- External URL to..
- Front MongoDB-Express
- Internal ConfigMap URL to..
- Back MongoDB

### Here we have the initial cluster kubernet service
```
$ minikube kubectl -- get all
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   23h
```

- Create the mongodb pod
```
$ touch mongo-deployment.yml
```


```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongodb-deployment
  labels:
    app: mongodb
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongodb
  template:
    metadata:
      labels:
        app: mongodb
    spec:
      containers:
      - name: mongodb
        image: mongo

```

https://hub.docker.com/_/mongo

- 27017

```
$ cat local_minikube.sh 
alias kubectl='minikube kubectl -- $@ '

$ . ./local_minikube.sh 

$ kubectl get pod
No resources found in default namespace.
```





