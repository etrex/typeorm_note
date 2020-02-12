# 寫入關聯的方式

## 一對一關聯寫入

這樣的寫入是可行的：

```ts
const a = new A();
await a.save();

const b = new B();
b.a = a;
await b.save();
```

或

```ts
const a = await A.build({});
const b = await B.build({ a });
```

但是這樣寫就會炸裂：

```ts
const b = new B();
await b.save();

const a = new A();
a.b = b;
await a.save();
```

或

```ts
const b = await B.build({}});
const a = await A.build({ b });
```

在炸裂的情況下，一樣會正常寫入而且不會跳出錯誤訊息，不過 b 物件當中的 aId 值會是

## 一對多關聯寫入

這樣的寫入是可行的：

```ts
const c = new C();
await c.save();

const d = new D();
d.c = c;
await d.save();
```

或

```ts
const d = new D();
await d.save();

const c = new C();
c.ds = [d];
await c.save();
```

或

```ts
const c = await C.build({});
const d = await D.build({ c });
```

或

```ts
const d = await D.build({});
const c = await C.build({ ds: [d] });
```

在一對一時會炸裂的寫法，在一對多的時候卻是好的，表示在一對一會炸裂可能是個 bug。

在 `c.save()` 執行時，會自動 update d 資料表當中的 cId。
