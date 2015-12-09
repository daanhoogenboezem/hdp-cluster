
Vagrant.configure(2) do |config|

  config.vm.define "master" do |master|
    master.vm.box = "hdp_vm"
    master.vm.network "forwarded_port", guest: 8080, host: 8080
    master.vm.hostname = "master.locallab.com"
    master.vm.network "private_network", ip: "192.168.100.10"

    master.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--name", "master.locallab.com"]
      vb.customize ["modifyvm", :id, "--memory", "4096"]
    end

    master.vm.provision "ansible" do |ansible|
      ansible.verbose = "v"
      ansible.playbook = "provisioning/ambari.yml"
      ansible.sudo = true
      ansible.host_key_checking = false
    end
  end

  config.vm.define "slave1" do |slave1|
    slave1.vm.box = "hdp_vm"
    slave1.vm.hostname = "slave1.locallab.com"
    slave1.vm.network "private_network", ip: "192.168.100.20"

    slave1.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--name", "slave1.locallab.com"]
      vb.customize ["modifyvm", :id, "--memory", "3072"]
    end

    slave1.vm.provision "ansible" do |ansible|
      ansible.verbose = "v"
      ansible.playbook = "provisioning/ambari.yml"
      ansible.sudo = true
      ansible.host_key_checking = false
    end
  end

  config.vm.define "slave2" do |slave2|
    slave2.vm.box = "hdp_vm"
    slave2.vm.hostname = "slave2.locallab.com"
    slave2.vm.network "private_network", ip: "192.168.100.30"

    slave2.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--name", "slave2.locallab.com"]
      vb.customize ["modifyvm", :id, "--memory", "3072"]
    end

    slave2.vm.provision "ansible" do |ansible|
      ansible.verbose = "v"
      ansible.playbook = "provisioning/ambari.yml"
      ansible.sudo = true
      ansible.host_key_checking = false
    end
  end
end
