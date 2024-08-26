
## Kubernetes - Day 3

### Jobs
A Job in Kubernetes is responsible for creating one or more Pods and ensuring they successfully complete their tasks. It will continue to retry pod execution until the specified number of successful completions is reached. Once the task is complete, the Job will terminate the Pods it created. Deleting a Job also cleans up the Pods it created, and suspending a Job will delete its active Pods until resumed.

You can use a Job to reliably run a single Pod to completion, and it will automatically start a new Pod if the first one fails or is deleted. Jobs can also be configured to run multiple Pods in parallel.

Example command to create a Job YAML file:
```bash
kubectl create job myjob --image busybox --dry-run=client -o yaml -- sleep 30 > myjob.yaml
```

#### Job with Completions & Parallelism
- **Completions**: Specifies the desired number of successfully finished Pods that the Job should run with. If set to `null`, any successful Pod signals the success of the Job.
- **Parallelism**: Specifies the maximum number of Pods the Job should run concurrently.

### CronJob
A CronJob in Kubernetes creates Jobs on a repeating schedule, similar to cron jobs in Unix systems. It is useful for performing regular tasks like backups or report generation. The schedule is defined in Cron format.

### Cordon & Uncordon Nodes
- To temporarily disable scheduling on a node:
  ```bash
  kubectl cordon <node-name>
  ```
- To re-enable scheduling:
  ```bash
  kubectl uncordon <node-name>
  ```

### Labeling Nodes
Nodes can be labeled to control where Pods are scheduled:
```bash
kubectl label nodes <node-name> <key>=<value>
```

### Affinity
Node affinity allows you to constrain which nodes your Pod can be scheduled on based on node labels. It can be more expressive than `nodeSelector`.

- **requiredDuringSchedulingIgnoredDuringExecution**: The Pod will only be scheduled on nodes that meet the rule.
- **preferredDuringSchedulingIgnoredDuringExecution**: The scheduler prefers nodes that meet the rule but will still schedule the Pod on other nodes if necessary.

Example command to label a node:
```bash
kubectl label nodes <node-name> disktype=ssd
```

### Taints & Tolerations
Taints are applied to nodes, determining whether a Pod can be scheduled on them. Tolerations are applied to Pods, allowing them to be scheduled on nodes with matching taints.

- **NoSchedule**: No new Pods will be scheduled on the node unless they have a matching toleration.
- **PreferNoSchedule**: The scheduler will try to avoid placing a Pod on the node, but it's not guaranteed.
- **NoExecute**: Existing Pods on the node will be evicted, and no new Pods will be scheduled.

To taint a node:
```bash
kubectl taint node <node-name> key=value:NoSchedule
```

### ConfigMaps
ConfigMaps store non-confidential data in key-value pairs, decoupling configuration from container images to make applications portable. They can be consumed by Pods as environment variables, command-line arguments, or files.

Example command to create a ConfigMap from a file:
```bash
kubectl create configmap myconfig --from-file=conf.yaml --dry-run=client -o yaml > my-config.yaml
```

### Secrets
Secrets are used to store confidential data, such as passwords. They can be created from literals or files.

Example command to create a Secret:
```bash
kubectl create secret generic mysecret --from-literal=PASSWD=Admin123
```

To decode a base64 encoded secret:
```bash
echo QWRtaW4xMjM= | base64 -d
```

