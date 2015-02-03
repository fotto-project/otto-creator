# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

Vagrant.configure(2) do |config|
  config.vm.box = "puphpet/ubuntu1404-x32"
  config.vm.network "public_network", bridge: 'en1'
  config.vm.provider "virtualbox" do |v|

    v.gui = false

    host = RbConfig::CONFIG['host_os']

    # via https://github.com/btopro/elmsln-vagrant/blob/master/Vagrantfile#L33
    # 1/4 system memory & all cpu cores
    if host =~ /darwin/
      cpus = `sysctl -n hw.ncpu`.to_i
      mem = `sysctl -n hw.memsize`.to_i / 1024 / 1024 / 8
    elsif host =~ /linux/
      cpus = `nproc`.to_i
      mem = `grep 'MemTotal' /proc/meminfo | sed -e 's/MemTotal://' -e 's/ kB//'`.to_i / 1024 / 4
    else # Windows ???
      cpus = 2
      mem = 1024
    end

    v.customize ["modifyvm", :id, "--memory", mem]
    v.customize ["modifyvm", :id, "--cpus", cpus]
    v.customize ["modifyvm", :id, "--ioapic", "on"]

  end

  config.vm.provider "parallels" do |v|
    v.update_guest_tools = true
    v.optimize_power_consumption = false

    host = RbConfig::CONFIG['host_os']

    # via https://github.com/btopro/elmsln-vagrant/blob/master/Vagrantfile#L33
    # 1/4 system memory & all cpu cores
    if host =~ /darwin/
      cpus = `sysctl -n hw.ncpu`.to_i
      mem = `sysctl -n hw.memsize`.to_i / 1024 / 1024 / 8
    elsif host =~ /linux/
      cpus = `nproc`.to_i
      mem = `grep 'MemTotal' /proc/meminfo | sed -e 's/MemTotal://' -e 's/ kB//'`.to_i / 1024 / 4
    else # Windows ???
      cpus = 2
      mem = 1024
    end
    v.memory = mem
    v.cpus = cpus
  end

  config.vm.synced_folder '.', "/stak/sdk", type: 'nfs'
# config.vm.synced_folder "scripts", "/kernel_builder", owner: "root", group: "root"
config.vm.provision "shell", path: "provision.sh"
end