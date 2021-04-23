# Eager 和 Lazy

如同前面所講的，如果我們做了這樣的查詢：

```ts
const query = await D.findOne(d.id);
```

在獲得了 query 之後，是無法取得 query.c 以及 query.cId 的，所以當你已經擁有一個 d 物件，卻發現 d.c 是 undefined 的時候，你無法只使用一個查詢就查到 c。

如果說每次當你取得 d 物件時，你都希望 d.c 已經幫你查好，那麼你可以利用 eager 來幫你做到這點。

# Eager

以下是採用了 Eager 後的 Entity D：

```ts
import { Entity, ManyToOne } from 'typeorm';
import Base from './Base';
import C from './C';

@Entity()
export default class D extends Base {
  @ManyToOne(
    type => C,
    c => c.ds,
    { eager: true }
  )
  c: C;
}
```

和原本不同的地方是我們在 Entity D 的關聯上加上 { eager: true }，就能夠在每次查詢 d 的時候就自動預載 c 囉！

# Eager 的限制

你不能同時在 Entity C 跟 D 同時加上 { eager: true }，當你這樣做的時候你會在連線到資料庫的同時就獲得以下錯誤訊息：

```bash
Error: Circular eager relations are disallowed.
```

# Lazy

Lazy 可以讓在你需要的時候才去查詢物件，比方說原本 d.c 是 undefined，但我們使用 lazy 後 d.c 就會變成 Promise<C>，然後我們就可以進行以下操作：

```ts
const c = await d.c;
```

在 Entity D 上要這麼寫：

```ts
@Entity()
export default class D extends Base {
  @ManyToOne(
    type => C,
    c => c.ds
  )
  c: Promise<C>;
}
```

# Lazy 的限制

在這種情況之下，原本寫入關聯的寫法是這樣：

```ts
const c = new C();
await c.save();
const d = new D();
d.c = c;
await d.save();
```

就必須改為這樣：

```ts
const c = new C();
await c.save();
const d = new D();
d.c = Promise.resolve(c);
await d.save();
```

這兩邊都有蠻大的缺點，除非真的適用的場景，我認為應該盡可能避免使用 eager 和 lazy。

# Relation Id Column

一開始提到的 query 本身無法取得 relation id 的問題其實是有解的，只要在 Entity D 增加一個與 relation id 同名的 Column 就能取得：

```ts
import { Entity, ManyToOne } from 'typeorm';
import Base from './Base';
import C from './C';

@Entity()
export default class D extends Base {
  @Column({ nullable: true })
  cId: number;

  @ManyToOne(
    type => C,
    c => c.ds
  )
  c: C;
}
```

之後在做查詢時：

```ts
const query = await D.findOne(d.id);
```

就可以獲得 query.cId 的值。

# 參考資料

[官方文件 - eager-and-lazy-relations](https://typeorm.io/#/eager-and-lazy-relations)

[官方文件 - relations-faq](https://typeorm.io/#/relations-faq)
