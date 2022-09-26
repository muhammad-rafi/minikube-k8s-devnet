##### Manage Kubernestes resource via config file

There are four main attributes in any Kubernetes configuration yaml file. 

```yaml
apiVersion:
Kind:
metadata:
spec:
```

- apiVersion: Specifies the API version of the Kubernetes resource object. It varies between Kubernetes resource.

- Kind: Kind describes the type of the object/resource to be created, i.e. pods, deployments, services etc.

- metadata: Uniquely identify a Kubernetes object. i.e. name, labels, etc.

- spec: Defines specification of the object and its attributes varies as per object type. 

Here are example of some config files with minimal configurations.

###### POD Config File
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    team: netdevops
    app: nginx
spec:
  containers:
    - name: nginx-pod
      image: nginx:latest
      ports:
        - containerPort: 80

# kubectl create -f nginx-pod.yaml 
```

###### ReplicaSet Config File 
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replicaset
  labels:
    team: netdevops
    app: nginx-server
  namespace: devnet-namespace
  annotations:
    monitoring: true
    pod: true
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      name: nginx-pod
      labels:
        app: nginx
    spec:
      containers:
        - image: nginx-container
          name: nginx:latest
          ports:
            - containerPort: 80

# create -f nginx-replicaset.yaml
```

###### Deployment Config File 
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    team: netdevops
    app: nginx-server
  namespace: devnet-namespace
  annotations:
    monitoring: true
    pod: true
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      name: nginx-pod
      labels:
        app: nginx
    spec:
      containers:
        - image: nginx-container
          name: nginx:latest
          ports:
            - containerPort: 80

# kubectl create -f nginx-deployment.yaml 
```

###### Service Config File 
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  labels:
    app: nginx
  namespace: devnet-namespace
spec:
  selector:
    app: nginx
  ports:
    - port: 8090 # service running port
      protocol: TCP
      targetPort: 80 # container listening port
  type: ClusterIP # default service type, cannot be accessed outside the cluster

# kubectl create -f nginx-service.yaml 
```
To access this service outside the cluster, we need to do port forwarding 
`$ kubectl port-forwards service/nginx-service 8090:8090 # <host:port:service-port>`


