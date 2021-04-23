# 索引 - Index

我們要在 Qna 上的 question 加入索引，加索引的方法很簡單：

```ts
@Index()
@Column()
question: string;
```

或

```ts
@Column()
@Index()
question: string;
```

都是可以的，改好之後進行[標準資料庫遷移流程](https://etrex.tw/typeorm_note/migration/run-migration.html)。

可以發現到在 qna 表格上新增了一個索引。

以下說明在 TypeORM 上加入索引的各種細節：

## 幫索引命名

```ts
@Index("name1-idx")
```

## 唯一索引

```ts
@Index({ unique: true })
```

## 多欄位索引

多欄位索引是寫在 Entity 上而不是 Column 上。

```ts
@Index(["question", "answer"])
export class Qna {
  ...
}
```

## 多欄位唯一索引

多欄位索引用第二個參數傳遞選項。

```ts
@Index(["question", "answer"], { unique: true })
export class Qna {
  ...
}
```

特殊索引請參考官方文件：[https://typeorm.io/#/indices/spatial-indices](https://typeorm.io/#/indices/spatial-indices)
