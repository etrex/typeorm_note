# 設定 ormconfig.js

Typeorm 會讀取專案根目錄下的 ormconfig 這個設定檔來進行初始化。

以下是一個 ormconfig 的範例：

```js
const config = {
  type: 'postgres',
  url: process.env.POSTGRESQL_URL,
  synchronize: false,
  logging: process.env.TYPEORM_LOGGING,
  entities: ['dist/entity/**/*.js'],
  migrations: ['dist/migration/**/*.js'],
  subscribers: ['dist/subscriber/**/*.js'],
  cli: {
    entitiesDir: 'src/entity',
    migrationsDir: 'src/migration',
    subscribersDir: 'src/subscriber',
  },
};

module.exports = config;
```

以下將一一說明其意義：

## type
指定選用的資料庫。

## url
指定 postgresql server 的位置，在這裡讀取環境變數，將來才比較好做部署。

url 格式如下：

```
postgresql://[user[:password]@][netloc][:port][/dbname][?param1=value1&...]
```

一些範例：

```
postgresql://
postgresql://localhost
postgresql://localhost:5432
postgresql://localhost/mydb
postgresql://user@localhost
postgresql://user:secret@localhost
postgresql://other@localhost/otherdb?connect_timeout=10&application_name=myapp
postgresql://localhost/mydb?user=other&password=secret
```

## synchronize
是否在連上 server 時自動進行資料庫 schema 的同步，在官方文件中說明不建議在 production 環境中開啟，所以就不建議使用。

## logging
指定是否需要顯示 log 在 console 上。

## entities
執行 `typeorm migration:generate` 時會去這個資料夾檢查目前的 entities 以及 資料庫當中的 table 之間的差
不會考慮 migration file 目前的狀態

必須指定在編譯好的 js 的資料夾路徑

## migrations
執行 `typeorm migration:run` 時會去此資料夾尋找所有 migration 檔案並且與資料庫中的 migrations table 做比對，找到所有需要執行的 migration file 並且一一執行。

必須指定在編譯好的 js 的資料夾路徑

## subscribers
不知道是做什麼的

## cli.entitiesDir
執行 `typeorm entity:create` 時產生的檔案將會放置在這裡

## cli.migrationsDir
執行 `typeorm migration:create` 和 `typeorm migration:generate` 時產生的檔案將會放置在這裡

## cli.subscribersDir
執行 `typeorm subscriber:create` 時產生的檔案將會放置在這裡