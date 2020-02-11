# 設定 package.json

先這樣

```
{
  "scripts": {
    "build": "rm -rf dist && tsc",
    "dev": "tsc -w & npx bottender dev",
    "start": "npx bottender start",
    "lint": "eslint '*/**/*.ts'",
    "test": "jest"
  },
  "dependencies": {
    "bottender": "1.0.7",
    "class-transformer": "^0.2.3",
    "ejs": "^3.0.1",
    "googleapis": "^45.0.0",
    "json-bigint": "^0.3.0",
    "line-pay-v3": "^1.0.5",
    "mz": "^2.7.0",
    "pg": "^7.15.1",
    "typeorm": "^0.2.22"
  },
  "devDependencies": {
    "@typescript-eslint/eslint-plugin": "^2.15.0",
    "@typescript-eslint/parser": "^2.6.0",
    "eslint": "^6.7.2",
    "eslint-config-prettier": "^6.7.0",
    "eslint-plugin-prettier": "^3.1.1",
    "jest": "^24.9.0",
    "prettier": "^1.19.1",
    "typescript": "^3.7.4"
  }
}
```