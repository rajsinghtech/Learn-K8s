A **ReplicaSet** in Kubernetes is a resource object used to ensure a specified number of replica pods are running at all times within a cluster. It is part of Kubernetes' workload management system and is often used to maintain the desired level of availability and scalability for a particular application or service.

ReplicaSets define the following key attributes:

- **Selector**: A label selector that specifies which pods are controlled by the ReplicaSet. The ReplicaSet ensures that the number of pods matching this selector is maintained.

- **Replicas**: The desired number of replica pods to be running. The ReplicaSet continuously monitors the cluster and automatically creates or deletes pods as necessary to meet this desired count.

- **Template**: A template for creating new pods when scaling is required. This template defines the pod's specifications, including container images, resource requests, and labels.

- **Pod Distribution**: ReplicaSets can be used in conjunction with anti-affinity rules and node affinity rules to control how pods are distributed across nodes in the cluster.

ReplicaSets are typically used for stateless applications that can be easily scaled horizontally by adding or removing identical pods. When a ReplicaSet is used, it ensures that the specified number of replicas is maintained even in the face of pod failures or node outages.

Additional notes:
Ran by **Replication Controller** - Spans across multiple nodes

``` YAML
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: frontend
  labels:
    app: guestbook
    tier: frontend
spec:
  # modify replicas according to your case
  replicas: 3
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:
        tier: frontend
    spec:
      containers:
      - name: php-redis
        image: gcr.io/google_samples/gb-frontend:v3
```

[[Pod]]