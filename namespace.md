☸️ What is a Namespace in Kubernetes?

A Namespace is a logical isolation layer in a Kubernetes cluster.

👉 It allows you to group resources and separate environments (dev, test, prod) inside the same cluster.

🎯 Why Namespaces are used?

Separate environments (dev / qa / prod)

Avoid resource name conflicts

Apply RBAC (access control)

Set resource limits (quota)

Better organization & security

🧱 Default Kubernetes Namespaces
kubectl get namespaces

Namespace	Purpose
default	Default workload namespace
kube-system	Core K8s components
kube-public	Public readable data
kube-node-lease	Node heartbeat info
📦 Namespaced vs Cluster-wide Resources
Namespaced resources

Pods

Deployments

Services

ConfigMaps

Secrets

Jobs / CronJobs

Cluster-wide resources

Nodes

PersistentVolumes

StorageClasses

Namespaces themselves

👉 Example:

kubectl get pods -n kube-system
kubectl get nodes

🛠️ Namespace Commands
Create Namespace
kubectl create namespace dev

Create using YAML
apiVersion: v1
kind: Namespace
metadata:
  name: prod

kubectl apply -f namespace.yaml

Set Default Namespace (Current Context)
kubectl config set-context --current --namespace=dev


Check:

kubectl config view --minify | grep namespace

List Resources in Namespace
kubectl get all -n dev

Delete Namespace
kubectl delete namespace dev


⚠️ Deletes everything inside it

🔐 Namespace + RBAC Example

Restrict user access to dev namespace:

kubectl create role dev-reader \
  --verb=get,list,watch \
  --resource=pods \
  -n dev

📊 ResourceQuota Example

Limit CPU & memory in namespace:

apiVersion: v1
kind: ResourceQuota
metadata:
  name: dev-quota
  namespace: dev
spec:
  hard:
    requests.cpu: "2"
    requests.memory: 2Gi
    limits.cpu: "4"
    limits.memory: 4Gi

🧠 Real-Time Use Case

Single cluster setup

dev   → developers
qa    → testing team
prod  → production


Each namespace:

Separate deployments

Separate access

Separate limits

❓ Common Interview Questions
Q: Can two namespaces have same pod name?

✅ Yes
❌ Not in the same namespace

Q: Can pods in different namespaces communicate?

✅ Yes (by default)
Use:

service-name.namespace.svc.cluster.local

Q: How to isolate network traffic?

👉 Use Network Policies

🚨 Best Practices

✔ Don’t run prod workloads in default
✔ Use namespaces for environments
✔ Apply quotas & RBAC
✔ Clean unused namespaces

🧪 Useful Commands Cheat Sheet
kubectl get ns
kubectl get pods -n kube-system
kubectl create ns test
kubectl delete ns test
kubectl config set-context --current --namespace=test
