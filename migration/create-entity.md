# 建立 Entity

假設我們想要建立一個 Qna 的資料表，其中包含 question 和 answer 兩個文字欄位：

```
typeorm entity:create -n Qna
```

會生成以下程式碼：

```
import {Entity} from "typeorm";

@Entity()
export class Qna {

}
```

新增欄位的方式是在 Qna 中加入程式碼如下：

```
import {Entity, Column, PrimaryGeneratedColumn} from "typeorm";

@Entity()
export class Qna {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  question: string;

  @Column()
  answer: string;
}
```

每一個 entity 都需要指定 PrimaryColumn，在這裡我們用序號遞增的 id 來做。