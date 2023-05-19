# -*- mode: ruby -*-
# # vi: set ft=ruby :

Vagrant.configure("2") do |config|    
    config.vagrant.plugins = "vagrant-reload"
    config.vm.define "lab-dc1" do |subconfig|
        subconfig.vm.box = "gusztavvargadr/windows-server"
        subconfig.vm.hostname = "lab-dc1"
        subconfig.vm.provider :virtualbox do |vb|
            vb.gui = false
        end
        subconfig.vm.network "private_network", ip: "192.168.11.2", 
            virtualbox__intnet: true
        subconfig.winrm.username = "vagrant"
        subconfig.winrm.password = "vagrant"
        subconfig.winrm.transport = :plaintext
        subconfig.winrm.basic_auth_only = true 
        #Install DSC Modules
        subconfig.vm.provision "shell",
            path: "provision\\PreDSCInstall.ps1"
        #https://github.com/dsccommunity/ActiveDirectoryDsc
        subconfig.vm.provision "shell",
            path: "provision\\DSCInstallDomain.ps1"
        #Restart VM to finish Active Directory Domain Services installation
        subconfig.vm.provision :reload
        #https://github.com/dsccommunity/xDhcpServer
        subconfig.vm.provision "shell",
            path: "provision\\DSCInstallDHCP.ps1"
    end
      
    config.vm.define "lab-test" do |subconfig|
        subconfig.vm.box = "gusztavvargadr/windows-server"
        subconfig.vm.hostname = "lab-test"
        subconfig.vm.provider :virtualbox do |vb|
            vb.gui = false
        end
        subconfig.vm.network "private_network", ip: "127.0.0.2", 
            auto_config: false,
            virtualbox__intnet: true
        subconfig.winrm.username = "vagrant"
        subconfig.winrm.password = "vagrant"
        subconfig.winrm.transport = :plaintext
        subconfig.winrm.basic_auth_only = true
        subconfig.vm.provision "shell",
            path: "provision\\ConfigureRebootOnNode.ps1"        
        subconfig.vm.provision "shell",
            path: "provision\\PreDSCInstall.ps1"
        subconfig.vm.provision "shell",
            path: "provision\\DSCJoinDomain.ps1"
    end
end
