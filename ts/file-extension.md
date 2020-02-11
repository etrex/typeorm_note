# 修改檔案

將 `src/index.js` 的副檔名改為 `.ts`

然後將 `module.exports = ` 改為 `export default`

修改 `index.js` 如下：

```
module.exports = require('./dist').default;
```