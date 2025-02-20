# Rails7の環境をherokuにパパッと作るためのテンプレート

## 前提
[dip](https://github.com/bibendi/dip)のインストール
herokuのアカウント作成、設定等の準備（無料版無くなってます。[月5＄からの課金が必要](https://jp.heroku.com/pricing)）(無料のホスティングを求めて放浪するのは疲れたんだ...)

## ローカルの準備

### このソースコードをDLして、自分の別のリポジトリとして登録しておく

### プロジェクトの新規作成
```
dip rails new . --force --database=postgresql
docker compose build web --no-cache
```

### DB
```
dip rails db:create
dip rails db:migrate
```

### database.ymlの書き換え
```
default: &default
  adapter: postgresql
  encoding: unicode
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  host: db
  username: postgres
  password: password

development:
  <<: *default
  database: myapp_development

test:
  <<: *default
  database: myapp_test

production:
  <<: *default
  database: myapp_production
```

### サーバー起動
```
docker-compose up
```

# 以降はデプロイ用

### ルートに表示するページを仮で用意
```ruby:route.rb
Rails.application.routes.draw do
  root "home#index"
end
```

dip shellでコンテナ内に入りコマンドを実行
```
rails generate controller Home
```

```ruby:home_controller.rb
class HomeController < ApplicationController
  def index
    render plain: 'Hello World!'
  end
end
```

## heroku
### アプリの作成
```
heroku login
heroku create myapp
```

### DB設定
```
heroku addons:create heroku-postgresql:mini -a myapp
```

### 環境変数の設定
```
heroku config:set RAILS_ENV=production -a myapp
```

## herokuにデプロイ
[手順](https://devcenter.heroku.com/ja/articles/build-docker-images-heroku-yml)
参考記事：https://zenn.dev/takuty/articles/b7aa6164fc85bb

# herokuよく使うコマンド
## deploy

```
heroku container:push web -a myapp && heroku container:release web -a myapp
```

をdipコマンドにしました

```
dip deploy
```

## コマンド実行

```
heroku run bash
```

## check log

```
heroku logs --tail
```

## access db

```
 heroku pg:psql
```

## reset db

```
heroku pg:reset -a myapp
heroku run rails db:migrate
```

