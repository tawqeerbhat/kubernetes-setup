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

### Installation Steps

