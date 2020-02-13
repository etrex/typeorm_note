# 在 Bottender 上使用 Relations

我們可以試著將原本的 Entity Qna 當中的 question 以及 answer 的字串欄位改成關聯，而我們另開一個 Entity 叫做 Sentence，這樣做的好處是當 question 以及 answer 有出現重複文字的時候可以節省儲存空間。

首先就是要在 `src/entity` 下新增 `Sentence.ts` 檔案，內容如下：

```ts
import { Entity, Column, OneToMany } from 'typeorm';
import Base from './Base';
import Qna from './Qna';

@Entity()
export default class Sentence extends Base {
  @Column({ unique: true })
  content: string;

  @OneToMany(
    type => Qna,
    qna => qna.question
  )
  questions: Qna[];

  @OneToMany(
    type => Qna,
    qna => qna.answer
  )
  answers: Qna[];

  static async get(content) {
    return (await Sentence.findOrCreateBy({ content })) as Sentence;
  }
}
```

然後修改原本的 Qna.ts 如下：

```ts
import { Entity, ManyToOne } from 'typeorm';
import Base from './Base';
import Sentence from './Sentence';

@Entity()
export default class Qna extends Base {
  @ManyToOne(
    type => Sentence,
    sentence => sentence.questions,
    {
      eager: true,
    }
  )
  question: Sentence;

  @ManyToOne(
    type => Sentence,
    sentence => sentence.answers,
    {
      eager: true,
    }
  )
  answer: Sentence;
}
```

記得改完 Entity 之後要做 migration 哦。

接著我們可以修改主程式如下：

```ts
import 'reflect-metadata';
import { createConnection } from 'typeorm';
import Qna from './entity/Qna';
import Sentence from './entity/Sentence';

createConnection()
  .then(async connection => {
    console.log('Connection established.');
  })
  .catch(error => console.log(error));

export default async function App(context: any) {
  const qna = new Qna();
  qna.question = await Sentence.get('你好嗎');
  qna.answer = await Sentence.get('我很好');
  await qna.save();

  const query = await Qna.findOne({
    question: await Sentence.get(context.event.text),
  });

  const responseMessage = query?.answer?.content ?? 'Hello, TypeORM!';
  await context.sendText(responseMessage);
}
```

就能獲得跟原本相同功能的程式囉！

不過這樣其實是有問題的，因為我們的 query 其實下了兩次的 SQL 指令：

```ts
const query = await Qna.findOne({
  question: await Sentence.get(context.event.text),
});
```

如果要讓他縮短成一次的話就必須使用 QueryBuilder 的寫法：

```ts
const query = await Qna.createQueryBuilder('qna')
  .leftJoinAndSelect('qna.question', 'question')
  .leftJoinAndSelect('qna.answer', 'answer')
  .where('question.content = :content', { content: context.event.text })
  .getOne();
```

可憐哪。

# 參考資料

只能使用 QueryBuilder 對關聯下查詢條件：https://github.com/typeorm/typeorm/issues/1916
