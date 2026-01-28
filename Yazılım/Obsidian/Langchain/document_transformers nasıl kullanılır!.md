# Proje: LangChain RAG Optimizasyonu
## Konu: LongContextReorder Kullanımı

**Neden kullanılır?**
LLM'ler bağlamın ortasındaki bilgileri unutmaya meyillidir. Bu yöntem en alakalı dökümanları başa ve sona alarak doğruluğu artırır.

## Konu : EmbeddingsRedundantFilter Kullanımı

**Neden kullanılır?**
Bu araç, birbirine çok benzeyen veya tamamen aynı olan dökümanları eleyerek döküman kalabalığını azaltır.

### Uygulama Kodu

```python
from langchain_core.retrievers import BaseRetriever
from langchain_community.document_transformers import LongContextReorder,EmbeddingsRedundantFilter
from langchain_core.embeddings import Embeddings  
from langchain_classic.retrievers.document_compressors import DocumentCompressorPipeline
from langchain_classic.retrievers.contextual_compression import ContextualCompressionRetriever

  
def siralanmiş_dokumanlar_ve_gelismis_retriever(retriever: BaseRetriever,embedding_model: Embeddings):
    
    # 1. Benzerleri Ele
    redundant_filter = EmbeddingsRedundantFilter(embeddings=embedding_model, similarity_threshold=0.70)

    reordering = LongContextReorder()
    # İşlemler sırasıyla yapılır: Önce filtreleme, sonra yeniden sıralama

    pipilene_compressor_list = [redundant_filter,reordering]

    pipeline_compressor = DocumentCompressorPipeline(transformers=pipilene_compressor_list)

  

    compression_retriever = ContextualCompressionRetriever(base_compressor=pipeline_compressor,base_retriever=retriever)

    return compression_retriever

    

```

> **Not :** Bu ikisi dökümanları iyi bir şekilde iyileştirir ve işlem maliyeti çok düşüktür ve compression_retriever da create_retrieval_chain retrievar'ına at

>⚠️ **Uyarı** : **create_retrieval_chain** çalıştırırken **<font color="#ff0000">(invok ederken)</font> sadece key input value kullanıcı sorgusu olan bir sözlük hazırla diğerlerini zaten create_retrieval_chain halletmiş olacak 



