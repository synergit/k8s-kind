

## 11 | 从0到1：搭建一个完整的Kubernetes集群

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
kubectl apply -f service-account-admin-user.yaml
kubectl apply -f cluster-role-binding-admin-user.yaml
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

### Commands

```sh
# find out current namespace
kubectl config view --minify | grep namespace

# encode string base64
echo 'cGFzc3dvcmQK' | base64
# decode string
echo 'cGFzc3dvcmQK' | base64 -d
```