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

Here are example of some configuraton files

