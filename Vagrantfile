VAGRANTFILE_API_VERSION = '2'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = 'ubuntu/trusty64'

  config.vm.define 'use-chef-docker' do |v|
    v.vm.hostname = 'chefdk-container'
    v.vm.provider 'virtualbox' do |vb|
      vb.customize ['setextradata', 'global', 'GUI/SuppressMessages', 'all' ]
      vb.customize ['modifyvm', :id, '--ioapic', 'on']
      vb.cpus = 1
      vb.memory = 1024
      vb.linked_clone = true
    end
    v.vm.network 'private_network', ip: '172.20.20.10'
    v.vm.provision "docker" do |d|
      d.pull_images 'chef/chefdk'
    end
  end
end
