# https://codingbee.net/vagrant/vagrant-enabling-a-centos-vms-gui-mode

Vagrant.configure("2") do |config|
    # config.vm.box = "centos/7"
    # config.vm.box = "geerlingguy/centos7"
    config.vm.box = "generic/centos7"

    common = <<-SCRIPT
    sudo parted /dev/sda resizepart 2 100%
    sudo pvresize /dev/sda2
    sudo lvextend -l +100%FREE /dev/centos/root
    sudo xfs_growfs /dev/centos/root
    SCRIPT
    
    config.vagrant.plugins = ['vagrant-vbguest', 'vagrant-disksize', 'vagrant-reload']
    config.disksize.size = '100GB'
    
    # config.vm.define "node01" do |node1|
    #     # node1.vm.hostname = "node01"
    #     # node1.vm.network "private_network", ip: "192.168.56.121"
    #     config.vm.provision :shell, :inline => common
    # end

    config.vm.provider "virtualbox" do |vm|
        vm.gui = true
        vm.memory = 2048
        vm.cpus = 2
    end
    config.vm.network "public_network", use_dhcp_assigned_default_route: true
    config.vm.provision "shell", inline: "#{common}"
    config.vm.provision "shell", inline: "sudo yum groupinstall -y 'gnome desktop'"
    config.vm.provision "shell", inline: "sudo yum install -y 'xorg*'"
    config.vm.provision "shell", inline: "sudo yum remove -y initial-setup initial-setup-gui"
    config.vm.provision "shell", inline: "sudo systemctl isolate graphical.target"
    config.vm.provision "shell", inline: "sudo systemctl set-default graphical.target"
    config.vm.provision "shell", inline: "sudo yum install -y python3"
    config.vm.provision :reload
end
