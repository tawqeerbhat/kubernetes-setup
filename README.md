# Multi-Node Kubernetes Cluster Setup Using Kubeadm

This readme provides step-by-step instructions for setting up a multi-node Kubernetes cluster using Kubeadm.

## Overview

This guide provides detailed instructions for setting up a multi-node Kubernetes cluster using Kubeadm. The guide includes instructions for installing and configuring containerd and Kubernetes, disabling swap, initializing the cluster, installing Flannel, and joining nodes to the cluster.

## Prerequisites

Before starting the installation process, ensure that the following prerequisites are met:

- You have at least two Ubuntu 18.04 or higher servers available for creating the cluster.
- Each server has at least 2GB of RAM and 2 CPU cores.
- The servers have network connectivity to each other.
- You have root access to each server.

## Installation Steps

The following are the step-by-step instructions for setting up a multi-node Kubernetes cluster using Kubeadm:

### Update the system's package list and install necessary dependencies

Run the following commands:

```bash
sudo apt-get update
sudo apt install apt-transport-https curl -y

Install containerd
To install Containerd, use the following commands:

bash
Copy code
sudo mkdir -p /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install containerd.io -y
Create containerd configuration
Next, create the containerd configuration file using the following commands:

bash
Copy code
sudo mkdir -p /etc/containerd
sudo containerd config default | sudo tee /etc/containerd/config.toml
Edit /etc/containerd/config.toml
Edit the containerd configuration file to set SystemdCgroup to true. Use the following command to open the file:

bash
Copy code
sudo nano /etc/containerd/config.toml
Set SystemdCgroup to true:

bash
Copy code
SystemdCgroup = true
Alternatively, use this command:

bash
Copy code
sudo sed -i -e 's/SystemdCgroup = false/SystemdCgroup = true/g' /etc/containerd/config.toml
Restart containerd:

bash
Copy code
sudo systemctl restart containerd
Install Kubernetes
To install Kubernetes, use the following commands:

bash
Copy code
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
sudo systemctl enable --now kubelet
Disable swap
Disable swap using the following command:

bash
Copy code
sudo swapoff -a
If there are any swap entries in the /etc/fstab file, remove them using a text editor such as nano:

bash
Copy code
sudo nano /etc/fstab
Enable kernel modules
Enable necessary kernel modules with the following command:

bash
Copy code
sudo modprobe br_netfilter
Add some settings to sysctl:

bash
Copy code
sudo sysctl -w net.ipv4.ip_forward=1
Initialize the Cluster (Run only on master)
Use the following command to initialize the cluster:

bash
Copy code
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
Set up kubeconfig file for the master node
Create a .kube directory in your home directory:

bash
Copy code
mkdir -p $HOME/.kube
Copy the Kubernetes configuration file to your home directory:

bash
Copy code
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
Change ownership of the file:

bash
Copy code
sudo chown $(id -u):$(id -g) $HOME/.kube/config
Install Flannel (Run only on master)
Use the following command to install Flannel:

bash
Copy code
kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml
Verify Installation
Verify that all the pods are up and running:

bash
Copy code
kubectl get pods --all-namespaces
Join Nodes
To add nodes to the cluster, run the kubeadm join command with the appropriate arguments on each node. The command will output a token that can be used to join the node to the cluster.

vbnet
Copy code

This Markdown file is now ready to be copied and used as you prefer. If you want me to provide it as a downloadable `.md` file, just let me know!





