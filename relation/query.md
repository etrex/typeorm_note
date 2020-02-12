# 查詢關聯的方式

本文假設你有執行過以下程式：

```ts
const c = await C.build({});
const d = await D.build({ c });
```

一般情況下的查詢，不會取得任何關聯內容。

```ts
const query = await D.findOne({ id: d.id, relations: ['c'] });
console.log(query.c); // undefined
// 你也無法取得外來鍵的值
console.log(query.cId); // undefined
```

你可以使用 Entity.findOne 的第二個參數來指定要連帶取得的關聯資料：

```ts
const query = await D.findOne(d.id, { relations: ['c'] });
console.log(query.c);
```

或

```ts
const query = await D.findOne({ id: d.id }, { relations: ['c'] });
console.log(query.c);
```

在使用 Entity.find 的情況下，可以使用 where 下查詢條件，再用 relations 來指定取得關聯：

```ts
const query = await D.find({ where: { id: d.id }, relations: ['c'] });
console.log(query[0].c);
```
