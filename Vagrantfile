# https://codingbee.net/vagrant/vagrant-enabling-a-centos-vms-gui-mode

Vagrant.configure("2") do |config|
    config.vm.box = "centos/7"
    config.vagrant.plugins = ['vagrant-vbguest', 'vagrant-disksize', 'vagrant-reload']
    config.vm.provider "virtualbox" do |v|
        v.gui = true
        v.memory = 2048
        v.cpus = 2
        
    end
    config.disksize.size = "65000MB"
    config.vm.network "public_network", use_dhcp_assigned_default_route: true
    config.vm.provision "shell", inline: "sudo yum groupinstall -y 'gnome desktop'"
    config.vm.provision "shell", inline: "sudo yum install -y 'xorg*'"
    config.vm.provision "shell", inline: "sudo yum remove -y initial-setup initial-setup-gui"
    config.vm.provision "shell", inline: "sudo systemctl isolate graphical.target"
    config.vm.provision "shell", inline: "sudo systemctl set-default graphical.target"
    config.vm.provision "shell", inline: "sudo yum install -y python3"
    # disable selinux
    config.vm.provision "shell", inline: "sudo sed -i 's/.*SELINUX=.*/SELINUX=disabled/' /etc/selinux/config"
    # install lustre
    config.vm.provision "shell", inline: "sudo yum update -y"
    config.vm.provision "shell", inline: "sudo yum install wget -y"
    config.vm.provision "shell", inline: "sudo wget https://fsx-lustre-client-repo-public-keys.s3.amazonaws.com/fsx-rpm-public-key.asc -O /tmp/fsx-rpm-public-key.asc"
    config.vm.provision "shell", inline: "sudo rpm --import /tmp/fsx-rpm-public-key.asc"
    config.vm.provision "shell", inline: "sudo wget https://fsx-lustre-client-repo.s3.amazonaws.com/el/7/fsx-lustre-client.repo -O /etc/yum.repos.d/aws-fsx.repo"
    config.vm.provision "shell", inline: "sudo yum install -y kmod-lustre-client lustre-client"
    config.vm.provision :reload
end
