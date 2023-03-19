# 環境構築手順

## ローカルにcloneしてdockerを起動

````
$ git clone git@github.com:DaigoHiroi/kaiyaku.git
$ cd kaiyaku
$ docker compose up -d
````

## appコンテナでlaravelインストール

````
[mac] $ docker compose exec app bash
````
書き込み権限がないとキャッシュやログにエラーを書き込めないので、権限を付与

````
[app] $ chmod -R 777 storage bootstrap/cache
````

````
[app] $ composer install
````

## appコンテナでその他設定

envをコピー、APP_KEY= の値がないとエラーになるため作成。
シンボリックリンクを貼り、migrate
````
[app] $ cp .env.example .env
[app] $ php artisan key:generate
Application key set successfully.
[app] $ php artisan storage:link
[app] $ php artisan migrate
````
