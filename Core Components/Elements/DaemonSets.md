## DaemonSets in Kubernetes

DaemonSets are a type of Kubernetes workload resource designed to run a specific pod on every node in a cluster. They are often used for deploying system-level daemons or agents that need to be present on all nodes.

DaemonSets ensure that one pod replica runs on each node in the cluster, automatically scaling as nodes are added or removed.

### YAML Template for a DaemonSet

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: example-daemonset
spec:
  selector:
    matchLabels:
      app: example-app
  template:
    metadata:
      labels:
        app: example-app
    spec:
      containers:
        - name: example-container
          image: example-image:tag
```

In the above YAML template:

- `apiVersion` and `kind` specify the resource type as a DaemonSet.
- `metadata` includes the name for the DaemonSet.
- `spec` contains the specifications for the DaemonSet, including the selector to match nodes based on labels and the pod template.
- `template` defines the pod template for the DaemonSet, including metadata and container specifications.

Remember to replace `example-daemonset`, `example-app`, `example-container`, `example-image:tag` with appropriate names and image information for your specific use case.

[[Pod]]