
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

#### **StatefulSets**
StatefulSets manage the deployment and scaling of a set of Pods, and provide guarantees about the ordering and uniqueness of these Pods. Unlike Deployments, StatefulSets maintain a sticky identity for each of their Pods, which is critical for stateful applications. Each Pod in a StatefulSet has a persistent identifier that it maintains across any rescheduling.

#### **Resource Quota**
Resource Quotas are used to limit the amount of resources a namespace can consume in a Kubernetes cluster. They help ensure fair resource distribution among different teams or applications sharing the same cluster.

- **Commands:**
  - To understand the resource configuration of a pod:  
    ```bash
    kubectl explain pod.spec.containers.resources
    ```
  - Create a resource quota to limit CPU, memory, and the number of pods:
    ```bash
    kubectl create quota my-quota --hard=cpu=1,memory=10G,pods=10 --dry-run=client -o yaml > resource-quota.yaml
    ```
  - Apply a resource quota on a namespace:
    ```bash
    kubectl create -f pod-quota.yaml --namespace=myns1
    ```

#### **Service Account**
Service Accounts provide a way for applications running in Pods to authenticate with the Kubernetes API server. They manage the credentials for applications, ensuring secure access to the cluster's resources.

- **Key Operations:**
  - Check current Kubernetes configuration:
    ```bash
    kubectl config view
    ```
  - Generate a private key and certificate signing request:
    ```bash
    openssl genrsa -out mykey.key 2048
    openssl req -new -key mykey.key -out mycsr.csr -subj "/CN=myuser/O=myorg"
    ```
  - Sign the certificate request:
    ```bash
    openssl x509 -req -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -days 730 -in mycsr.csr -out mycrt.crt
    ```
  - Configure Kubernetes context for the new user:
    ```bash
    kubectl config set-credentials myuser --client-certificate=mycrt.crt --client-key=mykey.key
    kubectl config set-context myuser@myorg --cluster=mycluster --user=myuser --namespace=default
    kubectl config use-context myuser@myorg
    ```
  - Create and apply a service account:
    ```bash
    kubectl create -f serviceaccount.yaml
    kubectl get serviceaccounts -n myns1
    ```

#### **Helm**
Helm is a package manager for Kubernetes that simplifies the deployment and management of applications by using Helm charts. Charts are pre-configured Kubernetes resources that allow easy versioning, upgrading, and rollbacks.

- **Key Operations:**
  - Install Helm:
    ```bash
    curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
    chmod 700 get_helm.sh
    ./get_helm.sh
    ```
  - Create a Helm chart:
    ```bash
    mkdir project1
    cd project1/
    mkdir templates
    vi Chart.yaml
    ```
  - Deploy an application using Helm:
    ```bash
    helm install my-nginx project1
    helm upgrade my-nginx project1
    helm rollback my-nginx
    helm list
    helm history my-nginx
    helm delete my-nginx
    ```

You can explore and use various Helm charts from [Artifact Hub](https://artifacthub.io/). For example, to install Prometheus using Helm, use the following commands:
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install stable prometheus-community/prometheus
```
