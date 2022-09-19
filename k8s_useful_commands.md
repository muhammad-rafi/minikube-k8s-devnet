# Minikube & Kubctl Useful Commands 

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

Check `kubectl` config 
```bash
$ minikube kubectl config view
$ kubectl config view
$ cat .kube/config 
```

Start/Stop `minikube` commands with and without options e.g. with node, driver etc.
```bash
$ minikube start
$ minikube start --driver=docker
$ minikube start --nodes 2 -p minikube--driver=docker
$ minikube start --addons=ingress --cpus=4 --cni=flannel --install-addons=true --kubernetes-version=stable --memory=4g
$ minikube stop
```

Verify `minikube` status (-p to specify cluster name)
```bash
$ minikube status
$ minikube status -p minikube 
```

Check `minikube` nodes, contexts and cluster information
```bash
$ kubectl get nodes -A
$ kubectl get nodes -o yaml
$ kubectl describe node
$ kubectl describe node <node_name> | grep Roles
$ minikube node list
$ minikube ip
$ kubectl cluster-info
$ kubectl config get-contexts 
$ kubectl config get-contexts minikube
```

Add or delete worker node to your `minikube` cluster
```bash
$ minikube node add --worker -p <cluster-name>
$ minikube node add --worker -p minikube
$ minikube node delete <node_name> -p minikube
$ minikube node delete minikube-m02 -p minikube
```

Enable the dashbaord 
```bash
$ minikube dashboard 
$ minikube dashboard --url -p minikube 
```

Check what API resource you can get via `kubectl` command
```bash
$ kubectl auth can-i get deployments
$ kubectl auth can-i get namespaces
$ kubectl auth can-i get pods
$ kubectl auth can-i get services
$ kubectl auth can-i get secrets
$ kubectl auth can-i get PersistentVolumeClaim
```

Other `kubectl` miscellaneous for api resources, componentstatus, events, logs etc.
```bash
$ kubectl api-resources
$ kubectl get componentstatus
$ kubectl get events
$ kubectl get namespaces
$ kubectl logs -h
$ kubectl describe -h
$ kubectl create -h 
$ kubectl run -h
```

kubectl PODs commands 
```bash
$ kubectl get po -A
$ kubectl get pods 
$ kubectl describe pods
$ kubectl get pod --namespace=kube-system
$ kubectl get pods -o wide --all-namespaces --watch
```

kubectl Deployments commands

kubectl Services commands


