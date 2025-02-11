Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu24" 
    config.vm.provision "shell", inline: <<-SHELL
    echo "upgrading package ..."
    apt-get update -y
    apt-get upgrade -y
    echo "installing iptables-persistent and bash-completion..."
    DEBIAN_FRONTEND=noninteractive apt-get install -y iptables-persistent bash-completion curl wget 

    echo "Copying iptables rules..."
    cp /vagrant/rules.v4 /etc/iptables/rules.v4
    echo "Copying issue file..."
    cp /vagrant/issue /etc/issue

    echo "Applying iptables rules..."
    iptables-restore < /etc/iptables/rules.v4

    echo "Restarting iptables-persistent service..."
    systemctl restart netfilter-persistent
  SHELL
  
    # Define the first VM (controller)
    config.vm.define "controller" do |controller|
      controller.vm.hostname = "controller"
      controller.vm.network "private_network", ip: "192.168.10.101"
      # controller.vm.network "public_network", bridge: "wifi"
      controller.vm.provision "shell", inline: <<-SHELL
        echo "Updating package list and installing python packages..."
        apt-get update -y
        DEBIAN_FRONTEND=noninteractive apt-get install -y python3-pip python3-venv 
      SHELL
      controller.vm.provider "virtualbox" do |vb|
        vb.memory = "1024"
        vb.cpus = 1
        vb.name = 'Controller'
      end      
    end  
     # Define the second VM
 
    config.vm.define "node" do |node|
      node.vm.hostname = "node"
      node.vm.network "private_network", ip: "192.168.10.102"
      # node.vm.network "public_network", bridge: "wifi"
      node.vm.provider "virtualbox" do |vb|
        vb.memory = "1024"
        vb.cpus = 1
        vb.name = 'node'    
      end
    end
  end
  