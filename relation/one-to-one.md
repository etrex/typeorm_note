# 建立一對一關聯

以下是建立兩個 entity A 跟 B，一對一關聯的欄位被儲存在 B 的範例。

```ts
import { Entity, OneToOne } from 'typeorm';
import Base from './Base';
import B from './B';

@Entity()
export default class A extends Base {
  @OneToOne(type => B)
  b: B;
}
```

```ts
import { Entity, OneToOne, JoinColumn } from 'typeorm';
import Base from './Base';
import A from './A';

@Entity()
export default class B extends Base {
  @OneToOne(type => A)
  @JoinColumn()
  a: A;
}
```

我們使用 `@JoinColumn()` 來指定哪一邊該生成實際的資料庫欄位。

在本範例下 B 資料表會生成一個 aId 的 integer 欄位。
