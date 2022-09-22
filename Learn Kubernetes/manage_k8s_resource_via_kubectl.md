##### Manage Kubernestes resource via Kubectl command line

I am running all these command on default namespace, however you can run these under specific namespace with `ns <namespace-name>`

###### To run a pod via kubectl command

`$ kubectl run my-nginx-pod --image=nginx:latest`

```bash
(main) expert@expert-cws:~$ kubectl run my-nginx-pod --image=nginx:latest
pod/my-nginx-pod created
(main) expert@expert-cws:~$ kubectl get pods
NAME           READY   STATUS              RESTARTS   AGE
my-nginx-pod   0/1     ContainerCreating   0          5s
(main) expert@expert-cws:~$ kubectl delete pods my-nginx-pod
pod "my-nginx-pod" deleted
(main) expert@expert-cws:~$ kubectl get pods
No resources found in default namespace.
(main) expert@expert-cws:~$ 
```

###### To create a deployment via kubectl command
