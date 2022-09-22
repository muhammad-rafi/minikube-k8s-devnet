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

Check `minikube` nodes, contexts, profile and cluster information etc.
```bash
$ kubectl get nodes -A
$ kubectl get nodes -o yaml
$ kubectl describe node
$ kubectl describe node <node_name> | grep Roles
$ minikube node list
$ minikube profile list
$ minikube profile
$ minikube service list
$ minikube service --url <service-name>
$ minikube service <service-name> --url
$ minikube ip
$ kubectl cluster-info
$ kubectl config get-contexts 
$ kubectl config get-contexts minikube
```

Add or delete worker node to your `minikube` cluster
```bash
$ minikube node add --worker -p <cluster-name>
$ minikube node add --worker -p minikube
$ minikube node delete <node_name> -p minikube # -p: profile name
$ minikube node delete minikube-m02 -p minikube
```
Enable Add-on
```bash
minikube addons enable dashboard
minikube addons enable metrics-server
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

Other `kubectl` commands
```bash
$ kubectl api-resources
$ kubectl api-versions
$ kubectl run -h
$ kubectl get componentstatus
$ kubectl get secrets
$ kubectl get events
```

Kubectl Logs commands 
```bash
$ kubectl logs <pod_name>
$ kubectl logs --since=1h <pod_name>
$ kubectl logs --tail=20 <pod_name>
$ kubectl logs -f <service_name> [-c <$container>]
$ kubectl logs -c <container_name> <pod_name>
$ kubectl logs <pod_name> pod.log
$ kubectl logs --previous <pod_name>
```

Kubectl namespace commands
```bash
$ kubectl get namespaces
$ kubectl create ns <namespace-name> # namespace-name: my-namespace
$ kubectl describe ns <namespace-name>
$ kubectl edit ns <namespace-name>
$ kubectl delete ns <namespace-name>
$ kubectl get all --all-namespaces
$ kubectl get <resource> --all-namespaces # resource: pods, deployments, services etc.
```

Kubectl PODs commands 
```bash
$ kubectl get po -A
$ kubectl get pods 
$ kubectl edit pods <pod-name>
$ kubectl delete pods <pod-name>
$ kubectl describe pods
$ kubectl get pod --namespace=my_namespace # my_namespace is a custom namespace
$ kubectl get pods -o wide --all-namespaces --watch
```

Kubectl GET Resources 
```bash
$ kubectl get all
$ kubectl get <resource> # resource: pods, deployments, services etc.
$ kubectl get <resource> --namespace=my_namespace # my_namespace is a custom namespace
```

Kubectl Create Resources 
```bash
$ kubectl create <resource> <resource-name> 
# resource: pods, deployments, ,daemonsets, services etc.
# resource-name: name the resource you like to set, e.g. my-pod, my-deplpoyment, etc.
$ kubectl create <resource> <resource-name> --namespace=<namespace-name>
$ kubectl get <resource> --namespace=my_namespace # my_namespace is a custom namespace
```

Kubectl Edit Resources 
```bash
$ kubectl create <resource> <resource-name> 
$ kubectl create <resource> <resource-name> --namespace=<namespace-name>
```

Kubectl Delete Resources 
```bash
$ kubectl delete <resource> <resource-name> 
```

Kubectl Describe Resources 
```bash
$ kubectl describe <resource> <resource-name> 
```

You can also use the short name for the Kubernetes resources, here are the list of resources with full and short names, run `kubectl api-resources` to get the short names.

|   K8s Resources Full Name   |   K8s Resources Full Name  | 
|---------------------------- | -------------------------- | 
| namespaces                  |  ns                        | 
| nodes                       |  no                        | 
| pods                        |  po                        | 
| deployments                 |  deploy                    | 
| services                    |  svc                       | 
| replicasets                 |  rs                        | 
| replicationcontrollers      |  rc                        | 
| persistentvolumeclaims      |  pvc                       | 
| persistentvolumes           |  pv                        | 
| configmaps                  |  cm                        | 
| daemonsets                  |  ds                        | 
| endpoints                   |  ep                        | 
| events                      |  ev                        | 
| ingresses                   |  ing                       | 
| certificatesigningrequests  |  csr                       | 
| componentstatuses           |  cs                        | 
| horizontalpodautoscalers    |  hpa                       | 
| limitranges                 |  limits                    | 
| poddisruptionbudgets        |  pdb                       | 
| podsecuritypolicies         |  psp                       | 
| resourcequotas              |  quota                     | 
| serviceaccounts             |  sa                        | 

### References

[kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

[Kubernetes API](https://kubernetes.io/docs/reference/kubernetes-api/)
