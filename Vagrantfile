# -*- mode: ruby -*-
# vi: set ft=ruby :
# We set the last octet in IPV4 address here

VAGRANTFILE_API_VERSION = "2"

nodes = {
 'keystone'   => [1, 201]
}


Vagrant.configure(VAGRANTFILE_API_VERSION) do |config| 
  # Virtualbox
  config.vm.box = "precise64"
  config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/precise/current/precise-server-cloudimg-i386-vagrant-disk1.box"
  config.vm.synced_folder ".", "/vagrant", type: "nfs"
  config.vm.hostname = 'tin-os'

  config.berkshelf.berksfile_path = "Berksfile"
  config.berkshelf.enabled = true
  config.omnibus.chef_version = :latest

#  config.vm.provider "virtualbox" do |vb|
#  	vb.gui = true
#  end
  
  # Default is 2200..something, but port 2200 is used by forescout NAC agent.
  config.vm.usable_port_range= 2800..6000

  nodes.each do |prefix, (count, ip_start)|
    count.times do |i|
      hostname = "%s" % [prefix, (i+1)]

      config.vm.define "#{hostname}" do |box|
        box.vm.hostname = "#{hostname}.book"
        box.vm.network "forwarded_port", guest: 8080, host_ip: '127.0.0.1', host: 8080
        box.vm.network "forwarded_port", guest: 5000, host_ip: '127.0.0.1', host: 5000
        box.vm.network "forwarded_port", guest: 80,   host_ip: '127.0.0.1', host: 80
        box.vm.network "forwarded_port", guest: 22,   host_ip: '127.0.0.1', host: 3022
        box.vm.network "forwarded_port", guest: 35357, host_ip: '127.0.0.1', host: 35357
        

        box.vm.provider :virtualbox do |vbox|
          vbox.customize ["modifyvm", :id, "--memory", 2048]
          vbox.customize ["modifyvm", :id, "--cpus", 1]
          vbox.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"]
          vbox.customize ["modifyvm", :id, "--nicpromisc4", "allow-all"]
        end # box.vm virtualbox
      end # config.vm.define 
    end # count.times
  end # nodes.each
end # Vagrant.configure("2")