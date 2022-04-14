# GitHub Actions Self Hosted Runner (Autoscaling with Kubernetes)

Content

* [Create K8S cluster with Ansible](#Create K8S cluster with Ansible)
* [Install cert-manager on K8S](#Install cert-manager on K8S)
* [Install actions-runner-controller](#Install actions-runner-controller)

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





