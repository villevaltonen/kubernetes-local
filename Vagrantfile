CODE_NAME = "jammy"
IMAGE_NAME = "ubuntu/jammy64"
NODE_COUNT = 2
KUBERNETES_VERSION = "1.28.2-1.1"
KUBERNETES_YAML_LOCATION = "/home/vagrant/kubernetes-yaml/"


Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    config.vm.define "nfs" do |nfs|
        nfs.vm.box = IMAGE_NAME
        nfs.vm.box_check_update = "True"
        nfs.vm.hostname = "k8s-nfs"
        nfs.vm.network "private_network", ip: "192.168.60.99"
        nfs.vm.provider "virtualbox" do |nfs|
            nfs.memory = 512
            nfs.cpus = 1
        end

        nfs.vm.provision "ansible_local" do |ansible|
            ansible.playbook = "ansible/playbooks/nfs/setup-nfs.yaml"
            ansible.extra_vars = {
                node_ip: "192.168.60.99"
            }
        end
    end


    config.vm.define "k8s-master" do |master|
        master.vm.box = IMAGE_NAME
        master.vm.box_check_update = "True"
        master.vm.network "private_network", ip: "192.168.60.100"
        master.vm.hostname = "k8s-master"
        master.vm.provider "virtualbox" do |master|
            master.memory = 2048
            master.cpus = 2
        end

        master.vm.provision "ansible_local" do |ansible|
            ansible.playbook = "ansible/playbooks/kubernetes/setup-master.yaml"
            ansible.extra_vars = {
                node_ip: "192.168.60.100",
                kubernetes_version: "#{KUBERNETES_VERSION}",
                kubernetes_yaml_location: "#{KUBERNETES_YAML_LOCATION}"
            }
        end

        master.vm.provision "ansible_local" do |ansible|
            ansible.playbook = "ansible/playbooks/kubernetes/setup-nfs-provisioner.yaml"
            ansible.extra_vars = {
                kubernetes_yaml_location: "#{KUBERNETES_YAML_LOCATION}"
            }
        end
    end

    (1..NODE_COUNT).each do |node_id|
        config.vm.define "k8s-node-#{node_id}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.hostname = "k8s-node-#{node_id}"
            node.vm.box_check_update = "True"
            node.vm.network "private_network", ip: "192.168.60.10#{node_id}"
            node.vm.provider "virtualbox" do |node|
                node.memory = 1024
                node.cpus = 1
            end
        
            node.vm.provision "ansible_local" do |ansible|
                ansible.playbook = "ansible/playbooks/kubernetes/setup-node.yaml"
                ansible.extra_vars = {
                    node_ip: "192.168.60.10#{node_id}",
                    kubernetes_version: "#{KUBERNETES_VERSION}",
                    kubernetes_yaml_location: "#{KUBERNETES_YAML_LOCATION}"
                }
            end
        end
    end

end