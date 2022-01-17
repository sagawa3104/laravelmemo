# Laravelプロジェクトの作成

## プロジェクトの作成

wsl内で以下のコマンドを打つ

```shell
cd ~
curl -s "https://laravel.build/{プロジェクト名}" | bash
cd {プロジェクト名} && ./vendor/bin/sail up
```

`sail up`コマンドでlaravelプロジェクトが連携するサービスコンテナが一式立ち上がる。
デフォルトでは以下のサービスが起動する。

| サービス名 | 役割 |
|:-|:-|
|mysql|リレーショナルDB|
|redis|キャッシュ用DB|
|selenium|画面テスト用？|
|meilisearch|検索エンジン|
|mailhog|メールサーバ|

`laravel.test`はPHPの機能によるビルトインWebサーバ(ApacheやNginx)のようなもの

特定のサービスのみ連携する場合、プロジェクト作成時のURLにhttps://laravel.build/{プロジェクト名} に以下のようにクエリパラメータを追加する。

```shell
# mysql、redisのみ使用する場合
curl -s "https://laravel.build/{プロジェクト名}?with=mysql,redis" | bash
```

もしくは`sail up -d`でコンテナをバックグラウンドで実行した後に使用するサービスを指定し直し、コンテナをdownする。

```shell
# コンテナ起動
sail up -d
# sailコマンド経由でコンテナ内のartisanコマンドを使い、sailが連携するサービスを列挙し直す
sail artisan sail:install --with=mysql,redis
# 今のコンテナを終了し破棄する（stopだとコンテナは残るため、列挙していないサービスコンテナが残り続ける
sail down
# コンテナ再起動
sail up -d
```

1. [参考](https://readouble.com/laravel/8.x/ja/installation.html)
1. [参考](https://readouble.com/laravel/8.x/ja/sail.html)

## 既存Laravelプロジェクトから環境構築する場合(git cloneしてくる場合など)

git上で管理されるLaravelプロジェクトの資産にはcomposerで管理されるライブラリは含まれない(venodrディレクトリが存在しない)ため、sailコマンドが使えない。
WSL内にphpやcomposer環境を用意するか、[こちら](https://readouble.com/laravel/8.x/ja/sail.html#:~:text=%E6%97%A2%E5%AD%98%E3%82%A2%E3%83%97%E3%83%AA%E3%82%B1%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E3%81%A7Composer%E4%BE%9D%E5%AD%98%E9%96%A2%E4%BF%82%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)を参考に使い捨てのdockerコンテナを利用してcomposerライブラリを用意する。

## VScodeでサービスコンテナ内にリモート接続する

[ここ](https://docs.microsoft.com/ja-jp/windows/wsl/tutorials/wsl-containers)と[ここ](https://readouble.com/laravel/8.x/ja/sail.html#:~:text=bin/sail%20up-,Devcontainer,-%E3%81%AE%E4%BD%BF%E7%94%A8)を参考に以下のコマンドを打ってVScodeを起動する。

```shell
sail -up -d
sail artisan sail:install --devcontainer
code .
```

プロジェクトディレクトリでVScodeを開いたら、`Ctrl + Shift + p`でコマンドパレットを開き、「Remote-Containers: Open Folder in Container...」を選択し、現在のプロジェクトディレクトリを選択する。
