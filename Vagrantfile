#VM settings
VM_IP = "172.28.128.16"
VM_MEMORY_MB  = "1024" #1GB

Vagrant.configure(2) do |config|
  config.vm.box = "debian/buster64"  # https://app.vagrantup.com/debian/
  config.vm.define "jupyter" do |jupyter|
    jupyter.vm.provider "virtualbox" do |vb|
      vb.memory = VM_MEMORY_MB
    end
    jupyter.vm.network "private_network", ip: VM_IP
    jupyter.vm.network "forwarded_port", guest: 8888, host: 8888
    jupyter.vm.synced_folder ".", "/vagrant"
    jupyter.vm.provision "docker"
    jupyter.vm.provision :shell, inline: <<-SHELL
    #Installing vim because is sexy
    sudo apt-get install -y vim;
    echo 'syntax on' > ~/.vimrc;         #fixing issue with MobaXterm on vim
    echo 'set background=dark' >> ~/.vimrc;  #fixing issue with MobaXterm on vim
    cat /vagrant/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys;   #copy the key
    #Installing compose 
    if [ -z "$( which docker-compose )" ] ; then
      sudo apt-get update;
      sudo apt-get install -y python3-pip;
      sudo pip3 install -U docker-compose;
    fi
    cd /vagrant;
    #create directory
    sudo mkdir -p /vagrant/jupyter/work;
    sudo chown -R 1000 /vagrant/jupyter/work;
    docker-compose up --detach;
    #show the url or at least where to find it
    sleep 10;
    docker-compose logs | tail -10f; 
    SHELL
  end
end
