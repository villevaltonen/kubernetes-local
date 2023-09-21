## kubernetes-local

This repository contains files for creating a local multi-node Kubernetes cluster with the help of Vagrant and Ansible. 

The cluster setup has four virtual machines: one control node, two worker nodes and one NFS-server. The NFS-server is used for dynamically provision storage for user workloads with a default StorageClass and PersistentVolumeClaims.

### Requirements
- Vagrant
- VirtualBox

### Stack
- Kubernetes v1.28.2
- Ubuntu 22.04 (Jammy Jellyfish)

### Other Kubernetes components
- Cilium 
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
The repository contains a manifest for a `dnsutils` pod as well as for an example `PersistentVolumeClaim`. These can be found under `debug` directory.

### Example application
An example application can be deployed by using the manifests in `hello-server` directory.

### IP-address ranges
- VirtualBox VMs: 192.168.60.0/25
- Pod IPs: 192.168.61.0/24
- Cluster Service IPs: 10.96.0.0/12
- Cilium cluster pool: 10.0.0.0/22
- Cilium load balancer IP pool: 10.0.10.0/24
