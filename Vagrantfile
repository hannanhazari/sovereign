# -*- mode: ruby -*-

Vagrant.configure('2') do |config|
  config.vm.hostname = 'sovereign.local'
  config.vm.network 'private_network', ip: '172.16.100.2'

  config.vm.provision "shell", inline: <<-SHELL
    apt-get install -y \
      python \
      python-apt
  SHELL

  config.vm.provision :ansible do |ansible|
    ansible.playbook = 'site.yml'
    ansible.host_key_checking = false
    ansible.extra_vars = { ansible_ssh_user: 'vagrant', testing: true }
    ansible.groups = {
      "testing" => ["buster", "bionic"]
    }

    # ansible.tags = ['blog']
    # ansible.skip_tags = ['openvpn']
    # ansible.verbose = 'vvvv'
  end

  config.vm.provider :virtualbox do |v|
    v.memory = 512
  end

  config.vm.provider :vmware_fusion do |v|
    v.vmx['memsize'] = '512'
  end

  # vagrant-cachier
  #
  # Install the plugin by running: vagrant plugin install vagrant-cachier
  # More information: https://github.com/fgrehm/vagrant-cachier
  if Vagrant.has_plugin? 'vagrant-cachier'
    config.cache.enable :apt
    config.cache.scope = :box
  end

  # Debian 10.2 64-bit (officially supported)
  config.vm.define 'buster', primary: true do |buster|
    buster.vm.box = 'debian/contrib-buster64'
  end

  # Ubuntu 18.04 (LTS) 64-bit (currently unavailable)
  config.vm.define 'bionic', autostart: false do |bionic|
    bionic.vm.box = 'ubuntu/bionic64'
  end
end
