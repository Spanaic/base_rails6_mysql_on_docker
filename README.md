# Rails6 + MySQL on docker の環境構築

1. リポジトリをクローンして新しい rails プロジェクトを作成

```sh
mkdir your_app_name
cd your_app_name
git clone this repository
docker-compose run web rails new . --force --no-deps --database=mysql --skip-test --webpacker
docker-compose build
```

2. database.yml の設定を変更

```database.yml
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: <%= ENV.fetch("MYSQL_USERNAME", "root") %>
  password: <%= ENV.fetch("MYSQL_PASSWORD", "password") %>
  host: <%= ENV.fetch("MYSQL_HOST", "db") %>

development:
  <<: *default
  database: myapp_development

test:
  <<: *default
  database: myapp_test

production:
  <<: *default
  database: myapp_production
  username: myapp
  password: <%= ENV['MYAPP_DATABASE_PASSWORD'] %>
```

3. DB の作成後、コンテナを起動して接続チェックで完了

```sh
docker-compose run web rake db:create
docker-compose up
```
