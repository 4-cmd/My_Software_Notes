
Neden Pydantic-settings Kullanmalıyım
---

| Klasik Yöntem (`os.getenv`)                               | Pydantic Settings Yöntemi                                                        |
| --------------------------------------------------------- | -------------------------------------------------------------------------------- |
| Değerlerin tipini elinle çevirmen gerekir (str -> int).   | **Otomatik Tip Dönüşümü:** Port numarasını otomatik `int` yapar.                 |
| Değişken adını yanlış yazarsan `None` döner, hata vermez. | **Doğrulama:** Değişken eksikse uygulama ayağa kalkarken hata verir (Fail-fast). |
| Her yerde `os.getenv` çağırman gerekir.                   | **Tek Merkez:** Tüm ayarlar tek bir sınıf (Class) içinde toplanır.               |
| `.env` dosyasını okumak için ek kütüphane gerekir.        | **Dahili Destek:** `.env` dosyasını, sistem değişkenlerini otomatik okur.        |

```python
from pydantic_settings import BaseSettings, SettingsConfigDict
from llama_index.llms.mistralai import MistralAI
from functools import cached_property

  

class Settings(BaseSettings):
    # .env dosyasındaki isimlerle birebir aynı olmalı
    MISTRAL_API_KEY: str
    MISTRAL_MODEL_NAME: str 

  

    model_config = SettingsConfigDict(env_file=".env")

    # @property kullanarak objeyi ihtiyaç duyulduğunda oluşturuyoruz
    @cached_property
    def my_mistral_model(self) -> MistralAI:
        return MistralAI(
            api_key=self.MISTRAL_API_KEY,
            model=self.MISTRAL_MODEL_NAME
        )
settings = Settings()

```

**Bu uygulamayı Kullanmanın Artısı** 
* Önce bilgisayarındaki **çevre değişkenlerine (Environment Variables)** bakar.
* Orada bulamazsa **`.env`** dosyasına bakar.
* Eğer hiçbir yerde bulamazsa ve sen bir varsayılan değer vermediysen, programı çalıştırmaz ve seni uyarır: _"Hey, API_KEY eksik!"

Pydantic'te önce verileri (`key`, `model_name`) yüklemeli, ardından bu verileri kullanan objeyi bir **property (özellik)** olarak tanımlamalısın.