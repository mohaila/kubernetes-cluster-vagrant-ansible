BOX = "bento/ubuntu-16.04"
NODES = 3

Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
     
  config.vm.define "k8s-master" do |m|
    m.vm.provider "virtualbox" do |v|
      v.name = "k8s-master"
      v.memory = 1024
      v.cpus = 2
    end  
    m.vm.box = BOX
    m.vm.network "private_network", ip: "192.168.33.10"
    m.vm.hostname = "k8s-master"
    m.vm.provision "ansible" do |a|
      a.playbook = "ansible/playbook.yml"
      a.extra_vars = {
          node_ip: "192.168.33.10",
      }
    end
  end

  (1..NODES).each do |i|
    config.vm.define "k8s-node-#{i}" do |n|
      n.vm.provider "virtualbox" do |v|
        v.name = "k8s-node-#{i}"
        v.memory = 2048
        v.cpus = 4
      end 
      n.vm.box = BOX
      n.vm.network "private_network", ip: "192.168.33.#{i + 10}"
      n.vm.hostname = "k8s-node-#{i}"
      n.vm.provision "ansible" do |a|
          a.playbook = "ansible/playbook.yml"
          a.extra_vars = {
              node_ip: "192.168.33.#{i + 10}",
          }
      end
    end
  end
end
