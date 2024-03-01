## A Beginner's Guide to Minikube Installation:

Minikube is a lightweight Kubernetes implementation that runs on a single machine, typically used for development, testing, and learning purposes. It's designed to make it easy to set up and manage a Kubernetes cluster locally, allowing developers to experiment with Kubernetes features without needing access to a full-scale cluster.

To install Minikube, which is a tool that allows you to run Kubernetes locally, you can follow these steps:


### Prerequisites: 
- 2 CPUs or more
- 2GB of free memory or more
- 20GB of free disk space
- Docker
- Kubectl
- Container or virtual machine manager, such as: Docker, QEMU, Hyperkit, Hyper-V, KVM, Parallels, Podman, VirtualBox, or VMware Fusion/Workstation



### Install kubectl binary:

**kubectl** is a command-line tool for interacting with Kubernetes clusters. You can install it using a package manager like apt, yum, brew, or by downloading the binary from the official sites or GitHub repository.


```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

chmod +x kubectl

sudo mv kubectl /usr/local/bin/
```


**Note:**

If you do not have root access on the target system, you can still install kubectl to the ~/.local/bin directory:

```
chmod +x kubectl

mkdir -p ~/.local/bin
mv ./kubectl ~/.local/bin/kubectl
```


```
kubectl version --client

    Client Version: v1.29.2
    Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
```


### Install minikube binary:

Download the latest Minikube binary for your operating system from the official sites and move the minikube binary to a directory listed in your system's PATH, such as /usr/local/bin.

```
lscpu | grep Virtualization
```


```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

sudo install minikube-linux-amd64 /usr/local/bin/minikube


or,

sudo cp minikube-linux-amd64 /usr/local/bin/minikube
sudo chmod +x /usr/local/bin/minikube
```


```
minikube version

    minikube version: v1.32.0
    commit: 8220a6eb95f0a4d75f7f2d7b14cef975f050512d
```


### Start Minikube cluster:

To start a Minikube cluster, you can use the "minikube start" command. If you want to start Minikube with specific settings or options, you can pass additional flags. Here's how you can do it:

```
echo $USER
sudo usermod -aG docker $USER
sudo chown -R $USER:$USER /var/run/docker.sock
```


```
minikube start -h
```


```
minikube start

minikube start --nodes=2 -p <cluster_name> --driver=docker

minikube start --nodes=2 -p my-cluster --driver=docker
```


### Verify:
After Minikube starts, you can verify its status by running:

```
minikube status
minikube status -p my-cluster


my-cluster
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured

my-cluster-m02
type: Worker
host: Running
kubelet: Running

```


```
minikube profile list

|------------|-----------|---------|--------------|------|---------|---------|-------|--------|
|  Profile   | VM Driver | Runtime |      IP      | Port | Version | Status  | Nodes | Active |
|------------|-----------|---------|--------------|------|---------|---------|-------|--------|
| my-cluster | docker    | docker  | 192.168.49.2 | 8443 | v1.28.3 | Running |     2 |        |
|------------|-----------|---------|--------------|------|---------|---------|-------|--------|
```


```
docker images

REPOSITORY                    TAG       IMAGE ID       CREATED        SIZE
gcr.io/k8s-minikube/kicbase   v0.0.42   dbc648475405   3 months ago   1.2GB
```


```
docker ps

CONTAINER ID   IMAGE                                 COMMAND                  CREATED          STATUS          PORTS                                                                                                                                  NAMES
6f255670e6fa   gcr.io/k8s-minikube/kicbase:v0.0.42   "/usr/local/bin/entr…"   14 minutes ago   Up 14 minutes   127.0.0.1:32777->22/tcp, 127.0.0.1:32776->2376/tcp, 127.0.0.1:32775->5000/tcp, 127.0.0.1:32774->8443/tcp, 127.0.0.1:32773->32443/tcp   my-cluster-m02
e240313eeb32   gcr.io/k8s-minikube/kicbase:v0.0.42   "/usr/local/bin/entr…"   15 minutes ago   Up 15 minutes   127.0.0.1:32772->22/tcp, 127.0.0.1:32771->2376/tcp, 127.0.0.1:32770->5000/tcp, 127.0.0.1:32769->8443/tcp, 127.0.0.1:32768->32443/tcp   my-cluster
```


```
kubectl get nodes

NAME             STATUS   ROLES           AGE    VERSION
my-cluster       Ready    control-plane   4m5s   v1.28.3
my-cluster-m02   Ready    <none>          3m2s   v1.28.3
```


```
kubectl get pods -A

NAMESPACE              NAME                                         READY   STATUS    RESTARTS      AGE
kube-system            coredns-5dd5756b68-5g2mg                     1/1     Running   2             18m
kube-system            etcd-my-cluster                              1/1     Running   0             19m
kube-system            kindnet-9gx4w                                1/1     Running   0             18m
kube-system            kindnet-9qk5q                                1/1     Running   0             18m
kube-system            kube-apiserver-my-cluster                    1/1     Running   0             19m
kube-system            kube-controller-manager-my-cluster           1/1     Running   0             19m
kube-system            kube-proxy-8scxw                             1/1     Running   0             18m
kube-system            kube-proxy-llcms                             1/1     Running   0             18m
kube-system            kube-scheduler-my-cluster                    1/1     Running   0             19m
kube-system            storage-provisioner                          1/1     Running   1 (18m ago)   18m
```


```
kubectl api-resources
kubectl cluster-info
kubectl config view
kubectl config get-contexts
```


```
minikube addons list

minikube addons enable <addon_name>
```


### Add node to cluster:

Add, remove, or list additional nodes: 

```
minikube node -h
minikube node add -h

minikube node add --worker -p my-cluster

minikube node delete <worker_node_name> -p my-cluster
```


```
minikube node list -p my-cluster

my-cluster      192.168.49.2
my-cluster-m02  192.168.49.3
```


```
minikube node stop my-cluster-m02 -p my-cluster
minikube node start my-cluster-m02 -p my-cluster
```


### Minikube dashboard: 

To open the Kubernetes dashboard with Minikube and open the dashboard in your web browser use the following command:

```
minikube dashboard
minikube dashboard --url

minikube dashboard -p my-cluster
minikube dashboard --url -p my-cluster
```


### Minikube service:
Returns the Kubernetes URL(s) for service(s) in your local cluster.


```
### Create Pod and Service:

kubectl apply -f webapp-wear.yaml
kubectl apply -f webapp-wear-svc.yaml
```


```
kubectl get pod -o wide

NAME          READY   STATUS    RESTARTS   AGE   IP           NODE             NOMINATED NODE   READINESS GATES
webapp-wear   1/1     Running   0          72s   10.244.3.2   my-cluster-m02   <none>           <none>
```


```
kubectl get svc

NAME              TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
kubernetes        ClusterIP   10.96.0.1       <none>        443/TCP          87m
webapp-wear-svc   NodePort    10.111.79.163   <none>        8080:30080/TCP   105s
```


```
minikube ip -p my-cluster
```


```
minikube service list -p my-cluster

|-------------|-----------------|--------------|---------------------------|
|  NAMESPACE  |      NAME       | TARGET PORT  |            URL            |
|-------------|-----------------|--------------|---------------------------|
| default     | kubernetes      | No node port |                           |
| default     | webapp-wear-svc |         8080 | http://192.168.49.2:30080 |
| kube-system | kube-dns        | No node port |                           |
|-------------|-----------------|--------------|---------------------------|
```


```
curl http://192.168.49.2:30080
```


### Delete Cluster: 

To delete a Minikube cluster, you can use the minikube delete command. This command will remove the Minikube cluster and associated resources from your local machine. Here's how to do it:

```
minikube stop
minikube stop -p my-cluster
```


```
minikube delete
minikube delete -p my-cluster
```



### Links:

- [Install Tools](https://kubernetes.io/docs/tasks/tools/)
- [Minikube Install](https://minikube.sigs.k8s.io/docs/start/)
- [kubectl Install](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)
- [Minikube Drivers](https://minikube.sigs.k8s.io/docs/drivers/)
- [Minikube commands](https://minikube.sigs.k8s.io/docs/commands/)


That's it! You now have Minikube installed and running on your system, allowing you to experiment with Kubernetes locally.

