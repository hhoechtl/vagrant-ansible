# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

# Add the projectname here => don't use more than 8 letters and no dashes (-)
PROJECT_NAME = "myproject"

# Which virtual hosts should be applied for this project => hostsupdater will set /etc/hosts
HOSTS = [PROJECT_NAME+".local.de","de."+PROJECT_NAME+".local.de"]
VHOSTS = HOSTS.clone
# If you enable the role profiling, uncomment the following line
HOSTS.concat(["prof."+PROJECT_NAME+".local.de"])

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "puphpet/debian75-x64"
  config.vm.hostname = PROJECT_NAME+".local.de"
  config.hostsupdater.aliases = HOSTS

  config.vm.network "private_network", ip: "192.168.33.55"
  # For performance reasons we use nfs as mount
  config.vm.synced_folder ".", "/vagrant", type: "nfs"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
    # Configure memory of the VM
    vb.customize ["modifyvm", :id, "--memory", "1024"]
    # Configure number of CPUs of the VM
    vb.customize ["modifyvm", :id, "--cpus", "2"]
    vb.customize ["modifyvm", :id, "--name", PROJECT_NAME]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.name = PROJECT_NAME
  end

    # Provisioning configuration for Ansible.
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provisioning/site.yml"
    # Run commands as root.
    ansible.sudo = true
    ansible.extra_vars = {
      project_name: PROJECT_NAME,
      virtual_hosts: VHOSTS
    }
  end

end
