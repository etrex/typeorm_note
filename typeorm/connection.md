# 建立 Connection

修改 `src/index.ts` 如下即可建立連線：

```ts
import 'reflect-metadata';
import { createConnection } from 'typeorm';

createConnection()
  .then(async connection => {
    console.log('Connection established.');
  })
  .catch(error => console.log(error));

export default async function App(context: any) {
  await context.sendText("hello");
}
```