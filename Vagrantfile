hosts = {
  "Shop"   => { ip: "192.10.0.5", port: 2202 },
  "test01"   => { ip: "192.10.0.6", port: 2203 },
}

#add configuration
Vagrant.configure("2") do |config|
  hosts.each do |name, host_config|
    config.vm.define name do |machine|
      machine.vm.box = "ubuntu/trusty64"
      #machine.vm.hostname = "%s.linio-staging.com" % name
      machine.vm.network :private_network, ip: host_config[:ip]
      config.vm.network :forwarded_port, guest: 22, host: host_config[:port], id: "ssh", auto_correct: true
      machine.vm.provider "virtualbox" do |v|
          v.name = name
          v.customize ["modifyvm", :id, "--memory", 384]
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
