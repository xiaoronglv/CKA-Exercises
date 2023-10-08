# Cluster Architecture, Installation & Configuration (25%)

### Setup autocomplete for k8s commands

<details><summary>show</summary>
<p>

```bash
source <(kubectl completion bash)
echo "source <(kubectl completion bash)" >> ~/.bashrc
```

</p>
</details>

### Show kubeconfig settings

<details><summary>show</summary>
<p>

```bash
kubectl config view
```

</p>
</details>

### Use multiple kubeconfig files at the same time

<details><summary>show</summary>
<p>

```bash
KUBECONFIG=~/.kube/config:~/.kube/kubconfig2
```

</p>
</details>

### Create a role the will allow users to get, watch, and list pods and container logs

<details><summary>show</summary>
<p>

```bash
# create a file named role.yml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods", "pods/log"]
  verbs: ["get", "watch", "list"]

# create the role
kubectl apply -f role.yml
```

</p>
</details>

### Create a role binding that binds to a role named pod-reader, applies to a user named `dev`

<details><summary>show</summary>
<p>

```bash
# create a file named role-binding.yml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: pod-reader
  namespace: default
subjects:
- kind: User
  name: dev
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

</p>
</details>

###

### Permanently save the namespace for all subsequent kubectl commands in that context.

<details><summary>show</summary>
<p>

```bash
kubectl config set-context --current --namespace=ggckad-s2
```

</p>
</details>

### Set a context utilizing a specific username and namespace

<details><summary>show</summary>
<p>

```bash
kubectl config set-context gce --user=cluster-admin --namespace=foo \
  && kubectl config use-context gce
```

</p>
</details>

### List services sorted by name

<details><summary>show</summary>
<p>

```bash
kubectl get services --sort.by=.metadata.name
```

</p>
</details>

### Get the external IP of all nodes

<details><summary>show</summary>
<p>

```bash
kubectl get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=="ExternalIP")].address}'
```

</p>
</details>

### Get the status of the control plane components (cluster health)

<details><summary>show</summary>
<p>

```bash
# check the livez endpoint
curl -k https://localhost:6443/livez?verbose

# or

kubectl get --raw='/livez?verbose'

# check the readyz endpoint
curl -k https://localhost:6443/readyz?verbose

# or

kubectl get --raw='/readyz?verbose'

# check the healthz endpoint
curl -k https://localhost:6443/healthz?verbose

# or

kubectl get --raw='/healthz?verbose'
```

[Kubernetes API Health Endpoints](https://kubernetes.io/docs/reference/using-api/health-checks/)

</p>
</details>

### List all pods that are in the running state using field selectors

<details><summary>show</summary>
<p>

```bash
kubectl get po --field-selector status.phase=Running
```

</p>
</details>

### List all services in the default namespace using field selectors

<details><summary>show</summary>
<p>

```bash
kubectl get svc --field-selector metadata.namespace=default
```

</p>
</details>

### List all API resources in your Kubernetes cluster

<details><summary>show</summary>
<p>

```bash
kubectl api-resources
```

</p>
</details>

### List the services on your Linux operating system that are associated with Kubernetes

<details><summary>show</summary>
<p>

```bash
systemctl list-unit-files --type service --all | grep kube
```

</p>
</details>

### List the status of the kubelet service running on the Kubernetes node

<details><summary>show</summary>
<p>

```bash
systemctl status kubelet
```

</p>
</details>
