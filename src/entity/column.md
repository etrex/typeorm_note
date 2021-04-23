# Column

每一個 entity 代表一個資料表，而每一個 column 則是代表一個欄位。

以下將介紹欄位的各種設定方式

## 指定欄位類型 (type)

以數字欄位為例，如果我們想要建立一個 price 欄位，寫法如下：

```ts
@Column()
price: number;
```

你可能會想要指定在 postgresql 當中使用 decimal，你可以這樣寫：

```ts
@Column('decimal')
price: number;
```

或者這樣寫：

```ts
@Column({
  type: 'decimal'
})
price: number;
```

更多介紹請參考官方文件：[https://typeorm.io/#/entities/column-types](https://typeorm.io/#/entities/column-types)

## 指定主鍵 Primary

把 `@Column()` 改為 `@PrimaryColumn()` 即可指定為主鍵。

或者可以使用 `@PrimaryGeneratedColumn()` 來設定自動遞增序號主鍵欄位。

一個 Entity 可以有多個 PrimaryColumn，代表多欄位的主鍵。

使用 `@Column()` 時也可以投過參數傳遞來指定為主鍵：

```ts
@Column({
  primary: true
})
```

更多介紹請參考官方文件：[https://typeorm.io/#/entities/primary-columns](https://typeorm.io/#/entities/primary-columns)

## 指定在資料庫的欄位名稱

```ts
@Column({
  name: '在資料庫的欄位名稱'
})
```

## 設定預設值

```ts
@Column({
  default: '預設值'
})
```

## 是否可為 Null

指定此欄位的值是否可為空，預設值為 false

```ts
@Column({
  nullable: true
})
```

## 是否唯一

指定此欄位的值是否唯一，預設值為 false

```ts
@Column({
  unique: true
})
```

## 其他選項

更多的設定請參考官方文件：[https://typeorm.io/#/entities/column-options](https://typeorm.io/#/entities/column-options)
