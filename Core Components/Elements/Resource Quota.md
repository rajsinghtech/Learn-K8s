Resource quotas in Kubernetes are a crucial resource management feature that enables cluster administrators to define and enforce limits on the amount of CPU, memory, and other computing resources that can be consumed by workloads running within specific namespaces. 

- **Purpose**: Resource quotas help prevent resource contention, ensure fair resource distribution, and protect the overall stability and performance of the cluster by setting boundaries on resource consumption.

- **Scope**: Resource quotas are applied at the namespace level, meaning you can define different quotas for different namespaces within the same cluster.

- **Components**: Resource quotas are configured using YAML files that specify limits for CPU, memory, and other resource types. These limits can be set for pods, services, replication controllers, and other Kubernetes objects.

- **Metrics**: Kubernetes tracks resource usage for each namespace and compares it to the defined quotas. If a namespace exceeds its quota, Kubernetes may deny resource allocation requests or take other specified actions.

- **Examples**: You can define a resource quota that restricts a namespace to using, for example, a maximum of 4 CPU cores and 8GB of memory across all its pods.

Resource quotas are an essential tool for managing multi-tenant Kubernetes clusters, ensuring that one namespace's workloads do not monopolize cluster resources and impact the performance of other workloads.

```YAML
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-resources
spec:
  hard:
    requests.cpu: "1"
    requests.memory: 1Gi
    limits.cpu: "2"
    limits.memory: 2Gi
    requests.nvidia.com/gpu: 4
```

[[Namespaces]]