Evet, **`st.session_state`** içindeki bir değişkene diğer dosyalardan (örneğin `prints.py`) doğrudan ulaşabilirsin.

**⚠️ Bunun tek bir kuralı vardır: `prints.py` dosyasının en üstüne de `import streamlit as st` yazmalısın.**

### Örnek Uygulama

**1. `main.py` (Ana dosyan):**

```python
import streamlit as st
from prints import session_verisini_yazdir # Diğer dosyadan fonksiyonu çağırıyoruz

# Değişkeni tanımla
if "document" not in st.session_state:
    st.session_state.document = "Gizli Hukuk Metni v1"

st.title("Ana Sayfa")

# Diğer dosyadaki fonksiyonu çalıştır
if st.button("Veriyi Diğer Dosyadan Oku"):
    session_verisini_yazdir()
```    

2. `prints.py` (Yardımcı dosyan):

```python
import streamlit as st # Bu import şart!

def session_verisini_yazdir():
    # main.py'de tanımlanan veriye buradan ulaşıyoruz
    veri = st.session_state.document
    print(f"Terminal Çıktısı: {veri}") # Konsola yazar
    st.write(f"Ekran Çıktısı: {veri}") # Arayüze yazar
```

### 💡 Dikkat Etmen Gerekenler

1. **Tanımlama Sırası:** `prints.py` içindeki fonksiyonu çağırmadan önce, o değişkenin `main.py` içinde (veya başka bir yerde) mutlaka oluşturulmuş olması gerekir.