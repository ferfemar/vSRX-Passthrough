# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

require "vagrant-host-shell"
require "vagrant-junos"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define "ndo-client", primary: true do |ndo|
    ndo.vm.box = "juniper/netdevops-ubuntu1404-headless"
    ndo.vm.hostname = "Client"
    ndo.vm.network "private_network",
                   ip: "172.16.0.10",
                   virtualbox__intnet: "NetDevOps-Client"
    ndo.vm.synced_folder "", "/vagrant"
    ndo.ssh.password = "vagrant"

    ndo.vm.provider "virtualbox" do |v|
    end

    ndo.vm.provision "shell" do |s|
      # this script provisions the ndo box for you
      s.path = "scripts/ndo-setup-client.sh"
    end
  end

  config.vm.define "ndo-server", primary: true do |ndo|
    ndo.vm.box = "juniper/netdevops-ubuntu1404-headless"
    ndo.vm.hostname = "Server"
    ndo.vm.network "private_network",
                   ip: "192.168.0.10",
                   virtualbox__intnet: "NetDevOps-Server"
    ndo.vm.synced_folder "", "/vagrant"
    ndo.ssh.password = "vagrant"

    ndo.vm.provider "virtualbox" do |v|
    end

    ndo.vm.provision "shell" do |s|
      # this script provisions the ndo box for you
      s.path = "scripts/ndo-setup-server.sh"
    end
  end 

  config.vm.define "srx" do |srx|
    srx.vm.box = "juniper/ffp-12.1X47-D20.7"
    srx.vm.hostname = "NetDevOps-SRX01"
    srx.vm.network "private_network",
                   ip: "172.16.0.1",
                   nic_type: 'virtio',
                   virtualbox__intnet: "NetDevOps-Client"
    srx.vm.network "private_network",
                   ip: "192.168.0.1",
                   nic_type: 'virtio',
                   virtualbox__intnet: "NetDevOps-Server"

    srx.vm.synced_folder "", "/vagrant", disabled: true

    srx.vm.provider "virtualbox" do |v|
      # increase RAM to support AppFW and IPS
      # comment out to make it run at 2GB
      v.customize ["modifyvm", :id, "--memory", "3072"]
      v.check_guest_additions = false
    end

    srx.vm.provision "file", source: "scripts/srx-setup.sh", destination: "/tmp/srx-setup.sh"
    srx.vm.provision :host_shell do |host_shell|
      # provides the inital configuration
      host_shell.inline = 'vagrant ssh srx -c "/usr/sbin/cli -f /tmp/srx-setup.sh"'
    end
  end
end
