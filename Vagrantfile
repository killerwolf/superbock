#!/usr/bin/env ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :
ENV['VAGRANT_DEFAULT_PROVIDER'] = "virtualbox"
require "yaml"
machines = YAML.load_file(Dir.pwd+"/.superbock.yml")
Vagrant.configure(2) do |config|
  config.vm.box = "debian/jessie64"
  machines.each_with_index do |machine, index|
    config.vm.define machine["name"].to_sym do |serv|
      serv.vm.provider :virtualbox do |provider|
          provider.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      end
      serv.vm.hostname = machine['name']
      #serv.vm.synced_folder "./vsd-bo", "/var/www/admin.vsd.dev/current", :nfs => true, :mount_options => ['nolock,vers=3,udp']
    end
  end
end