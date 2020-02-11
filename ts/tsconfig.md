# 設定 tsconfig.json

新增設定檔 `tsconfig.json` 到專案根目錄，其內容如下：

```json
{
  "include": ["src/**/*"],
  "exclude": ["**/__tests__", "**/*.{spec,test}.ts"],
  "compilerOptions": {
    "target": "es5",
    "lib": ["es2017", "esnext.asynciterable"],
    "module": "commonjs",
    "skipLibCheck": true,
    "moduleResolution": "node",
    "esModuleInterop": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "rootDir": "./src",
    "outDir": "./dist",
    "types": ["node", "jest"],
    "strict": false
  }
}
```