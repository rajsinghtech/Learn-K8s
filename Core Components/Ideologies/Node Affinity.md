Node Affinity is a feature in Kubernetes that allows you to influence the scheduling of pods (containers) onto specific nodes in a cluster. It enables you to define rules or preferences for pod placement based on node attributes or labels.

With Node Affinity, you can specify two types of affinity rules:

1. **Node Selector Affinity**: This type allows you to select nodes for pod placement based on labels assigned to nodes. For example, you can instruct Kubernetes to schedule pods only on nodes labeled as "GPU-enabled" or "SSD-storage."
    
2. **Node Anti-Affinity**: Conversely, Node Anti-Affinity lets you express preferences to avoid placing pods on nodes with specific labels. This is useful for spreading workload or ensuring high availability by preventing pods from being scheduled on nodes with similar characteristics.
    
Node Affinity can be a valuable tool for optimizing resource utilization, performance, and reliability in your Kubernetes cluster by controlling where pods are scheduled based on node attributes.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: disktype
            operator: In
            values:
            - ssd            
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent

```

```
requiredDuringSchedulingIgnoredDuringExecution
preferredDuringSchedulingIgnoredDuringExecution
```
`kubectl label nodes <your-node-name> disktype=ssd`

[[kube-scheduler]]