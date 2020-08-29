# https://codingbee.net/vagrant/vagrant-enabling-a-centos-vms-gui-mode

Vagrant.configure("2") do |config|
    # config.vm.box = "centos/7"
    # config.vm.box = "geerlingguy/centos7"
    config.vm.box = "generic/centos7"

    generic = <<-SCRIPT
    sudo parted /dev/sda resizepart 2 100%
    sudo pvresize /dev/sda2
    sudo lvextend -l +100%FREE /dev/centos_centos7/root
    sudo xfs_growfs /dev/centos_centos7/root
    SCRIPT
    
    config.vagrant.plugins = ['vagrant-vbguest', 'vagrant-disksize', 'vagrant-reload']
    config.disksize.size = '100GB'


    config.vm.provider "virtualbox" do |vm|
        vm.gui = true
        vm.memory = 2048
        vm.cpus = 2
    end
    config.vm.network "public_network", use_dhcp_assigned_default_route: true
    config.vm.provision "shell", inline: "#{generic}"
    config.vm.provision "shell", inline: "sudo yum groupinstall -y 'gnome desktop'"
    config.vm.provision "shell", inline: "sudo yum install -y 'xorg*'"
    config.vm.provision "shell", inline: "sudo yum remove -y initial-setup initial-setup-gui"
    config.vm.provision "shell", inline: "sudo systemctl isolate graphical.target"
    config.vm.provision "shell", inline: "sudo systemctl set-default graphical.target"
    config.vm.provision "shell", inline: "sudo yum install -y python3"
    # disable selinux
    config.vm.provision "shell", inline: "sudo sed -i 's/.*SELINUX=.*/SELINUX=disabled/' /etc/selinux/config"
    # install lustre
    config.vm.provision "shell", inline: "sudo yum install wget -y"
    config.vm.provision "shell", inline: "sudo wget https://fsx-lustre-client-repo-public-keys.s3.amazonaws.com/fsx-rpm-public-key.asc -O /tmp/fsx-rpm-public-key.asc"
    config.vm.provision "shell", inline: "sudo rpm --import /tmp/fsx-rpm-public-key.asc"
    config.vm.provision "shell", inline: "sudo wget https://fsx-lustre-client-repo.s3.amazonaws.com/el/7/fsx-lustre-client.repo -O /etc/yum.repos.d/aws-fsx.repo"
    config.vm.provision "shell", inline: "sudo yum install -y kmod-lustre-client lustre-client"
    # config.vm.provision "shell", inline: "sudo yum update -y"
    config.vm.provision :reload
end
