
## Kubernetes - Day 4

### PersistentVolume (PV)
A PersistentVolume (PV) in Kubernetes is a piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using Storage Classes. It abstracts the underlying storage and provides a persistent storage solution for Kubernetes applications. PVs exist independently of the lifecycle of Pods and can be used to retain data even if the Pod is deleted or recreated.

To create a PersistentVolume:
```bash
kubectl create -f PersistentVolume.yaml
```
Verify the PersistentVolume creation:
```bash
kubectl get pv
```

### PersistentVolumeClaim (PVC)
A PersistentVolumeClaim (PVC) is a request for storage by a user. It is similar to a Pod in that Pods consume node resources, and PVCs consume PV resources. PVCs are used by developers to request storage resources without needing to know the details of the underlying storage infrastructure. The PVC will be bound to an available PV that meets the requested storage requirements.

To create a PersistentVolumeClaim and attach it to a Pod:
```bash
kubectl create -f Pod-PersistentVolumeClaim.yaml
```
Check the status of your PersistentVolumeClaim:
```bash
kubectl get pvc
```

To deploy the Pod with the PersistentVolumeClaim:
```bash
kubectl create -f Pod-PersistentVolumeClaim.yaml
```

