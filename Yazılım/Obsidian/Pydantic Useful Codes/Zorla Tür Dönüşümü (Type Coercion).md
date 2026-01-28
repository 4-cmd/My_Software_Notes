
**Strict (Sıkı) Mod vs Lax (Gevşek) Mod**

* **Lax Mode (Varsayılan):** Otomatik dönüşüm yapar
* **Strict Mode:** Sadece tam olarak o tipte veri kabul eder.

**Lax Mode Örneği** : Burada otomatik tür dönüşümü yapılır 

```python
from pydantic import BaseModel

class ModelAyarlari(BaseModel):
    islem_sayisi: int
    ogrenme_hizi: float
    aktif_mi: bool
    integer_liste : list[int]



küme = {1,2,3,4,5}    
model = ModelAyarlari(
    aktif_mi="true", # String ama bool'a dönebilir
    integer_liste=küme,# küme ama listeye dönüşebilir
    islem_sayisi="50", # String ama sayıya dönebilir
    ogrenme_hizi="1.111" # String ama float'a dönebilir
)


print(model.islem_sayisi) # Çıktı: 50 (Artık bir integer!)
print(type(model.islem_sayisi)) # <class 'int'>

```

**Strict Mode:** Burada ise otomatik dönüşüm yapılmaz. Sadece istenilen tipte veri gelmesi beklenir. Eğer istenilen veri türünde veri gelmezse o zaman hata patlatır

```python

from pydantic import BaseModel, ConfigDict, ValidationError

class SikiModel(BaseModel):
    model_config = ConfigDict(strict=True) # Sıkı modu açtık
    sayi: int

  
try:
    SikiModel(sayi="10") # HATA VERİR! Çünkü veri string.
except ValidationError as e:
    print("Hata: Sıkı modda string kabul edilmez.")



```