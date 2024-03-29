# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "bento/ubuntu-22.04"

  config.vm.network "forwarded_port", guest: 80, host: 80
  # 開放したいポート範囲を指定します
  from_port = 8888
  to_port = 8888
  for i in from_port..to_port
    config.vm.network "forwarded_port", guest: i, host: i
  end
  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"

  config.vm.synced_folder "./synced_folder", "/vagrant_data"

  # メモリ設定
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
  end

  # プロキシ設定
  if Vagrant.has_plugin?("vagrant-proxyconf") && ENV["HTTP_PROXY"]
      config.proxy.http     = ENV["HTTP_PROXY"]
      config.proxy.https    = ENV["HTTP_PROXY"]
      config.proxy.no_proxy = "localhost,127.0.0.1"
  end

  # 仮想環境プロビジョニング
  config.vm.provision "shell", inline: <<-SHELL
    # アップデート
    sudo apt-get update -y && sudo apt-get upgrade -y
    # Dockerに必要なモジュールのインストール
    sudo apt-get install -y \
         ca-certificates \
         curl \
         gnupg \
         lsb-release \
         apt-transport-https \
         make
    # GPGキー取得
    curl -fsSL -k https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    # リポジトリ情報追加
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update -y 
    # Dockerインストール
    sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
    # vagrantユーザをDockerグループに追加
    sudo usermod -g docker vagrant
    # docker-composeのインストール
    sudo curl -L -k https://github.com/docker/compose/releases/download/v2.10.0/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
    sudo chmod +x /usr/local/bin/docker-compose
    # Docker再起動
    sudo systemctl daemon-reload
    sudo systemctl restart docker
  SHELL

end
