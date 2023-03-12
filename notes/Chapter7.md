# Chapter 7 - Orchestrating with Kubernetes

First and first mostly back to seriousness now. This Remote - SHH plugin for VSCode is on another level. I also wonder if I've over complicated things by not making my dev PC headless. 

## K8s From an Airplane's Perspective

Networking containers must consider ports and access needs. Communication between containers happens with microservices - like what I did with Azure - or running public-facing web servers - also like what I did with Azure. Node affinity would be setting up nodes for specific things, like have the workers on one node. Obvi to to restrict UAM.

## Resources

Pod - made up of one or more containers. Can connect and/or share mounted volume.
ReplicaSet - used to maintain a fixed number of pods. Self-healing.
Deployments - manage pods and replicasets and most widely used for governing apps. Long lived and fault tolerant. 
StatefulSets - Also adds features like managing unique Pod name,s managing Pod creation, and ordering termination. Use this if using PostgresSQL and other stateful apps.
Services - Did this with Azure.
Volumes - aforementioned mounted volumes. Persistent volume if for pod's lifecycle such as Amazon EBS for AWS. 
Secrets - self explanatory. Oh but this is an entire API built just for secrets and should be using RBAC to restrict broad access to the Secret manifests. 
ConfigMaps - Updated and deploy new manifests without redeploy and keep changes up to date.
Namespace - divides k8 cluster into several smaller vclusters. 

## Displaying the Sample telnet-server Application

By the end of this section, you’ll have a Kubernetes Deployment with two Pods (replicas) running the telnet-server application that can be accessed from your local host.

We aren't installing kubectl locally. Though it seems it would save some typing. 


```bash
jasen@bertha DevOps-Desperate(main)$ minikube kubectl cluster-info
    > kubectl.sha256:  64 B / 64 B [-------------------------] 100.00% ? p/s 0s
    > kubectl:  45.80 MiB / 45.80 MiB [------------] 100.00% 55.73 MiB p/s 1.0s
Kubernetes control plane is running at https://192.168.59.101:8443
CoreDNS is running at https://192.168.59.101:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
jasen@bertha DevOps-Desperate(main*)$ 
```

### Reviewing the Manifests