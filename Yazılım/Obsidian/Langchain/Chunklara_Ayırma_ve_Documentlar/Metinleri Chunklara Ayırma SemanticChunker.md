
## 📌 SemanticChunker

Artı Özelliği
**Bağlam Kopmaz:** `RecursiveCharacterTextSplitter` bazen bir cümleyi tam ortasından veya en önemli yerinden bölebilir. `SemanticChunker` ise konunun bittiği yeri bekler.

```python
from langchain_experimental.text_splitter import SemanticChunker
from langchain_core.embeddings import Embeddings

def semantik_parçalayıcı(documents, embedding_model: Embeddings):
    # 1. Chunker nesnesini oluştur
    # breakpoint_threshold_type: Bölme noktasını neye göre belirleyeceği (yüzdelik, standart sapma vb.)
    
    text_splitter = SemanticChunker(
        embedding_model, 
        breakpoint_threshold_type="percentile" # En yaygın ve dengeli yöntem
    )

    # 2. Dökümanları böl
    # split_documents kullanımı metadata'ları (hash vb.) korur.
    semantic_docs = text_splitter.split_documents(documents)
    
    print(f"✅ Anlamsal olarak {len(semantic_docs)} parçaya bölündü.")
    return semantic_docs
    
```

Not : Bu eğer yerelde çalışacaksa biraz işlem maliyeti var ama api kullanılacaksa sorun yok