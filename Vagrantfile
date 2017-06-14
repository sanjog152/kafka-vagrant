Vagrant.configure("2") do |config|
  config.vm.define "kafka1" do |kafka1|
    kafka1.vm.box = "ubuntu/trusty64"
    kafka1.vm.hostname = 'kafka1'
    kafka1.vm.box_url = "ubuntu/trusty64"

    kafka1.vm.network :private_network, ip: "192.168.56.101"

    kafka1.ssh.forward_agent    = true
    kafka1.ssh.insert_key       = false
    kafka1.ssh.private_key_path =  ["~/.vagrant.d/insecure_private_key","ansible/files/id_rsa"]
    kafka1.vm.provision :shell, privileged: false do |s|
      ssh_pub_key = File.readlines("ansible/files/id_rsa.pub").first.strip
      s.inline = <<-SHELL
      echo #{ssh_pub_key} >> /home/$USER/.ssh/authorized_keys
      sudo bash -c "echo #{ssh_pub_key} >> /root/.ssh/authorized_keys"
      SHELL
    end

    kafka1.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 1048]
      v.customize ["modifyvm", :id, "--name", "kafka1"]
    end
  end

  config.vm.define "kafka2" do |kafka2|
    kafka2.vm.box = "ubuntu/trusty64"
    kafka2.vm.hostname = 'kafka2'
    kafka2.vm.box_url = "ubuntu/trusty64"

    kafka2.vm.network :private_network, ip: "192.168.56.102"
    kafka2.ssh.forward_agent    = true
    kafka2.ssh.insert_key       = false
    kafka2.ssh.private_key_path =  ["~/.vagrant.d/insecure_private_key","ansible/files/id_rsa"]
    kafka2.vm.provision :shell, privileged: false do |s|
      ssh_pub_key = File.readlines("ansible/files/id_rsa.pub").first.strip
      s.inline = <<-SHELL
      echo #{ssh_pub_key} >> /home/$USER/.ssh/authorized_keys
      sudo bash -c "echo #{ssh_pub_key} >> /root/.ssh/authorized_keys"
      SHELL
    end
    kafka2.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 1048]
      v.customize ["modifyvm", :id, "--name", "kafka2"]
    end
  end
    # Install Kafka cluster on VMS
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ansible/playbook.yml"
    ansible.inventory_path = "provisioning/hosts"
    ansible.sudo = true
  end
end