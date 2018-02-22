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
