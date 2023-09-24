In Kubernetes, resource requirements and limits are used to define the computing resources (CPU and memory) that a containerized application or pod needs and the maximum resources it can consume, respectively. These specifications help Kubernetes schedule and manage resources effectively, ensuring fair resource allocation and preventing resource hogging by individual pods. Here's an explanation and example YAML templates for setting resource requirements and limits:

### Resource Requirements

Resource requirements specify the minimum amount of CPU and memory that a pod or container needs to run effectively. These requirements help Kubernetes scheduler make informed decisions about where to place the pod based on available resources in the cluster.

**YAML Template for Resource Requirements:**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: my-image:latest
      resources:
        requests:
          memory: "256Mi"    # Minimum memory required
          cpu: "0.5"         # Minimum CPU required
```

In this example, we specify that the `my-container` pod requires at least 256MB of memory and 0.5 CPU cores to run.

### Resource Limits

Resource limits, on the other hand, define the maximum amount of CPU and memory a pod or container is allowed to use. These limits prevent a misbehaving or resource-intensive pod from affecting the stability and performance of other pods in the same cluster.

**YAML Template for Resource Limits:**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: my-image:latest
      resources:
        limits:
          memory: "512Mi"    # Maximum memory allowed
          cpu: "1"           # Maximum CPU allowed
```

In this example, we set resource limits for the `my-container` pod, allowing it to use a maximum of 512MB of memory and 1 CPU core.

By specifying both resource requirements and limits in your Kubernetes pod or container definitions, you ensure that your applications have the necessary resources to operate efficiently while preventing them from overconsuming resources and negatively impacting other workloads in the cluster.