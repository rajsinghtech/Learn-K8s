This will update the package lists from the software repository.

```sh
apt update
```

This will install the kubeadm version 1.27.0

```sh
apt-get install kubeadm=1.27.0-00
```

This will upgrade Kubernetes controlplane node.

```sh
kubeadm upgrade apply v1.27.0
```

> Note that the above steps can take a few minutes to complete.

This will update the `kubelet` with the version `1.27.0`.

```sh
apt-get install kubelet=1.27.0-00 
```

You may need to reload the `daemon` and restart `kubelet` service after it has been upgraded.

```sh
systemctl daemon-reload
systemctl restart kubelet
```