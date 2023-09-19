## kubernetes-local

This repository contains files for creating a local multi-node Kubernetes cluster with the help of Vagrant and Ansible. 

The cluster setup has four virtual machines: one control node, two worker nodes and one NFS-server. The NFS-server is used for dynamically provision storage for user workloads with a default StorageClass and PersistentVolumeClaims.

### Requirements
- Vagrant
- VirtualBox

### Stack
- Kubernetes v1.28.2
- Ubuntu 22.04 (Jammy Jellyfish)

### Othere Kubernetes components
- Calico
- metrics-server

### How to use

#### Startup
`vagrant up`

#### SSH into a node
`vagrant ssh <machine, e.g. k8s-master>`

#### Power off machines
`vagrant halt`

#### Suspend machines
`vagrant suspend`

#### Delete everything
`vagrant destroy -f`

### Debug

The repository contains a manifest for a debugger pod. It can be found under `debug/`