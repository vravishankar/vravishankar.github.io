# Single Server Cluster Using minikube

## Installation of Minikube on Mac OS

To install minikube on Mac OS you need a hypervisor either **VirtualBox** or **xhyve**. This installation will use the **VirtualBox** hypervisor.

### Step 1 - Install VirtualBox

Install [VirtualBox](http://download.virtualbox.org/virtualbox/5.1.22/VirtualBox-5.1.22-115126-OSX.dmg) on MacOS

### Step 2 - Install minikube

Download minikube [here](https://github.com/kubernetes/minikube/releases). Once downloaded, we need to make it executable and copy it in the PATH.

```sh
$ curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.20.0/minikube-darwin-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```

### Step 3 - Start minikube
Start minikube using the start command - 
**minikube start**

```sh
$ minikube start
Starting local Kubernetes v1.6.4 cluster...
Starting VM...
Downloading Minikube ISO
90.95 MB / 90.95 MB [==============================================] 100.00% 0s
Moving files into cluster...
Setting up certs...
Starting cluster components...
Connecting to cluster...
Setting up kubeconfig...
Kubectl is now configured to use the cluster.
```

### Step 4 - Check minikube status
Check if minikube is running by checking the status using the **minikube status** command.
```sh
$ minikube status
minikube: Running
localkube: Running
kubectl: Correctly Configured: pointing to minikube-vm at 192.168.99.10
```

