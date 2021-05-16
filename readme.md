

## Build a K8s cluster on local machine

The yaml file is updated to 
```yaml
apiVersion: kubeadm.k8s.io/v1beta2
kind: ClusterConfiguration
kubernetesVersion: v1.20.0
controllerManager:
  extraArgs:
    cluster-signing-key-file: /home/k8s/keys/ca.key
    bind-address: 0.0.0.0
    deployment-controller-sync-period: "50"
```

Also, 1-CPU Unix machine won't be able to run this

```bash
error execution phase preflight: [preflight] Some fatal errors occurred:
	[ERROR NumCPU]: the number of available CPUs 1 is less than the required 2
```


<details>
  <summary>Complete error log:</summary>
  
```bash
root@ubuntu-s-1vcpu-1gb-nyc1-01:~# kubeadm init --config kubeadm-new.yaml 
[init] Using Kubernetes version: v1.20.0
[preflight] Running pre-flight checks
	[WARNING Service-Docker]: docker service is not enabled, please run 'systemctl enable docker.service'
	[WARNING IsDockerSystemdCheck]: detected "cgroupfs" as the Docker cgroup driver. The recommended driver is "systemd". Please follow the guide at https://kubernetes.io/docs/setup/cri/
	[WARNING FileExisting-socat]: socat not found in system path
	[WARNING Service-Kubelet]: kubelet service is not enabled, please run 'systemctl enable kubelet.service'
error execution phase preflight: [preflight] Some fatal errors occurred:
	[ERROR NumCPU]: the number of available CPUs 1 is less than the required 2
	[ERROR Mem]: the system RAM (985 MB) is less than the minimum 1700 MB
	[ERROR FileExisting-conntrack]: conntrack not found in system path
	[ERROR KubeletVersion]: couldn't get kubelet version: cannot execute 'kubelet --version': executable file not found in $PATH
[preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`
To see the stack trace of this error execute with --v=5 or higher
```

</details>

## kind on MacBook

<details>
   <summary>Master node `kubectl describe node kind`</summary>

```bash
kind get nodes
kind-control-plane
 chloe  ~  kind get clusters
kind
 chloe  ~  kubectl describe node kind-control-plane
Name:               kind-control-plane
Roles:              master
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=kind-control-plane
                    kubernetes.io/os=linux
                    node-role.kubernetes.io/master=
Annotations:        kubeadm.alpha.kubernetes.io/cri-socket: unix:///run/containerd/containerd.sock
                    node.alpha.kubernetes.io/ttl: 0
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Sun, 06 Dec 2020 22:25:55 -0500
Taints:             <none>
Unschedulable:      false
Lease:
  HolderIdentity:  kind-control-plane
  AcquireTime:     <unset>
  RenewTime:       Mon, 03 May 2021 09:57:54 -0400
Conditions:
  Type             Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----             ------  -----------------                 ------------------                ------                       -------
  MemoryPressure   False   Mon, 03 May 2021 09:55:24 -0400   Sun, 18 Apr 2021 23:04:55 -0400   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure     False   Mon, 03 May 2021 09:55:24 -0400   Sun, 18 Apr 2021 23:04:55 -0400   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure      False   Mon, 03 May 2021 09:55:24 -0400   Sun, 18 Apr 2021 23:04:55 -0400   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready            True    Mon, 03 May 2021 09:55:24 -0400   Sun, 18 Apr 2021 23:04:55 -0400   KubeletReady                 kubelet is posting ready status
Addresses:
  InternalIP:  172.21.0.2
  Hostname:    kind-control-plane
Capacity:
  cpu:                4
  ephemeral-storage:  61255492Ki
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             2035400Ki
  pods:               110
Allocatable:
  cpu:                4
  ephemeral-storage:  61255492Ki
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             2035400Ki
  pods:               110
System Info:
  Machine ID:                 e4e97c9d87e944e282450378e9a663cd
  System UUID:                9e3e327c-ce64-4f4d-bb8a-bffcb37c92bf
  Boot ID:                    5fadb9b0-00f1-49b1-b73d-abc0769559c1
  Kernel Version:             5.10.25-linuxkit
  OS Image:                   Ubuntu Groovy Gorilla (development branch)
  Operating System:           linux
  Architecture:               amd64
  Container Runtime Version:  containerd://1.4.0
  Kubelet Version:            v1.19.1
  Kube-Proxy Version:         v1.19.1
PodCIDR:                      10.244.0.0/24
PodCIDRs:                     10.244.0.0/24
ProviderID:                   kind://docker/kind/kind-control-plane
Non-terminated Pods:          (9 in total)
  Namespace                   Name                                          CPU Requests  CPU Limits  Memory Requests  Memory Limits  AGE
  ---------                   ----                                          ------------  ----------  ---------------  -------------  ---
  kube-system                 coredns-f9fd979d6-fkj8x                       100m (2%)     0 (0%)      70Mi (3%)        170Mi (8%)     147d
  kube-system                 coredns-f9fd979d6-w27rh                       100m (2%)     0 (0%)      70Mi (3%)        170Mi (8%)     147d
  kube-system                 etcd-kind-control-plane                       0 (0%)        0 (0%)      0 (0%)           0 (0%)         147d
  kube-system                 kindnet-w8mlw                                 100m (2%)     100m (2%)   50Mi (2%)        50Mi (2%)      147d
  kube-system                 kube-apiserver-kind-control-plane             250m (6%)     0 (0%)      0 (0%)           0 (0%)         147d
  kube-system                 kube-controller-manager-kind-control-plane    200m (5%)     0 (0%)      0 (0%)           0 (0%)         147d
  kube-system                 kube-proxy-5zlv6                              0 (0%)        0 (0%)      0 (0%)           0 (0%)         147d
  kube-system                 kube-scheduler-kind-control-plane             100m (2%)     0 (0%)      0 (0%)           0 (0%)         147d
  local-path-storage          local-path-provisioner-78776bfc44-28vds       0 (0%)        0 (0%)      0 (0%)           0 (0%)         147d
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests    Limits
  --------           --------    ------
  cpu                850m (21%)  100m (2%)
  memory             190Mi (9%)  390Mi (19%)
  ephemeral-storage  0 (0%)      0 (0%)
  hugepages-1Gi      0 (0%)      0 (0%)
  hugepages-2Mi      0 (0%)      0 (0%)
Events:              <none>

```

</details>

### Create Cluster with worker nodes

```sh
kind create cluster --config=*.yaml
```

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
name: local-cluster
# One control plane node and three "workers".
#
# While these will not add more real compute capacity and
# have limited isolation, this can be useful for testing
# rolling updates etc.
#
# The API-server and other control plane components will be
# on the control-plane node.
#
# You probably don't need this unless you are testing Kubernetes itself.
nodes:
- role: control-plane
- role: worker
- role: worker
- role: worker
```

## K8s dashboard UI

[Doc](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/#accessing-the-dashboard-ui) 

```sh
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml
```

[dashboard link](http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/)

```sh
kubectl apply -f ./RBAC/service-account-admin-user.yaml
kubectl apply -f ./RBAC/cluster-role-binding-admin-user.yaml
kubectl -n kubernetes-dashboard get secret $(kubectl -n kubernetes-dashboard get sa/admin-user -o jsonpath="{.secrets[0].name}") -o go-template="{{.data.token | base64decode}}"
# copy the token
kubectl proxy
# enter the token
```

## Volume

### secrets

```sh
$ cat ./username.txt
admin
$ cat ./password.txt
c1oudc0w!

$ kubectl create secret generic user --from-file=./username.txt
$ kubectl create secret generic pass --from-file=./password.txt
```

## Persistent Volume

[create pv and pvc with local-path-storage](https://github.com/rancher/local-path-provisioner)

```sh
$kubectl get pv
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                    STORAGECLASS   REASON   AGE
pvc-403a048d-ca13-4b46-a614-02c828c49353   128Mi      RWO            Delete           Bound    default/local-path-pvc   local-path              6m5s
$kubectl get pvc
NAME             STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
local-path-pvc   Bound    pvc-403a048d-ca13-4b46-a614-02c828c49353   128Mi      RWO            local-path     11m
```

change headless-service/web-statefulset.yaml file line 22-33 with `spec.template.spec.container.volumeMount` and `spec.volumeClaimTemplates`

```sh
# can't change stateful on the fly
$k apply -f headless-service/web-statefulset.yaml 
The StatefulSet "web" is invalid: spec: Forbidden: updates to statefulset spec for fields other than 'replicas', 'template', and 'updateStrategy' are forbidden
```

delete statefulset and create again

```sh
$k get pvc
NAME                   STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
local-path-pvc         Bound    pvc-403a048d-ca13-4b46-a614-02c828c49353   128Mi      RWO            local-path     19m
local-path-pvc-web-0   Bound    pvc-bacfc1d7-0ff6-407d-869c-184438e140e1   50Mi       RWO            standard       7s
local-path-pvc-web-1   Bound    pvc-61cd15ca-61e2-4042-ab78-1f973711f601   50Mi       RWO            standard       4s

$k get pv
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                          STORAGECLASS   REASON   AGE
pvc-403a048d-ca13-4b46-a614-02c828c49353   128Mi      RWO            Delete           Bound    default/local-path-pvc         local-path              15m
pvc-61cd15ca-61e2-4042-ab78-1f973711f601   50Mi       RWO            Delete           Bound    default/local-path-pvc-web-1   standard                82s
pvc-bacfc1d7-0ff6-407d-869c-184438e140e1   50Mi       RWO            Delete           Bound    default/local-path-pvc-web-0   standard                85s

$k get pods
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-85cf96fb5d-65t8l   1/1     Running   0          26h
nginx-deployment-85cf96fb5d-pv5sl   1/1     Running   0          29h
volume-test                         1/1     Running   0          20m
web-0                               1/1     Running   0          111s
web-1                               1/1     Running   0          108s

$for i in 0 1; do kubectl exec web-$i -- sh -c 'echo hello $(hostname) > /usr/share/nginx/html/index.html'; d
one
$k exec -it web-0 -- /bin/sh
# cat /usr/share/nginx/html/index.html  
hello web-0

```

## Service

```sh
k apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.34.1/deploy/static/provider/kind/deploy.yaml
```

## Commands

```sh
# find out current namespace
kubectl config view --minify | grep namespace

# encode string base64
echo 'cGFzc3dvcmQK' | base64
# decode string
echo 'cGFzc3dvcmQK' | base64 -d

# monitoring rolling out status
kubectl rollout status deployment/nginx-deployment

# watch pod creatation `-w`
kubectl get pods -w -l app=<app selector>

```


## Read more
https://github.com/ContainerSolutions/k8s-deployment-strategies

## Nice findings

[how-to-install-git-on-mac-without-xcode](https://superuser.com/questions/322633/how-to-install-git-on-mac-without-xcode)

## demo

### etcd with Kind on localhost

```sh
$ git clone https://github.com/coreos/etcd-operator
$ . example/rbac/create_role.sh 

Creating role with ROLE_NAME=etcd-operator, NAMESPACE=default
Warning: rbac.authorization.k8s.io/v1beta1 ClusterRole is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRole
clusterrole.rbac.authorization.k8s.io/etcd-operator created
Creating role binding with ROLE_NAME=etcd-operator, ROLE_BINDING_NAME=etcd-operator, NAMESPACE=default
Warning: rbac.authorization.k8s.io/v1beta1 ClusterRoleBinding is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRoleBinding
clusterrolebinding.rbac.authorization.k8s.io/etcd-operator created
-bash: POWERLINE_COMMAND_ARGS: unbound variable
Saving session...-bash: HISTTIMEFORMAT: unbound variable

[Process completed]


$ cd ./etcd-operator
$ k create -f ./example/deployment.yaml
$ k get pods
NAME                             READY   STATUS    RESTARTS   AGE
etcd-operator-646cbffdb6-gwrp4   1/1     Running   0          12s
# once pos is running a CRD is created
$ k get crd
NAME                                    CREATED AT
etcdclusters.etcd.database.coreos.com   2021-05-09T20:28:53Z
$ k apply -f ./example/example-etcd-cluster.yaml
$ k get pods
NAME                              READY   STATUS    RESTARTS   AGE
etcd-operator-646cbffdb6-gwrp4    1/1     Running   0          2m17s
example-etcd-cluster-5jbbqn56kf   0/1     Running   0          35s
$ k get pods
k get pods
NAME                              READY   STATUS    RESTARTS   AGE
etcd-operator-646cbffdb6-gwrp4    1/1     Running   0          2m56s
example-etcd-cluster-5jbbqn56kf   1/1     Running   0          74s
example-etcd-cluster-7cvtmc9trc   1/1     Running   0          29s

$ k get pvc data-etcd1-d347a1e3-0 -o yaml > pvc-test.yaml
$ k delete pvc data-etcd1-d347a1e3-0
$ k apply -f etcd-pvc.yaml  # with 10Mi and storageclass:local-path

```

### etcd with IKS via IBM cloud Schematics - IBM Cloud's deployment manager

### etcd with OpenShift
