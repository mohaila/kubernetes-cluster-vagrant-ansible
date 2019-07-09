# Kubernetes cluster using Vagrant and Ansible

## Install Ansible on the local machine
- Install a Python 3 virtual environment
```bash
bash
$ python3 -m venv pyenv
# active your environment
$ . pyenv/bin/activate
# update PIP
$ python -m pip install --upgrade pip
# install packages
$ pip install wheel paramiko ansible 
$ pip list
Package      Version
------------ -------
ansible      2.8.2
asn1crypto   0.24.0
bcrypt       3.1.7
cffi         1.12.3
cryptography 2.7
Jinja2       2.10.1
MarkupSafe   1.1.1
paramiko     2.6.0
pip          19.1.1
pycparser    2.19
PyNaCl       1.3.0
PyYAML       5.1.1
setuptools   40.8.0
six          1.12.0
wheel        0.33.4
```
## Install Vagrant
Download setup files for your OS from https://www.vagrantup.com/

## Install Virtualbox
Download latest setup files for your OS from https://www.virtualbox.com and install Virtualbox Extension Pack

## Start your Kubernetes cluster machines
- Modify Vagrantfile NODES to suit your needs.
- Modify ansible/playbook.yml if you add more nodes.
- Start your cluster:
```bash
bash
$ vagrant up
```
- Wait until vagrant finish creating your VMs.

## Verify your cluster setup
- Connect to your Kubernetes master
```bash
bash
$ vagrant ssh k8s-master
```
- List your kubernetes cluster nodes:
```bash
bash
$ kubectl cluster-info
Kubernetes master is running at https://192.168.33.10:6443
KubeDNS is running at https://192.168.33.10:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'
$ kubectl get nodes
NAME         STATUS   ROLES    AGE   VERSION
k8s-master   Ready    master   21m   v1.15.0
k8s-node-1   Ready    <none>   18m   v1.15.0
k8s-node-2   Ready    <none>   15m   v1.15.0
k8s-node-3   Ready    <none>   12m   v1.15.0
```

### Have fun with deploying services