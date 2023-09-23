In Kubernetes, a **Replication Controller** is a resource that ensures a specified number of replica pods are running at all times. It is a part of the core Kubernetes concepts for managing and maintaining the desired number of identical pods. A Replication Controller works by monitoring the current state of the cluster and making adjustments as needed to maintain the desired number of pod replicas. If pods fail or are deleted, the Replication Controller automatically creates new pods to replace them. 
***Can span multiple nodes***
Here is an example of a basic Replication Controller configuration in YAML:
``` YAML
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx
spec:
  replicas: 3
  selector:
    app: nginx
  template:
    metadata:
      name: nginx
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```