# Base Entity

Entity 可以用繼承的方式獲得共同的設定值，所以可以製作一個 BaseEntity 來寫共用的部分。

我們本次的專案當中，所有的 Entity 都需要一個自動遞增主鍵 id，以及記錄建立時間的 createdAt 和紀錄最後更新時間 updatedAt 欄位，並且再加上一些方便的方法比如說 build、findOrCreateBy、exists 等，這就很適合用 BaseEntity 的方式來製作。

事實上 TypeORM 本身就已經做了一個 BaseEntity，提供了一些基本功能，而我們需要的是客製化的結果，所以我們可以新增一個 Base 類別並且再繼承至 BaseEntity 來處理。

TypeORM BaseEntity: [https://typeorm.io/#/active-record-data-mapper](https://typeorm.io/#/active-record-data-mapper)

在 `src/entity` 資料夾下新增一個 `Base.ts` 檔，撰寫以下內容：

```ts
import {
  BaseEntity,
  PrimaryGeneratedColumn,
  CreateDateColumn,
  UpdateDateColumn,
} from 'typeorm';

export default class Base extends BaseEntity {
  @PrimaryGeneratedColumn()
  id: number;

  @CreateDateColumn()
  createdAt: Date;

  @UpdateDateColumn()
  updatedAt: Date;

  static async build(params) {
    const entity = new this();
    Object.keys(params).forEach(key => {
      entity[key] = params[key];
    });
    await entity.save();
    return await this.findOne(entity.id);
  }

  static async findOrCreateBy(params) {
    const entity = await this.findOne(params);
    if (entity != null) {
      return entity;
    }
    return await this.build(params);
  }

  static async exists(params) {
    const entity = await this.findOne(params);
    return entity != null;
  }
}
```

完成後，修改 `src/entity/Qna.ts` 如下：

```ts
import { Entity, Column } from 'typeorm';
import Base from './Base';

@Entity()
export default class Qna extends Base {
  @Column()
  question: string;

  @Column()
  answer: string;
}
```

改好之後進行[標準資料庫遷移流程](https://etrex.tw/typeorm_note/migration/run-migration.html)。

可以發現到在 qna 表格上新增了兩個欄位 createdAt 和 updatedAt。
