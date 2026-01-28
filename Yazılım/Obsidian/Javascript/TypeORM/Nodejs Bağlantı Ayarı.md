
**TypeORM kullanarak Bağlantı Ayarı Nasıl Yapılır?**

```javascript
import {DataSource} from "typeorm";
import {NewsContentEdit} from "./Models/Models.js";

export const AppDataSource = new DataSource({
  type: "mssql",
  host: "localhost\\SQLEXPRESS",
  port: 1433,
  username: "node_js_user", // SQL Server auth username
  password: "Net141jm",        // SQL Server auth password
  database: "YerelHaberler",
  options: {
    encrypt: false,
    trustServerCertificate: true
  },
  synchronize: false,
  logging: true,
  entities: [NewsContentEdit] // ✅ Şimdi doğru
});

// Bağlantı kontrolü ve başlatma

AppDataSource.initialize()
    .then(() => {
        // Eğer bu kısım çalışırsa, bağlantı BAŞARILIDIR!
        console.log("✅ TypeORM Veritabanı Bağlantısı Başarılı!");
    })
    .catch((error) => {
        // Eğer bu kısım çalışırsa, bağlantı BAŞARISIZDIR!
        console.error("❌ TypeORM Bağlantı Hatası:", error);
        // Hata kodunu görmek için, hatanın tüm içeriğini yazdırmak faydalı olabilir:
        // console.error(JSON.stringify(error, null, 2));
    });
```

**entities: [NewsContentEdit]**

**Entityler yani sınıflar buraya tanımlanmalıdır**

