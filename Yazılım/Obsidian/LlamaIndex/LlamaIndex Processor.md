## 📌FixedRecencyPostprocessor Nasıl Kullanılır?


```python
from llama_index.core.postprocessor import FixedRecencyPostprocessor
postprocessor = FixedRecencyPostprocessor(top_k=5, date_key="creation_date") # Meta verinizdeki anahtar neyse onu yazmalısınız
# Bizim metadata içindeki zaman key creation_datedir


```

LlamaIndex, dokümanlarınızın (Node) `metadata` sözlüğüne bakar. 

Eğer meta verinizdeki anahtar (key) ismi `date` değil de `start_time` ise, `date_key` parametresini buna göre güncellemeniz **zorunludur**. 
Aksi takdirde işlemci dokümanların içindeki tarih bilgisini bulamaz ve sıralama yapamaz.