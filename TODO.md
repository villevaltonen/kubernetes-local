## Features
- Cilium based LB IPAM

## Issues
- Connectivity to the public web
    - Fix coredns configmap in kube-system namespace: invalid /etc/resolv.conf points to 10.0.2.3, but dns resolution works if set to, for example to one of the nameservers of Google