

```python
from pydantic import BaseModel, field_validator
import re

  

class Person(BaseModel):
    name: str
    age: int

    @field_validator("name")
    @classmethod # Pydantic 2.0+ için iyi bir alışkanlık
    def isim_de_sayi_var_mi(cls, v: str): # n yerine v (value) kullanımı yaygındır
        pattern = r"\d" # raw string (r) kullanmak kaçış karakterleri için güvenlidir
        
        if re.search(pattern, v): # findall yerine search daha hızlıdır (ilk sayıyı bulunca durur)
            raise ValueError("İsminiz içinde sayı olmamalıdır")

        return v # Doğrulama başarılıysa mutlaka değeri geri döndürmelisin!

try:
    instance = Person(name="Şevki1", age=18)
    print(instance)

except Exception as e:
    print(f"Hata yakalandı: {e}")

```

* Validator fonksiyonları sadece kontrol yapmaz, aynı zamanda veriyi **modifiye de edebilir.***
* Örneğin, ismi her zaman büyük harfe çevirmek isteseydin `return v.upper()` diyebilirdin.
* Eğer `return` etmezsen, Pydantic o alanı `None` olarak günceller (veya hata verir).