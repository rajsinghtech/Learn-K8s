# ETCD
## When to backup and restore?

ETCD Backup
```shell
ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 \
  --cacert=<trusted-ca-file> --cert=<cert-file> --key=<key-file> \
  snapshot save <backup-file-location>
```
ETCD Restore
```shell
ETCDCTL_API=3 etcdctl snapshot restore --data-dir <data-dir-location> snapshotdb
```
# Roles
Solution manifest file to create a CSR as follows:

```yaml
---
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: john-developer
spec:
  signerName: kubernetes.io/kube-apiserver-client
  request: B64 ENCODE THE .csr
  usages:
  - digital signature
  - key encipherment
  - client auth
```

To approve this certificate, run: `kubectl certificate approve john-developer`

Next, create a role `developer` and rolebinding `developer-role-binding`, run the command:

```sh
$ kubectl create role developer --resource=pods --verb=create,list,get,update,delete --namespace=development

$ kubectl create rolebinding developer-role-binding --role=developer --user=john --namespace=development
```

To verify the permission from kubectl utility tool:

```sh
$ kubectl auth can-i update pods --as=john --namespace=development
```

# Service Checking within cluster
To create a pod `test-nslookup`. Test that you are able to look up the service and pod names from within the cluster:

```sh
kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup nginx-resolver-service
kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup nginx-resolver-service > /root/CKA/nginx.svc
```

Get the IP of the `nginx-resolver` pod and replace the dots(.) with hyphon(-) which will be used below.

```sh
kubectl get pod nginx-resolver -o wide
kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never
```