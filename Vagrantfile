# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
BRIDGE_NETWORK_ADAPTER = "wlp2s0"
PUBLIC_SSHKEY_PATH = "~/.ssh/id_dsa.pub"

cluster = {
  "postgres" => { :ip => "192.168.0.180", :cpus => 1, :mem => 2048 },
  "n1vault" => { :ip => "192.168.0.170", :cpus => 1, :mem => 2048 },
  "n2vault" => { :ip => "192.168.0.171", :cpus => 1, :mem => 2048 }
}

 
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  cluster.each_with_index do |(hostname, info), index|

    config.vm.define hostname do |cfg|
      cfg.vm.provider :virtualbox do |vb, override|
        config.vm.box = "centos/7"
        #override.vm.network :private_network, ip: "#{info[:ip]}"
        override.vm.network "public_network", bridge: BRIDGE_NETWORK_ADAPTER, ip: "#{info[:ip]}"
        override.vm.hostname = hostname
        vb.name = hostname
        vb.gui = false
        vb.customize ["modifyvm", :id, "--memory", info[:mem], "--cpus", info[:cpus], "--hwvirtex", "on"]
      end # end provider
      config.vm.provision "file", source: PUBLIC_SSHKEY_PATH, destination: "~/.ssh/authorized_keys"
    end # end config

  end # end cluster
end