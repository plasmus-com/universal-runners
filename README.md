# GitHub Actions Self Hosted Runner (Autoscaling with Kubernetes

* [Create K8S cluster with Ansible](#create-k8s-cluster-with-ansible)
* [Install cert-manager on K8S](#install-cert-manager-on-k8s)
* [Install actions-runner-controller](#install-actions-runner-controller)

# Create K8S cluster with Ansible

Go over each file under ``kube-cluster`` directory and create K8S cluster with [Ansible](https://www.digitalocean.com/community/tutorials/how-to-create-a-kubernetes-cluster-using-kubeadm-on-ubuntu-20-04).


[Install Ansible](https://github.com/hackcoderr/ansible-setup) on your machine and follow this repository for more [info](https://github.com/hackcoderr/ansible-setup). 

Now follow this setup to create K8S cluster.


Step 1: Installing Kubernetetes’ Dependencies.

```
cd kube-cluster

ansible-playbook kube-dependencies.yml
```
Step 2: Setting Up the Control Plane Node.

```
ansible-playbook control-plane.yml
```
Step 3: Setting Up the Worker Nodes

```
ansible-playbook workers.yml
```

- Check connection with Kubernetes
```bash
kubectl get svc
```
# Install Cert-Manager K8S

- Add Helm repo
```bash
helm repo add jetstack https://charts.jetstack.io
```

- Update Helm
```bash
helm repo update
```

- Search for latest verion
```bash
helm search repo cert-manager
```

- Split the screen and install Helm chart
```bash
watch kubectl get pods -A
```

- Install Helm chart
```bash
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.6.0 \
  --set prometheus.enabled=false \
  --set installCRDs=true
```

## Install actions-runner-controller


- Create namespace
```bash
kubectl create ns actions
```

- Create secret to authenticate with GitHub
```bash
kubectl create secret generic controller-manager \
    -n actions \
    --from-literal=github_app_id=<> \
    --from-literal=github_app_installation_id=<> \
    --from-file=github_app_private_key=<>
```

- Add actions  helm repo
```bash
helm repo add actions-runner-controller https://actions-runner-controller.github.io/actions-runner-controller
helm repo update
helm search repo actions
```

- Install Helm chart
```bash
helm install actions \
    actions-runner-controller/actions-runner-controller \
    --namespace actions \
    --version 0.14.0 \
    --set syncPeriod=1m
```

## Create Single GitHub Actions Self Hosted Runner 

- Create `k8s/runner.yaml`
```bash
kubectl apply -f k8s/runner.yaml
kubectl get pods -n actions
kubectl logs -f k8s-single-runner -c runner -n actions
```

