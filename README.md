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
<!-- [app] $ php artisan migrate -->
````


### React, SemanticUI導入
````
# appコンテナに入って、UIパッケージ導入
$ docker-compose exec app composer require laravel/breeze --dev
$ docker-compose exec app php artisan breeze:install react --ssr

# appコンテナに入って、React.js導入　--authで認証機能も追加という意味
$ docker-compose exec app php artisan ui react --auth

# appコンテナに入って、TypeScript導入
$ docker-compose exec app npm install ts-loader typescript react-router-dom @types/react @types/react-dom @types/react-router-dom --save-dev

# appコンテナに入って、Semantic UI導入
$ docker-compose exec app npm install semantic-ui-react semantic-ui-css

# すべてインストール
$ docker-compose exec app npm install

$ php artisan migrate

#tsconfigファイルを作成
$ docker-compose exec app touch tsconfig.json

#中身を書く。

# ビルドはコンテナではなく、src配下で実行する。
$ npm run dev

使うとき
import 'semantic-ui-css/semantic.min.css'
````
