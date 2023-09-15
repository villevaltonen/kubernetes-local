CODE_NAME = "jammy"
IMAGE_NAME = "ubuntu/jammy64"
WORKER_COUNT = 2
KUBERNETES_VERSION = "1.28.1"


Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

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
            ansible.playbook = "ansible/playbooks/kubernetes/master.yaml"
            ansible.extra_vars = {
                node: "master"
            }
        end
    end

    (1..WORKER_COUNT).each do |node_id|
        config.vm.define "k8s-node-#{node_id}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.box_check_update = "True"
            node.vm.network "private_network", ip: "192.168.60.10#{node_id}"
            node.vm.provider "virtualbox" do |node|
                node.memory = 1024
                node.cpus = 1
            end
            node.vm.provision "ansible_local" do |ansible|
                ansible.playbook = "ansible/playbooks/kubernetes/node.yaml"
                ansible.extra_vars = {
                    node: "#{node_id}"
                }
            end
        end
    end
        
end