# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
  end
  config.vm.define :master do |master_config|
    master_config.vm.box = "ubuntu/trusty64"
    master_config.vm.host_name = 'salt-master'
    master_config.vm.network "private_network", ip: "192.168.50.10"
    master_config.vm.synced_folder "salt/roots/", "/srv"

    master_config.vm.provision :salt do |salt|
      salt.master_config = "salt/etc/master"
      salt.master_key = "salt/keys/master_minion.pem"
      salt.master_pub = "salt/keys/master_minion.pub"
      salt.minion_key = "salt/keys/master_minion.pem"
      salt.minion_pub = "salt/keys/master_minion.pub"
      salt.seed_master = {
                          "minion1" => "salt/keys/minion1.pub",
                          "minion2" => "salt/keys/minion2.pub"
                         }

      salt.install_type = "stable"
      salt.install_master = true
      salt.no_minion = true
      salt.verbose = true
      salt.colorize = true
      salt.bootstrap_options = "-P -c /tmp"
    end
  end

  config.vm.define :minion1 do |minion_config|
    minion_config.vm.box = "ubuntu/trusty64"
    minion_config.vm.host_name = 'trustyminion'
    minion_config.vm.network "private_network", ip: "192.168.50.11"

    minion_config.vm.provision :salt do |salt|
      salt.minion_config = "salt/etc/minion1"
      salt.minion_key = "salt/keys/minion1.pem"
      salt.minion_pub = "salt/keys/minion1.pub"
      salt.install_type = "stable"
      salt.verbose = true
      salt.colorize = true
      salt.bootstrap_options = "-P -c /tmp"
    end
  end

  config.vm.define :minion2 do |minion_config|
    minion_config.vm.box = "mwrock/windows2012r2"
    minion_config.vm.host_name = 'windowsminion'
    minion_config.vm.network "private_network", ip: "192.168.50.12"

    minion_config.vm.provision :salt do |salt|
      salt.minion_config = "salt/etc/minion2"
      salt.minion_key = "salt/keys/minion2.pem"
      salt.minion_pub = "salt/keys/minion2.pub"
      salt.install_type = "stable"
      salt.version = "2016.3.2" # windows only
      salt.verbose = true
      salt.colorize = true
      salt.bootstrap_options = "-P -c /tmp"
    end
  end

end
