# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "palekiwi/rhel9.1"

  config.ssh.insert_key = false

  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provider :libvirt do |v|
    v.memory = 256
    v.cpus = 1
  end

  # Control
  config.vm.define :control do |control|
    control.vm.hostname = "control.test"
    control.vm.provider :libvirt do |v|
      v.memory = 1024
      v.cpus = 2
    end
  end

  # Server Machines
  SERVERS = 3
  
  (1..SERVERS).each do |machine_id|
    config.vm.define "machine#{machine_id}" do |machine|
      machine.vm.hostname = "machine#{machine_id}"
      machine.vm.network "private_network", ip: "192.168.77.#{20+machine_id}"
  
      # Only execute once the Ansible provisioner,
      # when all the machines are up and ready.
      if machine_id == SERVERS
        machine.vm.provision :ansible do |ansible|
          # Disable default limit to connect to all the machines
          ansible.limit = "all"
          ansible.playbook = "playbooks/provision.yml"
          ansible.groups = {
            "db" => ["machine1"],
            "app" => ["machine[2:#{SERVERS}]"]
          }
        end
      end
    end
  end

end
