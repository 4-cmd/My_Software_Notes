
```javascript
/**  
 * @param {number} num  
 * @returns {number}  
 */
   
function addOne(num) {  
    return num + 1;  
}

```

**Kod Açıklamaları** 
1️⃣ /** ... */ → Bu JSDoc bloğu, normal yorum satırı değil, IDE tarafından okunur.

**2️⃣ @param {number} num**

- num parametresinin tipi number olduğunu belirtiyor.
- Yani VSCode sana şunu söyler: “Bu fonksiyona string veya başka tip geçemezsin, number olmalı.”
- Örnek:

addOne(5);    // ✅ OK  
addOne("5");  // ❌ IDE uyarısı: Expected number

**3️⃣ @returns {number}**
- Fonksiyonun geri dönüş tipini belirtiyor.
- Bu da IDE’ye “bu fonksiyon number döndürür” bilgisini verir.

**4️⃣ Fonksiyon kendisi:**

function addOne(num) {  
    return num + 1;  
}

- Burada num 5 ise 6 döner.
- JSDoc tipleri runtime’da zorunlu değil, yani JS yine çalıştırır ama IDE uyarır.

|                     |                                   |
| ------------------- | --------------------------------- |
| Bölüm               | Ne işe yarar                      |
| @param {number} num | Fonksiyon parametresi tipi        |
| @returns {number}   | Fonksiyon dönüş tipi              |
| /** ... */          | Yorum ama IDE tarafından okunuyor |

✅ Avantaj: .js dosyasında TypeScript kullanmadan tip kontrolü ve IntelliSense alırsın.