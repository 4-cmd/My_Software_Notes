
* Bunun haricinde ek bir işlem yapmana gerek yok 
* Zaten SQLRecoredManager hangisi döküman eklendi hangisi eklenmedi bunu otomatik olarak yapar

```python
from langchain.indexes import index, SQLRecordManager

# 3. Record Manager Kurulumu (Hash'lerin saklanacağı SQLite veritabanı)
namespace = "faiss/my_documents"
record_manager = SQLRecordManager(
namespace, db_url="sqlite:///record_manager_cache.sql"
)
record_manager.create_schema()

# 4. İndeksleme İşlemi (Hash kontrolünü otomatik yapar)
indexing_stats = index(
    docs_source=docs,
    record_manager=record_manager,
    vector_store=vector_store,
    cleanup="incremental",
    source_id_key="source"
)

# incremental  
# Değişen hashleri günceller, aynı olanları atlar.  
# En yaygın kullanım. Dosya güncellendiğinde eskisi silinir, yenisi gelir.

print(f"İndeksleme İstatistikleri: {indexing_stats}")

# Çıktı örneği:
# {
#   'num_added': 0,    <-- Eğer dökümanlar aynıysa burası 0 olur
#   'num_updated': 0,
#   'num_skipped': 15, <-- Sistem bunları zaten hash'ten tanıdı ve atladı
#   'num_deleted': 0
# 

```



