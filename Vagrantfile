# -*- mode: ruby -*-
# vi: set ft=ruby :
#
# Setting up ubuntu 14 image

VAGRANTFILE_API_VERSION = "2"

nodes = {
 'keystone'   => [1, 201]
}

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config| 
  # Virtualbox
  config.vm.box = "Ubuntu Server 14.04 LTS"
  config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-i386-vagrant-disk1.box"
  config.vm.synced_folder ".", "/vagrant", type: "nfs"
  config.vm.hostname = 'ubunut-14lts-os'
  config.vm.provision :shell, :path => "install.sh"

  config.berkshelf.berksfile_path = "Berksfile"
  config.berkshelf.enabled = true
  config.omnibus.chef_version = :latest
  config.vm.usable_port_range= 2200..6000

  nodes.each do |prefix, (count, ip_start)|
    count.times do |i|
      hostname = "%s" % [prefix, (i+1)]

      config.vm.define "#{hostname}" do |box|
        box.vm.hostname = "#{hostname}.book"
        box.vm.network "forwarded_port", guest: 8080,  host_ip: '127.0.0.1', host: 8080
        box.vm.network "forwarded_port", guest: 80,    host_ip: '127.0.0.1', host: 80
        box.vm.network "forwarded_port", guest: 5000,  host: 5000
        box.vm.network "forwarded_port", guest: 35357, host: 35357
        

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