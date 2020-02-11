# 資料庫遷移 - Migration

如果想要建立或者修改資料模型，使用 Migration 檔案的方式來管理資料庫結構的更新是最理想的。

Typeorm 支援自動生成 Migration 檔案，你可以在建立 Entity 或更新 Entity 後，自動生成 Migration 檔案，然後使用 Migration 檔案進行資料庫結構的更新。

Typeorm 不支援建立資料庫，所以建立資料庫的部分要自己做。
