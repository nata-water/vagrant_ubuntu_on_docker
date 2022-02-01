# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/hirsute64"

  config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"


  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"

  config.vm.synced_folder "./synced_folder", "/vagrant_data"

  #
  config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
    vb.memory = "4096"
  end
  # config.ssh.insert_key = false



  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    # アップデート
    sudo apt-get update -y && sudo apt-get upgrade -y
    # Dokcerに必要なモジュールのインストール
    sudo apt-get install \
         ca-certificates \
         curl \
         gnupg \
         lsb-release \
         apt-transport-https
    # GPGキー取得
    curl -fsSL -k https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

    # リポジトリ情報追加
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

    # Dokcerインストール
    sudo apt-get install -y docker-ce docker-ce-cli containerd.io

    # vagrantユーザをDockerグループに追加
    sudo usermod -g docker vagrant

    # docker-composeのインストール
    sudo curl -L -k https://github.com/docker/compose/releases/download/1.16.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
    sudo chmod +x /usr/local/bin/docker-compose

    # Docker再起動
    sudo systemctl daemon-reload
    sudo systemctl restart docker
  SHELL
  end
