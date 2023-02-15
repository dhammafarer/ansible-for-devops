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

  config.vm.provision :ansible do |ansible|
    ansible.playbook = "playbooks/provision.yml"
  end

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

  # App Server 1
  config.vm.define :app1 do |app|
    app.vm.hostname = "orc-app1.test"
  end

  # App Server 2
  config.vm.define "app2" do |app|
    app.vm.hostname = "orc-app2.test"
  end

  # DB Server
  config.vm.define "db" do |db|
    db.vm.hostname = "orc-db.test"
  end
end
