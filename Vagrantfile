# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.require_version ">= 1.5.0"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "hashicorp/precise64"

  config.vm.synced_folder ".", "/vagrant"

  config.vm.provider :virtualbox do |vb|
    vb.name = ENV['VM_NAME'] || "CoreOS Packer"
    vb.cpus = 1
    vb.memory = 1024

    # Create and attach a target HDD
    vb.customize [
      "createhd",
      "--filename", "tmp/CoreOS",
      "--size", "40000",
      "--format", "VMDK",
    ]
    vb.customize [
      "storageattach", :id,
      "--storagectl", "SATA Controller",
      "--port", "1",
      "--device", "0",
      "--type", "hdd",
      "--medium", "tmp/CoreOS.vmdk",
    ]
  end

  config.vm.provision :shell do |s|
    s.inline = <<-EOT
      sudo /vagrant/tmp/coreos-install -d /dev/sdb
      sudo mount /dev/sdb6 /mnt
      sudo cp /vagrant/oem/cloud-config.yml /mnt/
      sudo mkdir -p /mnt/bin
      sudo cp /vagrant/oem/coreos-setup-environment /mnt/bin/
      sudo umount /mnt
    EOT
  end
end
