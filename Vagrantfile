Vagrant.configure("2") do |config|
  
  config.vm.box = "gusztavvargadr/ubuntu-server-2204-lts"

  config.vm.define "master" do |master|
    master.vm.hostname = "master"
    master.vm.network "private_network", ip: "192.168.50.100"
    master.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
      vb.cpus = 2
    end
    master.vm.provision "shell", inline: <<-SHELL
        sudo apt update -y
        sudo apt install vim git -y
        cd ~
        git clone https://github.com/sandervanvugt/cka
        cd cka/
        sudo ./setup-container.sh
        sudo ./setup-kubetools.sh
    SHELL
  end

  (1..2).each do |i|
    config.vm.define "worker#{i}" do |worker|
      worker.vm.hostname = "worker#{i}"
      worker.vm.network "private_network", ip: "192.168.50.10#{i}"
      worker.vm.provider "virtualbox" do |vb|
        vb.memory = "1024"
        vb.cpus = 1
      end
      worker.vm.provision "shell", inline: <<-SHELL
        sudo apt update -y
        sudo apt install vim git -y
        cd ~
        git clone https://github.com/sandervanvugt/cka
        cd cka/
        sudo ./setup-container.sh
        sudo ./setup-kubetools.sh
        mkdir -p $HOME/.kube
    SHELL
    end
  end
end

