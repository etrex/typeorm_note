# 在 Bottender 上使用 Entity

我們現在要在 Bottender 簡單測試一下剛做好的 Qna Entity，我們會先新增一筆資料，然後根據用戶說的內容去比對 question，如果有中的話就回覆 answer，沒中的話就回覆預設值。

修改 `src/index.ts` 如下：

```ts
import 'reflect-metadata';
import { createConnection } from 'typeorm';
import Qna from './entity/Qna';

createConnection()
  .then(async connection => {
    console.log('Connection established.');
  })
  .catch(error => console.log(error));

export default async function App(context: any) {
  await Qna.findOrCreateBy({
    question: '你好嗎',
    answer: '我很好',
  });

  const qna = await Qna.findOne({ question: context.event.text });
  const responseMessage = qna?.answer ?? 'Hello, TypeORM!';
  await context.sendText(responseMessage);
}
```

以下說明上面的程式碼：

```ts
await Qna.findOrCreateBy({
  question: '你好嗎',
  answer: '我很好',
});
```

這段是為了教學方便，我們先用簡單的方法新增一些測試資料。

以下的程式碼才是真正的主程式，我們嘗試在 Qna 下尋找滿足 question 等於用戶傳的訊息：

```ts
const qna = await Qna.findOne({ question: context.event.text });
```

如果有找到內容的話就回應 answer，沒有找到的話就回應 Hello, TypeORM!

```ts
const response_message = qna?.answer ?? 'Hello, TypeORM!';
```

TypeORM 就是這麼簡單。

這裡用到的 findOne 是來自於 TypeORM 內建的 BaseEntity，關於 BaseEntity 的詳細說明請參考官方文件：[https://typeorm.io/#/active-record-data-mapper](https://typeorm.io/#/active-record-data-mapper)
