Taints and tolerations are concepts in Kubernetes that help control which nodes (worker machines) in a cluster can run specific pods (containers). They are used to influence the scheduling decisions made by the Kubernetes scheduler.

**Taints** are attributes or constraints applied to nodes in a Kubernetes cluster. A node can be tainted with one or more key-value pairs, where the key is a descriptive label, and the value is a specific effect. The three common effects for taints are:

- `NoSchedule`: This effect prevents new pods from being scheduled on nodes with the specified taint.
- `PreferNoSchedule`: This effect advises the scheduler to avoid scheduling pods on tainted nodes, but it is not as strict as `NoSchedule`.
- `NoExecute`: This effect evicts existing pods from nodes that have been tainted, ensuring that only pods with the corresponding tolerations continue to run.

**Tolerations**, on the other hand, are properties set on pods. When a pod has a toleration that matches a node's taint, it can be scheduled on that node, even if the node has been tainted with a constraint that would otherwise prevent it. Tolerations are defined as key-value pairs that mirror the taints on the nodes.

In summary, taints are restrictions applied to nodes, while tolerations are permissions granted to pods. By using taints and tolerations, Kubernetes administrators can control the placement of pods on specific nodes, ensuring that workloads are distributed according to desired constraints and requirements within the cluster.

`k taint nodes NODE_NAME` - 
`kubectl taint nodes node-name key=value:taint-effect` 
`kubectl taint nodes node1 app=myapp:NoSchedule `

NoSchedule | PreferNoSchedule | NoExecute

You specify a toleration for a pod in the PodSpec. Both of the following tolerations "match" the taint created by the `kubectl taint` line above, and thus a pod with either toleration would be able to schedule onto `node1`:

```yaml
tolerations:
- key: "key1"
  operator: "Equal"
  value: "value1"
  effect: "NoSchedule"
```