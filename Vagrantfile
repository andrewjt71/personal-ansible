Vagrant.require_version ">= 1.5"

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false
    config.vm.provider :virtualbox do |v|
        v.name = "template-vm"
        v.customize [
            "modifyvm", :id,
            "--name", "template-vm",
            "--memory", 512,
            "--natdnshostresolver1", "on",
            "--cpus", 1,
        ]
    end

    config.vm.box = "centos/7"

    config.vm.network :private_network, ip: "192.168.33.99"
    config.ssh.forward_agent = false

    config.vm.synced_folder '/Users/andrewtaylor/Code/personal-website', '/var/www/prod/personal-website', nfs:true
end
