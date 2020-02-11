# 自動生成 Migration

官方文件：https://typeorm.io/#/migrations/running-and-reverting-migrations

> typeorm migration:create and typeorm migration:generate will create .ts files. The migration:run and migration:revert commands only work on .js files. Thus the typescript files need to be compiled before running the commands.

由於自動生成 Migration 的指令只能讀取 js，而我們所寫的 entity 卻是 ts，所以我們需要先進行一次 build，再去做 migration 自動生成。

執行以下指令即可達成先編譯 js 再進行自動生成

```bash
tsc && typeorm migration:generate -n migration
```

在這裡要注意的是 `typeorm migration:generate` 指令並不會幫你去檢查目前的 migration file 內是否已經存在對應的 migration。

`typeorm migration:generate` 會去計算目前的 entities 以及資料庫當中的 table 之間的差，來自動生成 migration 檔案，他不會考慮尚未被執行的 migration 檔案，也就是說 `typeorm migration:generate` 不是冪等指令。

要證明這點也很簡單，只要多執行幾次 `typeorm migration:generate`，再去觀察 migration 資料夾下的檔案就可以發現他會不斷生成相同內容的檔案。

然而這很有可能會導致一個操作失誤就讓 migration 壞掉，通常存在 2 個相同內容的 migration 檔案時，第一個檔案會執行成功，但是第二個會失敗，因為同名的內容已經存在，所以就會卡死在這裡，這點要注意。
