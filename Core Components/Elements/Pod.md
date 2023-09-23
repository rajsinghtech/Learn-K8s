A `Pod` in Kubernetes is the smallest deployable unit in the Kubernetes object model. It represents a single instance of a running process in a cluster. A Pod can contain one or more containers that share the same network namespace, storage volumes, and other resources. These containers are typically tightly coupled and work together to perform a specific task or function. Pods are often used to deploy applications, microservices, or other workloads in Kubernetes. They can be scheduled to run on a node in the cluster and are managed by the Kubernetes control plane. 
Here's an example of a simple Pod definition in Kubernetes YAML format:
``` Pod
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
```

- `k get pods -o wide -A` - Get all pods in all namespace with extra columns
- `k get pods NAME_OF_POD -o yaml` - Output config of pod in yaml
- `k run NAME_OF_POD --image=nginx` - Run a pod