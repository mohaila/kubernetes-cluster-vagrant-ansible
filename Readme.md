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
## Microbot deployment
After Microbot deployment, view the Pods:
```bash
bash
$ kubectl get pods --all-namespaces
default       microbot-7dd47b8fd6-5c5zb                  1/1     Running   0          11m
default       microbot-7dd47b8fd6-8bzhd                  1/1     Running   0          11m
default       microbot-7dd47b8fd6-9fw68                  1/1     Running   0          11m
default       microbot-7dd47b8fd6-9fzr7                  1/1     Running   0          11m
default       microbot-7dd47b8fd6-crx4d                  1/1     Running   0          11m
default       microbot-7dd47b8fd6-ppcs5                  1/1     Running   0          11m
kube-system   calico-kube-controllers-658558ddf8-qg8xk   1/1     Running   0          11m
kube-system   calico-node-6rnnf                          1/1     Running   0          8m7s
kube-system   calico-node-bl9hb                          1/1     Running   0          11m
kube-system   calico-node-xvrbx                          1/1     Running   0          2m7s
kube-system   calico-node-zbddc                          1/1     Running   0          5m1s
kube-system   coredns-5c98db65d4-cp9m4                   1/1     Running   0          11m
kube-system   coredns-5c98db65d4-l4p7q                   1/1     Running   0          11m
kube-system   etcd-k8s-master                            1/1     Running   0          10m
kube-system   kube-apiserver-k8s-master                  1/1     Running   0          10m
kube-system   kube-controller-manager-k8s-master         1/1     Running   0          10m
kube-system   kube-proxy-mtngj                           1/1     Running   0          8m7s
kube-system   kube-proxy-n2pvz                           1/1     Running   0          5m1s
kube-system   kube-proxy-z8nns                           1/1     Running   0          2m7s
kube-system   kube-proxy-zxfqs                           1/1     Running   0          11m
kube-system   kube-scheduler-k8s-master                  1/1     Running   0          10m
```
And the services:
```bash
bash
$ kubectl get services
NAME               TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes         ClusterIP   10.96.0.1       <none>        443/TCP        15m
microbot-service   NodePort    10.108.174.59   <none>        80:31873/TCP   14m
```
To get more info:
```bash
bash
$ kubectl describe services/microbot-service
Name:                     microbot-service
Namespace:                default
Labels:                   app=microbot
Annotations:              <none>
Selector:                 app=microbot
Type:                     NodePort
IP:                       10.108.174.59
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  31873/TCP
Endpoints:                192.168.109.65:80,192.168.109.66:80,192.168.109.67:80 + 3 more...
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
```
To test all the pods, use the service IP, to test a specific pod use the endpoints:
```bash
bash
$ curl 10.108.174.59
<!DOCTYPE html>
<html>
  <style type="text/css">
    .centered
      {
      text-align:center;
      margin-top:0px;
      margin-bottom:0px;
      padding:0px;
      }
  </style>
  <body>
    <p class="centered"><img src="microbot.png" alt="microbot"/></p>
    <p class="centered">Container hostname: microbot-7dd47b8fd6-ppcs5</p>
  </body>
</html>
```
If you repeat the test, each time you get a response from a different pod (different Container hostname)

### Have fun with deploying services