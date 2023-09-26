#Scheduler 
The `kube-scheduler` is a critical component in a Kubernetes cluster responsible for making decisions about where and when to deploy newly created pods based on resource availability, policies, and constraints. It ensures efficient resource utilization, high availability, and reliability of the cluster.

The key responsibilities of `kube-scheduler` include:

- Selecting an appropriate node for a pod to run on, taking into account factors such as resource requirements, affinity/anti-affinity rules, node constraints, and more.
- Distributing pods evenly across nodes to prevent resource bottlenecks and optimize performance.
- Respecting pod priority and preemption policies to ensure that high-priority workloads get scheduled first.
- Monitoring the cluster for changes in node or pod states and adjusting scheduling decisions as needed.

`kube-scheduler` operates as a separate process in the Kubernetes control plane and can be configured or extended to meet specific cluster requirements.

#    Kubernetes Scheduler

The Kubernetes Scheduler is a critical component in a Kubernetes cluster responsible for assigning pods to nodes based on resource requirements, affinity/anti-affinity rules, and other constraints. It plays a pivotal role in ensuring that workloads are distributed efficiently across the cluster to optimize resource utilization and maintain high availability.

## How Kubernetes Scheduler Works

The Kubernetes Scheduler operates as follows:

1. **Pod Creation**: When a user creates a new pod or a controller (like a Deployment or StatefulSet) creates pods, they specify the desired pod attributes such as resource requests, node affinity, and pod affinity/anti-affinity rules in the pod's YAML configuration.
    
2. **API Server**: The pod specification is submitted to the Kubernetes API server, which stores the information in the cluster's etcd datastore.
    
3. **Scheduler Watch**: The Kubernetes Scheduler constantly watches the API server for newly created pods without assigned nodes.
    
4. **Node Selection**: For each unassigned pod, the scheduler evaluates node candidates in the cluster that meet the pod's resource requirements and satisfy any affinity/anti-affinity rules.
    
5. **Score and Rank**: The scheduler assigns a score to each node candidate based on factors like resource availability, node affinity, and balancing workloads. It ranks the nodes by their scores.
    
6. **Binding**: The scheduler selects the node with the highest score and binds the pod to it, updating the pod's assigned node field in the API server.
    
7. **Pod Placement**: The assigned node then schedules the pod, which includes creating necessary containers and managing the pod's lifecycle.\

# **Configure Multiple Schedulers in Kubernetes**

In Kubernetes, you can configure multiple schedulers to enhance the scheduling capabilities of your cluster. By default, Kubernetes uses the built-in scheduler, which assigns pods to nodes based on resource requirements and constraints. However, you may have specific use cases that require custom scheduling logic or the use of alternative schedulers. This is where configuring multiple schedulers comes into play.

Here's an explanation of how to configure multiple schedulers in Kubernetes, along with YAML templates for reference.

**Step 1: Define Custom Scheduler**

First, you need to define a custom scheduler. This typically involves creating a Pod that runs your custom scheduling logic. Below is a simplified example of a custom scheduler YAML template:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-custom-scheduler
  namespace: kube-system
spec:
  containers:
  - name: custom-scheduler
    image: my-custom-scheduler-image:tag
```

In this example:

- `my-custom-scheduler` is the name of the custom scheduler Pod.
- `my-custom-scheduler-image:tag` should point to a Docker image containing your custom scheduler logic.

**Step 2: Configure the Scheduler Policy**

Next, you need to configure the scheduler policy to use multiple schedulers. This is done by modifying the Kubernetes control plane components. Here's an example of how to modify the `kube-scheduler` configuration:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: kube-scheduler
  namespace: kube-system
  labels:
    component: kube-scheduler
spec:
  template:
    spec:
      containers:
      - command:
        - kube-scheduler
        - --authentication-kubeconfig=/etc/kubernetes/scheduler.conf
        - --authorization-kubeconfig=/etc/kubernetes/scheduler.conf
        - --kubeconfig=/etc/kubernetes/scheduler.conf
        - --config=/etc/kubernetes/my-scheduler-config.yaml  # Add this line
        image: k8s.gcr.io/kube-scheduler:vX.Y.Z  # Adjust the version
        name: kube-scheduler
        volumeMounts:
        - mountPath: /etc/kubernetes/scheduler.conf
          name: kubeconfig
      volumes:
      - hostPath:
          path: /etc/kubernetes/scheduler.conf
        name: kubeconfig
```

In this example:

- The `--config` flag points to a custom scheduler configuration file (`my-scheduler-config.yaml`), which is where you specify the scheduler name and priorities. Here's an example of the contents of `my-scheduler-config.yaml`:

```yaml
apiVersion: kubescheduler.config.k8s.io/v1alpha1
kind: KubeSchedulerConfiguration
profiles:
- schedulerName: my-custom-scheduler
  plugins:
    score:
      disabled:
        - "*"
      enabled:
        - "PodTopologySpread"
    bind:
      disabled:
        - "*"
      enabled:
        - "DefaultBinder"
```

In this `my-scheduler-config.yaml`, we've added a profile for the custom scheduler named `my-custom-scheduler` and specified the enabled plugins for scoring and binding. You can customize this further based on your requirements.

**Step 3: Restart the Scheduler**

After making these changes, you need to restart the `kube-scheduler` component to apply the configuration:

```bash
kubectl rollout restart deployment kube-scheduler -n kube-system
```

This ensures that the new scheduler configuration is loaded.

Now, when you create a Pod, you can specify the schedulerName in the Pod's spec to indicate which scheduler should be responsible for scheduling that Pod. For example:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  schedulerName: my-custom-scheduler
  containers:
  - name: my-container
    image: my-image:tag
```

This Pod will be scheduled using your custom scheduler (`my-custom-scheduler`) instead of the default scheduler.

Keep in mind that this is a simplified example, and you can customize your custom scheduler and its configuration further based on your specific requirements.