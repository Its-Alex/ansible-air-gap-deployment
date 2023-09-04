# -*- mode: ruby -*-
# vi: set ft=ruby :

cluster = {
  "admin" => { :ip => "192.168.56.10", :cpus => 2, :mem => 2048 },
  "server001" => { :ip => "192.168.56.11", :cpus => 2, :mem => 2048 }
}

Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-22.04"

  cluster.each_with_index do |(hostname, info), index|

    config.vm.define hostname do |cfg|
      cfg.vm.box = "bento/ubuntu-22.04"
      
      cfg.vm.synced_folder ".", "/vagrant"
      cfg.vm.provision "file", source: "./ssh_key/", destination: "/home/vagrant/"
      
      cfg.vm.provider :virtualbox do |vb, override|
        override.vm.hostname = hostname
        vb.name = hostname

        # Create a private network, which allows host-only access to the machine
        # using a specific IP.
        override.vm.network :private_network, ip: "#{info[:ip]}"

        vb.cpus = info[:cpus]
        vb.memory = info[:mem]
      end

      cfg.vm.provision "shell", inline: <<-SHELL
        cat /home/vagrant/ssh_key/vagrant_dev.pub >> /home/vagrant/.ssh/authorized_keys

        # Install ansible, must be done with network enabled
        apt update
        apt install -y python3-pip pipx
        pipx install --include-deps ansible==8.3.0
        pipx ensurepath

        # Enable VPN and disable outgoing traffic on default interface
        ufw default allow incoming
        ufw default allow outgoing
        ufw allow ssh
        ufw deny out on eth0
        ufw deny in on eth0
        yes | ufw enable
      SHELL
    end
  end

end