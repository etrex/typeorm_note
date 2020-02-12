# 設定 tsconfig.json

- [官方文件](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)
- [compiler options](https://www.typescriptlang.org/docs/handbook/compiler-options.html)

新增設定檔 `tsconfig.json` 到專案根目錄，其內容如下：

```json
{
  "exclude": ["**/__tests__", "**/*.{spec,test}.ts"],
  "compilerOptions": {
    // 基礎設定選項
    "target": "es5", // 輸出 ECMAScript 版本，預設是 es5
    "module": "commonjs", // 指定生成哪一種 module,
    "lib": ["es2017", "esnext.asynciterable"], // 列舉出編譯要引用的 library，特定語法例如 Object.assign 為 ES6 的語法
    "rootDir": "./src", // 宣告 TS 的 root 資料夾，不能 import 在這以外的 ts 檔
    "outDir": "./dist", // 編譯後的結果要輸出到哪裡
    "isolatedModules": true,

    // 檢查模式選項
    "strict": false,
    "types": ["node", "jest"], // 列舉出要被 compile 用的 type declaration files
    "skipLibCheck": true, // type declaration files 不做 type checking
    "forceConsistentCasingInFileNames": true,

    // module 解析選項
    "moduleResolution": "node", // 解析 module 的方法，有 classic 和 node
    "esModuleInterop": true, // 兼容 CommonJS module 引入，符合 ES6 module 規範
    "resolveJsonModule": true, // 讓 TS 可以解析 JSON 檔，解析出相對應的 type

    // 實驗性質選項
    "experimentalDecorators": true, // 允許使用實驗階段的 ES7 decorators
    "emitDecoratorMetadata": true
  }
}
```

### Reference

- https://ithelp.ithome.com.tw/articles/10222025
