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
  config.vm.box = "ubuntu/trusty64"
  opts = {
    create: true,
    type: 'rsync',
    rsync__exclude: ['.idea/', '.git/', '*.md'],
    rsync__args: ['--verbose', '--archive', '--delete', '-z']
  }
  
  config.vm.synced_folder ".", "/vagrant", opts
  config.vm.hostname = 'ubunut-14lts-os'

  if Vagrant.has_plugin?("vagrant-vbguest")
    config.vbguest.auto_update = false
  end 

  if Vagrant.has_plugin?("vagrant-proxyconf")
    #config.proxy.http = ""
    #config.proxy.https = ""
    config.proxy.no_proxy = "localhost,127.0.0.1"
  end

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
          vbox.customize ["modifyvm", :id, "--memory", 4056]
          vbox.customize ["modifyvm", :id, "--cpus", 2]
          vbox.customize ["modifyvm", :id, '--nictype1', 'virtio']
          vbox.customize ["modifyvm", :id, '--nictype2', 'virtio']
        end # box.vm virtualbox
      end # config.vm.define
    end # count.times
  end # nodes.each
  
  config.vm.provision :shell, :path => "/bin/sh /vagrant/install.sh"
end # Vagrant.configure("2")
