# -*- mode: ruby -*-
# vim: ft=ruby


# ---- Configuration variables ----

GUI               = true # Enable/Disable GUI
RAM               = 256 #128   # Default memory size in MB

# Network configuration
DOMAIN            = ".nat.example.com"
NETWORK           = "192.168.50."
NETMASK           = "255.255.255.0"

# Default Virtualbox .box
# See: https://wiki.debian.org/Teams/Cloud/VagrantBaseBoxes

#BOX               = 'centos/7'
#BOX               = 'debian/jessie64'
#BOX                = 'ubuntu/trusty64'   # db
BOX                = 'ubuntu/xenial64' # web
HOSTS = {
   "db" => [NETWORK+"11", RAM, GUI, BOX],
#   "web" => [NETWORK+"10", RAM, GUI, BOX],
}

ANSIBLE_INVENTORY_DIR = 'ansible/inventory'

# ---- Vagrant configuration ----

Vagrant.configure(2) do |config|
  HOSTS.each do | (name, cfg) |
    ipaddr, ram, gui, box = cfg

    config.vm.define name do |machine|
      machine.vm.box   = box
#      machine.vm.guest = :redhat
      machine.vm.guest = :ubuntu

      machine.vm.provider "virtualbox" do |vbox|
        vbox.gui    = gui
        vbox.memory = ram
      end

      machine.vm.hostname = name + DOMAIN
      machine.vm.network 'private_network', ip: ipaddr, netmask: NETMASK
    end
  end # HOSTS-each

  config.vm.provision "vai" do |ansible|
    ansible.inventory_dir=ANSIBLE_INVENTORY_DIR
    # optional: add a group listing all vagrant machines
    ansible.groups = {
      'secondGroup' => [ "db" ],
    #  '_provided_by_vagrant_'=> HOSTS.keys,
    }
  end

end
