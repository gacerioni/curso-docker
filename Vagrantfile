# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|


  config.vm.box = "ubuntu/xenial64"
  config.vm.box_check_update = true
  config.vm.network "public_network", bridge: "wlp4s0"

  config.vm.provider "virtualbox" do |vb|
   vb.memory = "1024"
  end

  (1..2).each do |i|
    config.vm.define "docker#{i}" do |node|
      node.vm.hostname = "docker#{i}"
    end
  end

  config.vm.define "storage" do |node|
    node.vm.hostname = "storage"
    node.vm.provision "shell", inline: <<-SHELL
      wget https://dl.minio.io/server/minio/release/linux-amd64/minio -O /usr/local/bin/minio
      chmod +x /usr/local/bin/minio
      #/usr/local/bin/minio server /mnt/data &
      wget https://dl.minio.io/client/mc/release/linux-amd64/mc -O /usr/local/bin/mc
      chmod +x /usr/local/bin/mc   
      apt-get -f install nfs-kernel-server -y
      echo "/mnt/nfs *(rw,async,no_subtree_check) > /etc/exports"
      service nfs-kernel-server restart
      exportfs
    SHELL
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get clean
    apt-get update
    apt-get remove docker docker-engine docker.io -y
    curl -fsSL get.docker.com -o get-docker.sh
    bash get-docker.sh
    apt-get -f install s3fs -y
  SHELL
end
