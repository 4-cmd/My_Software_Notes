```javascript
import { AppDataSource } from "./data-source.js";
import { NewsContentEdit } from "./Models/Models.js";

// ✅ 3) Repository yaratmak
const newsRepo = AppDataSource.getRepository(NewsContentEdit);

// ✅ 4) Veritabanı Select
const allNews = await newsRepo.find(); // SELECT * FROM NewsContentEdit
console.log(allNews);

// ✅ 5) Veritabanı Create and Save ve Kaydedilen Instance'in ID değerini almak
const new_instance = newsRepository.create(); // Özelliklere Değer Atama (Şemada tanımladığınız sütun adları geçerlidir)
new_instance.TitleofNew = "Yeni Haber Başlığı";
new_instance.Tarih = new Date();
const saved_object = await newsRepository.save(new_instance)
const kaydedilen_ID_degeri = saved_object.ID

// Nasıl Koşul Belirtilir? with Find 
const ID_of_Turkey = Cografya_Kütüphanesi_repository.find(
    {
        where :
        {
            Tanım : "Türkiye"
        }
    }
)
//  Output :[ { ID: 1, 'Tanım': 'Türkiye', USTID: 0 } ]

const ID_of_Turkey = await Cografya_Kutuphanesi_repository.find(
{
        select: {
            ID: true // Sadece ID sütununu seç.
            // Tanım: false, // Diğer sütunları almıyoruz.
        },
        where: {
            Tanım: "Türkiye"
        }
}

// Output :  [ { ID: 1 } ]



/*
🔍 findBy() Ne Döndürür?
findBy(options) metodu, veritabanından koşullara uyan tüm varlıkları (Entity'leri) getirir
*/


// SQL: SELECT * FROM Yayin_Organlari WHERE Coğrafya_ID = 5 AND URL IS NOT NULL  
const bursa_url_olanlar = await Yayin_Organlari_repository.findBy
({  
    Coğrafya_ID: 5,  
    URL: Not(IsNull()) // veya basitçe URL: Not(IsNull())  
});
```
