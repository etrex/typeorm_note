# 修改檔案

將 `src/index.js` 的副檔名改為 `.ts`。

然後將 `src/index.js` 程式碼中的 `module.exports = ` 改為 `export default`。

並且修改 `index.js` 如下：

```
module.exports = require('./dist').default;
```