# Single Server Cluster Using minikube

## Installation of kubectl on Mac OS

There are two ways of installing kubectl in macos - manually or using the home brew manager.

### Option 1 - Manual Installation of kubectl
- Download the latest stable kubectl binary
```sh
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/darwin/amd64/kubectl
```
- Make the kubectl binary executable
```sh
$ chmod +x ./kubectl
```
- Move the kubectl binary to the PATH
```sh
$ sudo mv ./kubectl /usr/local/bin/kubectl
```

### Option 2 - Using HomeBrew
```sh
$ brew install kubectl
```

To connect to the kubernetes cluster, **kubectl** needs the master node endpoint and the credentials to connect to it. While starting minikube the startup process by default creates a config file inside the **.kube** directory which is inside the user's home directory. This configuration file has all the connection details. By default, the kubectl binary accesses this file to find the Master Node's connection endpoint, along with the credentials. To look at the connection details, we can either see the content of the ~/.kube/config(Linux) file, or run the following command:

```sh
$ kubectl config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority: /Users/nkhare/.minikube/ca.crt
    server: https://192.168.99.100:8443
  name: minikube
contexts:
- context:
    cluster: minikube
    user: minikube
  name: minikube
current-context: minikube
kind: Config
preferences: {}
users:
- name: minikube
  user:
    client-certificate: /Users/nkhare/.minikube/apiserver.crt
    client-key: /Users/nkhare/.minikube/apiserver.key
```

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

### Step 5 - Stop minikube
minikube can be stopped using the **minikube stop** command.
```sh
$ minikube stop
Stopping local Kubernetes cluster...
Machine stopped
```

Once **kubectl** and **minikube** are installed,  we can get information about the minikube cluster with the kubectl cluster-info command:
```sh
$ kubectl cluster-info
Kubernetes master is running at https://192.168.99.100:8443
```
To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.

You can find more details about the kubectl command line options [here](https://kubernetes.io/docs/user-guide/kubectl-overview/)

### Step 6 - Dashboard

The Kubernetes Dashboard provides the user interface for the Kubernetes cluster. To access the Dashboard of Minikube, we can use minikube dashboard, which would open a new tab on our web browser, displaying the Kubernetes dashboard:

```sh
$ minikube dashboard
```

### Using kubectl proxy
Using the kubectl proxy command, kubectl would authenticate with the API Server on the Master Node and would make the dashboard available on http://localhost:8001/ui.

```sh
$ kubectl proxy
```sh
Starting to serve on 127.0.0.1:8001

After running the above command, we can access the dashboard at http://127.0.0.1:8001/ui.

When kubectl proxy is configured, we can send requests to localhost on the proxy port:

```sh
$ curl http://localhost:8001/
{
  "paths": [
    "/api",
    "/api/v1",
    "/apis",
    "/apis/apps",
    ......
    ......
    "/logs",
    "/metrics",
    "/swaggerapi/",
    "/ui/",
    "/version"
  ]
}%
```
With the above curl request, we requested all the API endpoints from the API Server.
