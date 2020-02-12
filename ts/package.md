# 設定 package.json

修改 `package.json` 如下：

```json
{
  "scripts": {
    "build": "rm -rf dist && tsc",
    "dev": "tsc -w & bottender dev", // 注意是一個 `&`
    "dev:console": "tsc -w & bottender dev -- --console", // 方便使用 console 模式開發 & 注意是一個 `&`
    "start": "npm run build && bottender start",
    ...
  },
  ...
}
```
