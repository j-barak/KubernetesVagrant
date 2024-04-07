# KubernetesVagrant

This repository contains Vagrant configurations to quickly 
set up a Kubernetes cluster for development or testing purposes
on premise.

## Prerequisites

Before using these Vagrant configurations, make sure you have the following prerequisites installed on your system:

- [Vagrant](https://www.vagrantup.com/)
- [VirtualBox](https://www.virtualbox.org/)

## Getting Started

To set up a Kubernetes cluster using Vagrant, follow these steps:

1. Clone this repository to your local machine, open Git BASH in the projects folder and run:
```bash
vagrant up
3. Use vagrant to ssh into each VM(separate terminals):
```bash
vagrant ssh master
vagrant ssh worker1
vagrant ssh worker2
4.	Then on the master node:
```bash
sudo kubeadm init --apiserver-advertise-address=192.168.50.100
And copy the join command. 
5.	Then run the commands:
```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
6.	And install the network Calico add-on:
```bash
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
7.	Go to each worker node and:
```bash
mkdir -p $HOME/.kube
8.	Return to master and copy the .kube/config file to the worker VMs:
```bash
cd .kube/
scp config vagrant@192.168.50.101:~/.kube
9.	Then on the worker VMs install the Calico add-on:
```bash
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
10.	Finally, join the worker nodes to the cluster via join command.

Note: A better approach is to build your own Vagrant Base Box for Ubuntu server 22.04 and use it as your local box provider.
Check that each worker node has /etc/cni/net.d/ folder with all the neccessary Calico configurations of the target cluster.
For advanced trouble shooting of the issue (when master Ready, but worker NotReady) use the following command on master node:
```bash
kubectl config view
kubectl config --kubeconfig=~/.kube/config set-cluster NAME_CLUSTER --server=https://192.168.50.100 --certificate-authority=clusterca.crt

