#!/usr/bin/env ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :
ENV['VAGRANT_DEFAULT_PROVIDER'] = "virtualbox"
require "yaml"
machines = YAML.load_file(Dir.pwd+"/.superbock.yml")
Vagrant.configure(2) do |config|
  config.vm.box = "debian/jessie64"
  config.ssh.insert_key = false

  $ansible_groups = {
    "all_groups:children" => []
  }
  
  config.vm.provision "ansible" do |ansible|
    ansible.limit= "all"
    ansible.verbose= "vvv"
    ansible.playbook = File.dirname(__FILE__)+"/local.yml"
  end

  machines.each_with_index do |machine, index|
    config.vm.define machine["name"].to_sym do |serv|
      if $ansible_groups[machine['group']].nil?
        $ansible_groups[machine['group']] = []
        $ansible_groups["all_groups:children"] << machine['group']
      end
      $ansible_groups[machine['group']] << machine['name']
      
      serv.vm.provider :virtualbox do |provider|
          provider.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      end
      serv.vm.hostname = machine['name']
      serv.vm.provision "ansible" do |ansible|
        ansible.groups = $ansible_groups
        ansible.playbook = File.dirname(__FILE__)+"/#{machine['group']}.yml"
      end
      #serv.vm.synced_folder "./vsd-bo", "/var/www/admin.vsd.dev/current", :nfs => true, :mount_options => ['nolock,vers=3,udp']
    end
  end
end