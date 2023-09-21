Manage, Plan, Schedule, Monitor Nodes
Contains:
- [[kube-apiserver]] - Responsible for orchestrating all operations within the cluster
- [[ETCD Cluster]] - Highly Available Key-Value Store
- [[kube-scheduler]] - Identifies the right node to run the container on
- [[Controller-Manager]]
	- Replication-Manager - Ensures correct amount of containers are running in a replication group
	- Node-Controller - Responsible for onboarding new nodes onto the cluster
	- 