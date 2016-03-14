
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "centos/7"

  requireHostmanager(config)
  # Create 2 machines they will be hdp-master01 and hdp-worker01
  config.vm.define "hdp-master01" do |master|
    master.vm.network "private_network", type: "dhcp"
    master.vm.hostname = 'hdp-master01.hdp.com'

    # Bring up 8080 on 18080
    master.vm.network :forwarded_port, guest: 8080, host: 18080, auto_correct: true

    # Give this machine 2G ram and 2 cores
    master.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "2048"]
      vb.customize ["modifyvm", :id, "--cpus", "2"]
    end
  end

  config.vm.define "hdp-worker01" do |worker|
    worker.vm.network "private_network", type: "dhcp"
    worker.vm.hostname = 'hdp-worker01.hdp.com'

    # Give this machine 2G ram and 2 cores
    worker.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "2048"]
      vb.customize ["modifyvm", :id, "--cpus", "2"]
    end
  end


  # Use ansible as the provisioning system
  config.vm.provision "ansible" do |ansible|
    # All machines at once
    ansible.limit = "all"
    ansible.groups = {
      "masters" => ['hdp-master01'],
      "workers" => ['hdp-worker01'],
      "ambari-master" => ['hdp-master01'],
      "hdp:children" => ['masters', 'workers']
    }
    ansible.playbook = "hdp.yml"
    ansible.extra_vars = { ansible_ssh_user: 'vagrant', vagrant: 'true', ambari_master: 'hdp-master01.hdp.com' }
  end

end


# This function ensures the vagrant plugins are installed and correctly configured.
def requireHostmanager(config)
  
  # Check on Vagrant plugins
  if not Vagrant.has_plugin?("vagrant-vbguest")
    abort('\n-----\n\nThis Vagrantfile depends on the vbguest plugin, but it is not installed, please install using\n'\
    'vagrant plugin install vagrant-vbguest\n\n-----\n\n' )
  end

  if not Vagrant.has_plugin?("vagrant-hostmanager")
    abort('\n-----\n\nThis Vagrantfile depends on the hostmanager plugin, but it is not installed, please install using\n'\
    'vagrant plugin install vagrant-hostmanager\n\n-----\n\n' )
  end
  config.hostmanager.enabled = true
  
    cached_addresses = {}
    config.hostmanager.ip_resolver = proc do |vm, resolving_vm|
      if cached_addresses[vm.name].nil?
        if hostname = (vm.ssh_info && vm.ssh_info[:host])
          # print ("VBoxManage guestproperty get #{vm.id} /VirtualBox/GuestInfo/Net/1/V4/IP")
          ip = `VBoxManage guestproperty get #{vm.id} /VirtualBox/GuestInfo/Net/1/V4/IP`
          if ip == 'No value set!'
            ip = `VBoxManage guestproperty get #{vm.id} "/VirtualBox/GuestInfo/Net/0/V4/IP"`
          end
          # print ( "Found an ip of:" + ip )
          cached_addresses[vm.name] = ip.split()[1]
        end
      end
      # print "===== Setting IP for #{vm.name} (#{vm.id})to #{cached_addresses[vm.name]}\n"
      # print "\tCache: #{cached_addresses}\n"
      cached_addresses[vm.name]
    end

end


# -*- mode: ruby -*-
# vi: set ft=ruby :
