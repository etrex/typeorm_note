# 建立一對多關聯

以下是建立兩個 entity C 跟 D，關聯的欄位被儲存在 D 的範例。

```ts
import { Entity, OneToMany } from 'typeorm';
import Base from './Base';
import D from './D';

@Entity()
export default class C extends Base {
  @OneToMany(
    type => D,
    d => d.c
  )
  ds: D[];
}
```

```ts
import { Entity, ManyToOne } from 'typeorm';
import Base from './Base';
import C from './C';

@Entity()
export default class D extends Base {
  @ManyToOne(
    type => C,
    c => c.ds
  )
  c: C;
}
```

我們使用 `@ManyToOne()` 來指定哪一邊該生成實際的資料庫欄位。

在本範例下 D 資料表會生成一個 cId 的 integer 欄位。
