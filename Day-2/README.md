
## Kubernetes - Day 2

### Docker Command

- **Remove all running containers:**
  ```bash
  docker rm -f $(docker ps -a -q)
  ```

### Kubernetes Architecture

#### Control Plane (Master Node) Components

1. **Kube API Server:**  
   Acts as the front end for the Kubernetes API, managing the container lifecycle and handling client requests.

2. **Kube Scheduler:**  
   Assigns pods to nodes in the cluster, balancing resource utilization and ensuring performance, capacity, and availability.

3. **Kube Controller Manager:**  
   A collection of Kubernetes controllers that watch for state changes and ensure the actual state converges with the desired state.

4. **ETCD:**  
   An open-source distributed key-value store that manages Kubernetes configuration, state, and metadata.

#### Data Plane (Worker Node) Components

1. **Kubelet:**  
   A node agent that monitors application servers and routes administrative requests.

2. **Kube-proxy:**  
   A network proxy that maintains network connectivity between pods and services, acting as a load balancer.

3. **Container Runtime:**  
   Manages container lifecycle, including image management, execution, networking, and storage.

> Note: `kube-proxy` can run on the master node, but master node components do not run on worker nodes.

### Kubernetes Commands

- **Create a Pod using an Imperative Command:**
  ```bash
  kubectl run nginx --image=nginx
  ```

- **Create a Pod Manifest File:**
  ```bash
  kubectl run mypod --image=nginx --dry-run=client -o yaml > pod.yaml
  ```

- **Label a Pod:**
  ```bash
  kubectl label pod nginx env=prod
  kubectl get pod --show-labels
  ```

### Services

- **Expose a Pod as a NodePort Service:**
  ```bash
  kubectl expose pod mypod --type=NodePort --port=80 --target-port=80
  ```

  Access the service using the Node IP and port:
  ```bash
  kubectl get svc -o wide  # Get NodePort and Node IP
  curl <Worker_Node_IP>:<NodePort>
  ```

### Deployments

- **Create a Deployment:**
  ```bash
  kubectl create deployment mydeployment --image=nginx --replicas=3 --dry-run=client -o yaml > mydeployment.yaml
  kubectl create -f mydeployment.yaml
  ```

- **Scale a Deployment:**
  ```bash
  kubectl scale deployment mydeployment --replicas=10
  ```

  To save changes permanently:
  ```bash
  kubectl edit deployment mydeployment  # Adjust replicas as needed
  ```

- **Perform a Rolling Upgrade:**
  ```bash
  kubectl set image deployments/mydeployment nginx=nginx:1.27.2
  kubectl rollout status deployment/mydeployment
  ```

- **Rollback a Rolling Upgrade:**
  ```bash
  kubectl rollout undo deployment/mydeployment
  ```

### Namespaces

- **Create and Manage Namespaces:**
  ```bash
  kubectl create namespace namespace1
  kubectl get namespaces
  kubectl run nginx --image=nginx -n namespace1
  ```

### DaemonSets

A DaemonSet ensures that all (or some) nodes run a copy of a pod. As nodes are added or removed, pods are added or removed accordingly. Typical use cases include running storage daemons, log collection daemons, or node monitoring daemons on every node.

- **Get DaemonSets:**
  ```bash
  kubectl get ds
  ```

