# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  config.vm.synced_folder '.', '/vagrant', type: 'rsync', rsync__exclude: [".git/", "Vagrantfile"]
  config.ssh.insert_key = false
  config.vm.box_check_update = false

  config.vm.define "workstation" do |workstation|
    workstation.vm.box = "almalinux/8"
    workstation.vm.hostname = "workstation"
    workstation.vm.network :private_network, :ip => "192.168.188.188"
    workstation.vm.provider "libvirt" do |lv|
      lv.memory = 2048
      lv.cpus = 2
      lv.machine_virtual_size = 50
    end
    workstation.vm.provision "shell", inline: <<-SHELL
      /usr/bin/sed -i -e 's/rhgb quiet/console=ttyS0/' /etc/default/grub
      /usr/sbin/grub2-mkconfig -o /boot/grub2/grub.cfg
      dnf install -y cloud-utils-growpart
      growpart /dev/vda 3
      xfs_growfs /dev/vda3
      sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
      systemctl restart sshd
      dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
      dnf install -y sshpass ansible
    SHELL
    workstation.vm.provision :ansible_local do |ansible|
      ansible.playbook = "/vagrant/playbooks/site.yml"
      ansible.install = false
      ansible.compatibility_mode = "2.0"
      ansible.inventory_path = "/vagrant/inventory"
      ansible.config_file = "/vagrant/ansible.cfg"
      ansible.limit = "all"
    end
  end
end
