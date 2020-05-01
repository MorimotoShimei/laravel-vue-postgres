
# Laravel + vue + Postgres

[元ネタ](https://qiita.com/MasahiroHarada/items/2597bd6973a45f92e1e8)

## 変更点

- Laravel 7向けに修正
- ファイルの格納先をローカルディスクに変更

## 実行

envをコピー

```sh
cp web/.env.example web/.env
```

docker-composeを起動

```sh
docker-compose up -d
```

コンテナ内に入る

```sh
docker-compose exec vuesplash_web bash
```

モジュールのインストール、DBにテーブルを構築

```sh
# コンテナ内で実行
chmod 770 -R storage/
chmod 770 -R bootstrap/cache/
composer install
npm install
npm run dev
php artisan migrate
```

サーバを起こす

```sh
# コンテナ内で実行
php artisan serve --host 0.0.0.0 --port 8081
```

## 素から構築する(参考に残しておく)

docker-composeを起動

```sh
docker-compose up -d
```

コンテナ内に入る

```sh
docker-compose exec vuesplash_web bash
```

Laravel プロジェクトを作成

```sh
# コンテナ内で実行
composer create-project --prefer-dist laravel/laravel .
composer require laravel/ui
chmod 770 -R storage/
chmod 770 -R bootstrap/cache/
npm run install -D vue vuex vue-router
```

サーバを起こす

```sh
# コンテナ内で実行
php artisan serve --host 0.0.0.0 --port 8081
```

### 参考：ホストから実行する場合

```sh
docker-compose exec vuesplash_web php artisan serve --host 0.0.0.0 --port 8081
```

npmをホスト側から実行する場合

```sh
docker-compose exec vuesplash_web npm install
docker-compose exec vuesplash_web npm run watch
```

テスト docker内で実行

```sh
vendor/bin/phpunit --testdox
```
