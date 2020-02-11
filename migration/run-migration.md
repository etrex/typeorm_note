# 執行 Migratione

官方文件：https://typeorm.io/#/migrations/running-and-reverting-migrations

> typeorm migration:create and typeorm migration:generate will create .ts files. The migration:run and migration:revert commands only work on .js files. Thus the typescript files need to be compiled before running the commands.

前面我們介紹了 `typeorm migration:generate` 指令，而這個指令生成出來的檔案是 `.ts` 檔，但接下來要執行 migration 時我們需要的檔案仍然是 `.js` 檔，於是乎我們需要再編譯一次再執行。

輸入以下指令來編譯與執行 migration：

```
tsc && typeorm migration:run
```

一個標準的 migration 流程如下：

1. 編輯 entity 的 `.ts` 檔
2. 編譯 entity 的 `.js` 檔
3. 生成 migration 的 `.ts` 檔(typeorm migration:generate)
4. 編譯 migration 的 `.js` 檔
5. 執行 migration 的 `.js` 檔(typeorm migration:run)

由於 tsc 並不會刪除多餘的檔案，所以我會建議在下指令之前先刪除所有 build 好的 `.js`

1. 編輯 entity 的 `.ts` 檔
2. 刪除 dist 資料夾以及所有檔案
3. 編譯 entity 的 `.js` 檔
4. 生成 migration 的 `.ts` 檔(typeorm migration:generate)
5. 編譯 migration 的 `.js` 檔
6. 執行 migration 的 `.js` 檔(typeorm migration:run)

在每次修改完 entity 之後，需要執行的指令依順為：

```
rm -rf dist
tsc
typeorm migration:generate
tsc
typeorm migration:run
```