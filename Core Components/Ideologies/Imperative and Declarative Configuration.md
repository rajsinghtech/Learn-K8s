### Imperative Configuration
Imperative configuration in Kubernetes involves issuing direct commands to create, modify, or delete resources within the cluster. It focuses on describing the specific steps or actions to be taken. In this approach, administrators use imperative commands like `kubectl run`, `kubectl create`, or `kubectl delete` to make changes to the cluster's state. Imperative commands are similar to procedural instructions and are often used for quick, ad-hoc tasks or in scenarios where fine-grained control is required.

Example of imperative command to create a Deployment:
```SHELL
kubectl create deployment my-app --image=my-image:latest
```
### Declarative Configuration
Declarative configuration, on the other hand, involves providing a desired state for resources in the form of configuration files (usually in YAML or JSON). These configuration files specify what resources should exist and how they should be configured, but they don't prescribe the exact steps to achieve that state. Kubernetes continuously monitors the cluster and automatically reconciles the current state with the declared state. If there are differences, Kubernetes takes actions to bring the cluster into alignment with the desired configuration.

Example of declarative configuration for a Deployment:
```YAML
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  template:
    spec:
      containers:
        - name: my-app-container
          image: my-image:latest
```
In summary, imperative configuration relies on specific commands and steps, while declarative configuration relies on declaring the desired state and letting Kubernetes manage the details of achieving that state. Declarative configuration is the preferred and more widely used approach in Kubernetes, as it promotes consistency, automation, and better version control of your cluster's configuration.