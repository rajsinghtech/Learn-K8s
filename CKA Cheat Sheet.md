	This sheet contains all tricky aspects I have found around the CKA.

### mindset
- time is your most valuable resource, speed is your best friend
- be imperative first, declarative second

### `k create|run ... -h`
- copying yaml from docs is LAST RESORT
  - just make the thing with `k create|run`
  - add `... --dry-run=client -o yaml > resource.yaml` if you need to add things before apply
- use the `-h` option
  - tells you EXACTLY what can be created imperatively WITH EXAMPLES
  - menu increases in detail with base command

### understand [the k8s docs](https://kubernetes.io/docs/home/)
- remember important pages and examples
  - mentally bookmark templates that can't be created interactively (pv, pvc, netpol, etc.)
- ctrl-f `kind: <MY RESOURCE>` to quickly find example yaml
- use the [one-pager api reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.24/) for specific details

### use shortnames
- never type out a full resource name if you can help it
  - cm -> configmap
  - pvc -> persistentvolumeclaim
  - ...
- check all shortnames with `k api-resources`

### aliases, functions, and variables
- memorize what you think you'll use

```{bash}
# IMHO must-haves

## create yaml on-the-fly faster
export do='--dry-run=client -o yaml'
```

```{bash}
# nice to haves

## create/destroy from yaml faster
alias kaf='k apply -f '
alias kdf='k delete -f '

## namespaces (poor man's `kubens`)
export nk='-n kube-system'
export n='-n important-ns' # set this as needed

## destroy things without waiting
export now='--grace-period 0 --force'
```
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
## Creating Users
Solution manifest file to create a CSR as follows:

```yaml
---
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: john-developer
spec:
  signerName: kubernetes.io/kube-apiserver-client
  request: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ1ZEQ0NBVHdDQVFBd0R6RU5NQXNHQTFVRUF3d0VhbTlvYmpDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRApnZ0VQQURDQ0FRb0NnZ0VCQUt2Um1tQ0h2ZjBrTHNldlF3aWVKSzcrVVdRck04ZGtkdzkyYUJTdG1uUVNhMGFPCjV3c3cwbVZyNkNjcEJFRmVreHk5NUVydkgyTHhqQTNiSHVsTVVub2ZkUU9rbjYra1NNY2o3TzdWYlBld2k2OEIKa3JoM2prRFNuZGFvV1NPWXBKOFg1WUZ5c2ZvNUpxby82YU92czFGcEc3bm5SMG1JYWpySTlNVVFEdTVncGw4bgpjakY0TG4vQ3NEb3o3QXNadEgwcVpwc0dXYVpURTBKOWNrQmswZWhiV2tMeDJUK3pEYzlmaDVIMjZsSE4zbHM4CktiSlRuSnY3WDFsNndCeTN5WUFUSXRNclpUR28wZ2c1QS9uREZ4SXdHcXNlMTdLZDRaa1k3RDJIZ3R4UytkMEMKMTNBeHNVdzQyWVZ6ZzhkYXJzVGRMZzcxQ2NaanRxdS9YSmlyQmxVQ0F3RUFBYUFBTUEwR0NTcUdTSWIzRFFFQgpDd1VBQTRJQkFRQ1VKTnNMelBKczB2czlGTTVpUzJ0akMyaVYvdXptcmwxTGNUTStsbXpSODNsS09uL0NoMTZlClNLNHplRlFtbGF0c0hCOGZBU2ZhQnRaOUJ2UnVlMUZnbHk1b2VuTk5LaW9FMnc3TUx1a0oyODBWRWFxUjN2SSsKNzRiNnduNkhYclJsYVhaM25VMTFQVTlsT3RBSGxQeDNYVWpCVk5QaGhlUlBmR3p3TTRselZuQW5mNm96bEtxSgpvT3RORStlZ2FYWDdvc3BvZmdWZWVqc25Yd0RjZ05pSFFTbDgzSkljUCtjOVBHMDJtNyt0NmpJU3VoRllTVjZtCmlqblNucHBKZWhFUGxPMkFNcmJzU0VpaFB1N294Wm9iZDFtdWF4bWtVa0NoSzZLeGV0RjVEdWhRMi80NEMvSDIKOWk1bnpMMlRST3RndGRJZjAveUF5N05COHlOY3FPR0QKLS0tLS1FTkQgQ0VSVElGSUNBVEUgUkVRVUVTVC0tLS0tCg==
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

## Service accounts to pods
Pods authenticate to the API Server using ServiceAccounts. If the serviceAccount name is not specified, the default service account for the namespace is used during a pod creation.  
  
Reference: `https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/`

Now, create a service account `pvviewer`:

```sh
kubectl create serviceaccount pvviewer
```

To create a clusterrole:

```sh
kubectl create clusterrole pvviewer-role --resource=persistentvolumes --verb=list
```

To create a clusterrolebinding:

```sh
kubectl create clusterrolebinding pvviewer-role-binding --clusterrole=pvviewer-role --serviceaccount=default:pvviewer
```

Solution manifest file to create a new pod called `pvviewer` as follows:

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: pvviewer
  name: pvviewer
spec:
  containers:
  - image: redis
    name: pvviewer
  # Add service account name
  serviceAccountName: pvviewer
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

# Persistant Volumes
Add a `persistentVolumeClaim` definition to pod definition file.
Solution manifest file to create a pvc `my-pvc` as follows:
```yaml
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
       storage: 10Mi
```
And then, update the pod definition file as follows:
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: use-pv
  name: use-pv
spec:
  containers:
  - image: nginx
    name: use-pv
    volumeMounts:
    - mountPath: "/data"
      name: mypd
  volumes:
    - name: mypd
      persistentVolumeClaim:
        claimName: my-pvc
```

# Pod
## Run user as pod
Solution manifest file to create a pod called `non-root-pod` as follows:

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: non-root-pod
spec:
  securityContext:
    runAsUser: 1000
    fsGroup: 2000
  containers:
  - name: non-root-pod
    image: redis:alpine
```

Verify the user and group IDs by using below command:

```
kubectl exec -it non-root-pod -- id
```

## Taint Nodes and Pods
To add taints on the `node01` worker node:

```sh
kubectl taint node node01 env_type=production:NoSchedule
```

Now, deploy `dev-redis` pod and to ensure that workloads are not scheduled to this `node01` worker node.

```sh
kubectl run dev-redis --image=redis:alpine
```

To view the node name of recently deployed pod:

```sh
kubectl get pods -o wide
```

Solution manifest file to deploy new pod called `prod-redis` with toleration to be scheduled on `node01` worker node.

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: prod-redis
spec:
  containers:
  - name: prod-redis
    image: redis:alpine
  tolerations:
  - effect: NoSchedule
    key: env_type
    operator: Equal
    value: production     
```

To view only `prod-redis` pod with less details:

```sh
kubectl get pods -o wide | grep prod-redis
```
# Network
## Network Policy
Solution manifest file to create a network policy `ingress-to-nptest` as follows:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: ingress-to-nptest
  namespace: default
spec:
  podSelector:
    matchLabels:
      run: np-test-1
  policyTypes:
  - Ingress
  ingress:
  - ports:
    - protocol: TCP
      port: 80
```

ingress is traffic entering pod 
egress is traffic exiting pod
