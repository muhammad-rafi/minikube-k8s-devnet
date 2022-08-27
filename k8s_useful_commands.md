# Minikube & Kubernetes Useful Commands 

Commands to Install the latest version of `minikube` on Ubuntu 20.04 using binary file.
```bash
$ curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
$ sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

Commands to install the stable version of `kubectl` on Ubuntu 20.04 using binary file. 
```bash
$ curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
$ chmod +x kubectl
$ sudo mv kubectl /usr/local/bin/
```

Verify `minikube` and `kubectl` versions commands
```bash
$ minikube version
$ kubectl version
$ kubectl version --short
$ kubectl version -o yaml
$ kubectl version --output=yaml
$ kubectl version --client
$ kubectl version --client --output=yaml 
```

Start `minikube` commands with and without options e.g. with node, driver etc.
```bash
$ minikube start
$ minikube start --driver=docker
$ minikube start --nodes 2 -p mini-cluster --driver=docker
$ minikube start --addons=ingress --cpus=4 --cni=flannel --install-addons=true --kubernetes-version=stable --memory=4g
```

Verify `minikube` status (-p to specify cluster name)
```bash
$ minikube status
$ minikube status -p mini-cluster 
```

Check `minikube` nodes, contexts and cluster information
```bash
$ kubectl get nodes
$ kubectl get nodes -o yaml
$ kubectl describe node
$ kubectl describe node <node_name> | grep Roles
$ minikube node list
$ minikube ip
$ kubectl cluster-info
$ kubectl config get-contexts 
$ kubectl config get-contexts mini-cluster 
```

Check `kubectl` config 
```bash
$ minikube kubectl config view
$ kubectl config view
$ cat .kube/config 
```

