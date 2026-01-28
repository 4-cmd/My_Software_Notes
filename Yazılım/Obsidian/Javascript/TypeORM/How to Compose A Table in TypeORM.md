
```javascript

import { EntitySchema } from "typeorm";

export const NewsContentEdit = new EntitySchema({
  name: "NewsContentEdit",
  tableName: "NewsContentEdit",
  columns: {
    ID: {
      type: Number,
      primary: true,
      generated: true
    },
    TitleofNew: {
      type: String,
      nullable: true
    },
    Link: {
      type: String,
      nullable: true
    },
    Tarih: {
      type: Date,
      nullable: true
    },
    Link: {
      type: String,
      nullable: true
    },
    Category: {
      type: String,
      nullable: true
    }
  }
});
```