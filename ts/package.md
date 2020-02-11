# 設定 package.json

安裝套件

```
yarn add typescript -D
```

修改 `package.json` 如下：

```json
{
  "scripts": {
    "build": "rm -rf dist && tsc",
    "dev": "tsc -w & bottender dev",
  },
  ...
}
```