

- InfluxDB -v1.8
- Telegraf - v1.19
- Grafana - v8.1


### Create a new folder for configuration (menifest)

Let's create a new folder and `cd` into it, where we can put all the required Kubernetes configuration files to successfully run the TIG stack for model driven telemetry with Cisco devices.

(main) expert@expert-cws:~$ mkdir mdt-tig-k8s && cd mdt-tig-k8s
(main) expert@expert-cws:~/mdt-tig-k8s$ 


I am creating new namespace 'devnet-namespace' but you can use default namespace as well.

### Create a new namespace 'devnet-namespace' (optional)

```bash
(main) expert@expert-cws:~$ kubectl create namespace devnet-namespace
namespace/devnet-namespace created
(main) expert@expert-cws:~$ 
(main) expert@expert-cws:~$ kubectl get namespace devnet-namespace
NAME               STATUS   AGE
devnet-namespace   Active   30s
(main) expert@expert-cws:~$ 
(main) expert@expert-cws:~$ kubectl get namespaces
NAME                   STATUS   AGE
default                Active   31d
devnet-namespace       Active   40s
kube-node-lease        Active   31d
kube-public            Active   31d
kube-system            Active   31d
kubernetes-dashboard   Active   28d
(main) expert@expert-cws:~$
(main) expert@expert-cws:~$ kubectl config get-contexts
CURRENT   NAME       CLUSTER    AUTHINFO   NAMESPACE
*         minikube   minikube   minikube   default
(main) expert@expert-cws:~$
```

or create via configuration file

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: devnet-namespace
```

As you can see from the above output we have a new 'devnet-namespace' and currently default namespace is a default, for the ease of this toturial, I am going to make 'devnet-namespace' as default namespace, so we don't have to provide '-n devnet-namespace' flag for the command we run. If you are using a default namespace, you can feel free to skip this step.

```bash
(main) expert@expert-cws:~$ kubectl config set-context --current --namespace=devnet-namespace
Context "minikube" modified.
(main) expert@expert-cws:~$
(main) expert@expert-cws:~$ kubectl config get-contexts
CURRENT   NAME       CLUSTER    AUTHINFO   NAMESPACE
*         minikube   minikube   minikube   devnet-namespace
(main) expert@expert-cws:~$ 
```

You can you also use `$ kubectl config vieww` to see the full Kubectl config.

### Create InfluxDB container on Kubernetes

As you know InfluxDB is one of the main componenet in TIG stakc, we will deploy this first, but before we deploy, we need to look at the K8s resources we need to run a InfluxDB container in Kubernestes.

- SecretMap: To store the InfluxDB name, credentials, etc. It keeps the data in base64 encode.
- ConfigMap: 
- Persistent Volume (pv): for InfluxDB
- Persistent Volume Claim (pvc): for InfluxDB
- Deployment:
- Service: 

#### Create InfluxDB SecretMap

```s
kubectl create secret generic influxdb-creds \
  --from-literal=INFLUXDB_DB=cisco_mdt \
  --from-literal=INFLUXDB_ADMIN_USER=root \
  --from-literal=INFLUXDB_ADMIN_PASSWORD=telegraf \
  --from-literal=INFLUXDB_USER=murafi \
  --from-literal=INFLUXDB_USER_PASSWORD=telegraf \
  --from-literal=INFLUXDB_HOST=influxdb  \
  --from-literal=INFLUXDB_HTTP_AUTH_ENABLED=true \
  --from-literal=INFLUXDB_CONFIG_PATH=/etc/influxdb/influxdb.conf \
  --dry-run=client -o yaml > influxdb-secret.yaml
```

You may either apply these commands directly by removing the last line `--dry-run=client -o yaml > influxdb-secret.yaml` or you can create a secret configuration file first, and apply it via `kubectl apply -f influxdb-secretMap.yaml`, here dry-run is the same concept you may find it in some other tools e.g, NSO, Ansible etc. It will only dry run the command but will not create the actual resource.

I like to create configuration files, so I can reuse them for other deployments.

```bash
(main) expert@expert-cws:~/mdt-tig-k8s$ kubectl create secret generic influxdb-creds \
>   --from-literal=INFLUXDB_DB=cisco_mdt \
>   --from-literal=INFLUXDB_ADMIN_USER=root \
>   --from-literal=INFLUXDB_ADMIN_PASSWORD=telegraf \
>   --from-literal=INFLUXDB_USER=murafi \
>   --from-literal=INFLUXDB_USER_PASSWORD=telegraf \
>   --from-literal=INFLUXDB_HOST=influxdb  \
>   --from-literal=INFLUXDB_HTTP_AUTH_ENABLED=true \
>   --from-literal=INFLUXDB_CONFIG_PATH=/etc/influxdb/influxdb.conf \
>   --dry-run=client -o yaml > influxdb-secret.yaml
(main) expert@expert-cws:~/mdt-tig-k8s$ ls -l
total 4
-rw-rw-r-- 1 expert expert 388 Sep 24 19:10 influxdb-secret.yaml
(main) expert@expert-cws:~/mdt-tig-k8s$
```

You can also run `$ kubectl get secrets -o yaml > influxdb-secret.yaml` if you have previously created a secret map without `--dry-run=client -o yaml > influxdb-secret.yaml` and edit the file as needed.

Choose your favourtie editor and check the file contents. 

```yaml
apiVersion: v1
data:
  INFLUXDB_ADMIN_PASSWORD: dGVsZWdyYWY=
  INFLUXDB_ADMIN_USER: cm9vdA==
  INFLUXDB_CONFIG_PATH: L2V0Yy9pbmZsdXhkYi9pbmZsdXhkYi5jb25m
  INFLUXDB_DB: Y2lzY29fbWR0
  INFLUXDB_HOST: aW5mbHV4ZGI=
  INFLUXDB_HTTP_AUTH_ENABLED: dHJ1ZQ==
  INFLUXDB_USER: bXVyYWZp
  INFLUXDB_USER_PASSWORD: dGVsZWdyYWY=
kind: Secret
metadata:
  creationTimestamp: null
  name: influxdb-creds
```

It looks good, however, I like to keep the configuration file in a standard format, so I am going to delete undesired key value pairs e.g, `creationTimestamp: null` and rearrange a file in standard Kubernetes file format. You may have also noticed that, all the values are in base64, so we can change them onto the original form except 'passwords'. I have also added namespace and type parameters too. 

Influxdb SecretMap

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: influxdb-secrets
  namespace: devnet-namespace
type: Opaque
data:
  INFLUXDB_DB: cisco_mdt
  INFLUXDB_HOST: influxdb
  INFLUXDB_HTTP_AUTH_ENABLED: true
  INFLUXDB_ADMIN_USER: root
  INFLUXDB_ADMIN_PASSWORD: dGVsZWdyYWY=
  INFLUXDB_USER: bXVyYWZp
  INFLUXDB_USER_PASSWORD: dGVsZWdyYWY=
  INFLUXDB_CONFIG_PATH: /etc/influxdb/influxdb.conf
```

Base64 Encoding and Decoding 

```bash
(main) expert@expert-cws:~/mdt-tig-k8s$ echo root | base64
cm9vdAo=
(main) expert@expert-cws:~/mdt-tig-k8s$ echo cm9vdAo= | base64 --decode
root
```

```
$ kubectl apply -f influxdb-secretMap.yaml
$ kubectl get secrets
$ kubectl get secrets -o yaml
```

#### Create InfluxDB ConfigMap

Influxdb ConfigMap

```yaml
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: influxdb-config
  namespace: devnet-namespace
data:
  INFLUXDB_DB: influxdb-service
```

#### Create InfluxDB PersistentVolume, PersistentVolumeClaim, StorageClass

Influxdb PersistentVolume

```yaml
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: influxdb-pv
  labels:
    component: influxdb-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: storage/data
```

Influxdb StorageClass

```yaml
---
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: mdt-tig-storage
provisioner: k8s.io/minikube-hostpath
volumeBindMode: Immediate
reclaimPolicy: Delete
```

Note: Storage Class and Persistent Volume are not in the namespace and they live outside the cluster

Influxdb PersistentVolumeClaim

```yaml
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  namespace: devnet-namespace
  name: influxdb-pvc
  labels:
    app: influxdb-pvc
spec:
  accessModes:
    - ReadWriteOnce # ReadWriteMany if you have multiple nodes
  resources:
    requests:
      storage: 5Gi
  selector:
    matchLabels:
      component: influxdb-pv
  storageClassName: "mdt-tig-storage" # If you have storageClass to dynamically create PV
```

If your storageClass doesn't exist, it will fallback to the static PersistenVolume.

#### Create InfluxDB Deployment

Influxdb Deployment

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  namespace: devnet-namespace
  name: influxdb
  labels:
    app: influxdb
spec:
  replicas: 1
  selector:
    matchLabels:
      app: influxdb
  template:
    metadata:
      labels:
        app: influxdb
    spec:
      containers:
        - name: influxdb
          image: docker.io/influxdb:1.8
          imagePullPolicy: IfNotPresent
          ports:
            - containerPort: 8086
              protocol: TCP
          envFrom:
            - secretRef:
                name: influxdb-secrets
          volumeMounts:
            - mountPath: /var/lib/influxdb
              name: influxdb-data
            - mountPath: /etc/influxdb/influxdb.conf
              name: influxdb-config
              subPath: influxdb.conf
              readOnly: true
      volumes:
        - name: influxdb-data
          persistentVolumeClaim:
            claimName: influxdb-pvc
        - name: influxdb-config
          configMap:
            name: influxdb-config
```

#### Create InfluxDB Service

Influxdb Service

```yaml
---
apiVersion: v1  
kind: Service  
metadata:  
  name: influxdb-svc  
spec:  
  selector:  
    app: influxdb  
  ports:  
    - protocol: TCP  
      port: 8086  
      targetPort: 8086
      # nodePort: 32000 # set this if you have LoadBalancer service type
  type: ClusterIP # can be LoadBalancer, if you are running in the cloud
```

```s
kubectl expose deployment influxdb-svc --port=8086 --target-port=8086 --protocol=TCP --type=ClusterIP
```
