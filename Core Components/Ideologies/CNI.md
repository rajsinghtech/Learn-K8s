Container Network Interface (CNI) is a critical component in Kubernetes that handles the network connectivity between pods (the smallest deployable units in Kubernetes) and allows them to communicate with each other and with external resources. CNI defines a set of specifications and plugins for network management within Kubernetes clusters. Here, I'll explain CNI in Kubernetes, its purpose, and provide some relevant information and templates where necessary.

**Purpose of CNI in Kubernetes:**
CNI is essential in Kubernetes for the following reasons:

1. **Pod Networking:** Pods are isolated networking environments in Kubernetes. CNI ensures that each pod can communicate with other pods within the cluster and with external services and resources.

2. **IP Address Management:** CNI assigns unique IP addresses to pods, facilitating their identification and communication.

3. **Network Policies:** CNI allows for the implementation of network policies that control the flow of traffic to and from pods, enhancing security within the cluster.

4. **Container-to-Container Communication:** CNI enables communication between containers within the same pod if multiple containers share the same network namespace.

5. **Integration with Underlying Infrastructure:** CNI plugins can integrate with the underlying network infrastructure, such as virtual networks or physical networks, ensuring seamless connectivity for pods.

**CNI Plugins:**
CNI relies on plugins to implement network configuration and management. These plugins are responsible for tasks like IP allocation, routing, and network policy enforcement. Kubernetes users can choose from a variety of CNI plugins to best suit their network requirements.

Here's a basic template that demonstrates the structure of a CNI configuration file:

```yaml
{
  "cniVersion": "0.4.0",
  "name": "my-cni-plugin",
  "type": "plugin-type",
  "someOption": "option-value",
  "anotherOption": "another-value"
}
```

In the template above:

- `"cniVersion"` specifies the CNI version being used.
- `"name"` is the name of the CNI plugin.
- `"type"` indicates the plugin type (e.g., bridge, macvlan, calico, flannel, etc.).
- `"someOption"` and `"anotherOption"` are placeholders for specific configuration options and their values.

**Example CNI Plugins:**

1. **Calico:**
   Calico is a popular CNI plugin that provides network connectivity and advanced network policy enforcement within Kubernetes clusters. It's often used in scenarios where fine-grained network policies are required.

   ```yaml
   {
     "cniVersion": "0.4.0",
     "name": "calico",
     "type": "calico",
     "etcd_endpoints": "http://etcd-cluster-endpoint:2379",
     "log_level": "info",
     "ipam": {
       "type": "calico-ipam"
     }
   }
   ```

2. **Flannel:**
   Flannel is another CNI plugin designed for simple and efficient networking. It's often used in smaller or less complex Kubernetes deployments.

   ```yaml
   {
     "cniVersion": "0.4.0",
     "name": "flannel",
     "type": "flannel",
     "delegate": {
       "isDefaultGateway": true
     }
   }
   ```

Remember that the actual configuration may vary based on your specific network requirements and the CNI plugin you choose.

In summary, CNI is a vital component in Kubernetes that manages pod networking, IP address allocation, network policies, and integration with the underlying infrastructure. Users can select from a variety of CNI plugins to configure their cluster's networking to meet their specific needs. The provided templates illustrate the basic structure of CNI configuration files.