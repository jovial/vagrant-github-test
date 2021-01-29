# This guide is optimized for Vagrant 1.7 and above.
# Although versions 1.6.x should behave very similarly, it is recommended
# to upgrade instead of disabling the requirement below.
Vagrant.require_version ">= 1.7.0"

Vagrant.configure(2) do |config|

  cluster_name = "testohpc"

  config.vm.define "#{cluster_name}-login-0" do |login|
    login.vm.hostname = "#{cluster_name}-login-0"
    login.vm.box = "centos/8"
    login.vm.network "private_network", ip: "192.168.56.10"
    login.vm.provision :hosts, :sync_hosts => true
    login.ssh.host = "192.168.56.10"
    login.ssh.port = "22"
  end

  config.vm.define "#{cluster_name}-control-0" do |control|
    control.vm.hostname = "#{cluster_name}-control-0"
    control.vm.box = "centos/8"
    control.vm.network "private_network", ip: "192.168.56.11"
    control.vm.provision :hosts, :sync_hosts => true
    control.ssh.host = "192.168.56.11"
    control.ssh.port = "22"
  end

  (0..2).each do |i|
    ip = "192.168.56.#{12 + i}"
    config.vm.define "#{cluster_name}-compute-#{i}" do |node|
        config.vm.provider "virtualbox" do |vb|
          vb.memory = 4096
          vb.cpus = 2
         end
      node.vm.hostname = "#{cluster_name}-compute-#{i}"
      node.vm.box = "centos/8"
      node.vm.network "private_network", ip: ip
      node.vm.provision :hosts, :sync_hosts => true
      node.ssh.host = ip
      node.ssh.port = "22"
    end
  end

  # Disable the new default behavior introduced in Vagrant 1.7, to
  # ensure that all Vagrant machines will use the same SSH key pair.
  # See https://github.com/mitchellh/vagrant/issues/5005
  config.ssh.insert_key = false

end
