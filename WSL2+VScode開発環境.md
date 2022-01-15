# WSL2 + VScode開発環境

## WSL2インストール

1. WSL2インストール

2. ディストリビューションインストール

[参考](https://docs.microsoft.com/ja-jp/windows/wsl/setup/environment)

`wsl --install` でWSL2を有効化する場合、規定ディストリビューションでubuntuがインストールされる。  
`wsl --list --online` でオンラインインストール可能なディストリビューションが表示される。(上気規定のディストリビューションは無印のubuntu)

## dockerインストール

1. dockerインストール

2. wslとの連携
[参考](https://docs.microsoft.com/ja-jp/windows/wsl/tutorials/wsl-containers)

dockerDesktop->setting->resource->WSL Integration にWSL内にインストールしたディストリビューションが表示されない場合、そのディストリビューションがWSL versionが1の場合がある。  
`wsl --list --version` で確認し、1の場合は、`wsl --set-version <distro> <version>`で2を指定して、dockerDesktopの先ほどの設定画面をRefleshすると表示される。

## wsl内の環境構築

PowerShellで`wsl`コマンドを打つことでディストリビューション内に接続でき、内部ではLinuxコマンドを使って操作する。

```shell
# 更新
sudo apt-get update
# git拡張機能のインストール
sudo apt-get install git-flow
```

## VScodeでwsl内に接続する

WSL内で`code .`で、VScodeがwsl内に接続した状態で起動する。
