# 建立 Entity

假設我們想要建立一個 Qna 的資料表，其中包含 question 和 answer 兩個文字欄位：

```
typeorm entity:create -n Qna
```

會生成以下程式碼：

```ts
import { Entity } from 'typeorm';

@Entity()
export class Qna {}
```

在這裡請直接修改 Qna，程式碼如下：

```ts
import { Entity, Column, PrimaryGeneratedColumn } from 'typeorm';

@Entity()
export default class Qna {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  question: string;

  @Column()
  answer: string;
}
```

可以看見我們用 `@PrimaryGeneratedColumn()`、`@Column()` 來宣告欄位。每一個 entity 都需要指定 PrimaryColumn，在這裡我們用序號遞增的 id 來做。
