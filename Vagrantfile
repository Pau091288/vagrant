hosts = {
  "Shop"   => { ip: "192.10.0.5"},
  "b4a"   => { ip: "192.10.0.6"},
}

Vagrant.configure("2") do |config|
  hosts.each do |name, host_config|
    config.vm.define name do |machine|
      machine.vm.box = "ubuntu/trusty64"
      #machine.vm.hostname = "%s.linio-staging.com" % name
      machine.vm.network :private_network, ip: host_config[:ip]
      #config.vm.network :forwarded_port, guest: 22, host: host_config[:port], id: "ssh", auto_correct: true
    
      machine.vm.provider "virtualbox" do |v|
          v.name = name
          v.customize ["modifyvm", :id, "--memory", 3000]
      end
       
      machine.vm.provision "ansible" do |ansible|
        #ansible.groups= {
        #  'web' => ['Shop','BFAlice','Front']
        #}
        #ansible.sudo= true
        ansible.inventory_path= "inventories/host.ini"
        ansible.verbose= "v"
        ansible.playbook= "playbooks/package.yml"
       end
    end
  end
end
