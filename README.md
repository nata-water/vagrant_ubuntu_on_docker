# Vagrant Ubuntu 22.04 (Jammy) on Docker

## [1-1] 事前に必要なこと

* 環境変数の追加

- 変数名: VAGRANT_PREFER_SYSTEM_BIN
- 値: 0

## [1-2] (プロキシ環境のみ)事前に必要なこと

* 環境変数の追加

- 変数名: HTTP_PROXY
  - 値: http://user:password@proxyhost:port
- 変数名: HTTPS_PROXY
  - 値: http://user:password@proxyhost:port


* もしくはPowerShellで以下を実行
   
```powershell
> $env:HTTP_PROXY="http://user:password@proxyhost:port"
> $env:HTTPS_PROXY="http://user:password@proxyhost:port"
```

* vagrant-proxyconfのインストール

```shell
> vagrant plugin install vagrant-proxyconf
```

## [1-3] (共通)起動

```shell
$ vagrant up
```

## [1-4] (プロキシ環境のみ)起動後に必要なこと

* /etc/systemd/system/docker.serviceのExecStartより前の行にプロキシ情報を追記する
* 
```sh
$ sudo vim /etc/systemd/system/docker.service

Environment="HTTP_PROXY=http://user:password@proxyhost:port/"
Environment="HTTPS_PROXY=http://user:password@proxyhost:port/"

```

```sh
$ sudo systemctl daemon-reload && sudo systemctl restart docker
```

