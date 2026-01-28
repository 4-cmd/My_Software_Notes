
**Retriever Güçlendirme ile İlgili notlar** 
1. FUSION_MODES.RECIPROCAL_RANK ile "reciprocal_rerank" arasında temel bir fark yoktur
2. sadece ayrım noktası birincisi enum değer ikincisi ise string değerdir
3. QueryFusionRetriever : LlamaIndex içinde hibrit arama (Hybrid Search) yapmanın modern ve "resmi" yolu budur.
4. Bu sınıf, sonuçları sadece birleştirmekle kalmaz, Reciprocal Rank Fusion (RRF) algoritmasıyla en alakalı olanları en üste taşır.




```python
from llama_index.core.indices.base import BaseIndex
from llama_index.core.retrievers import VectorIndexRetriever
from llama_index.retrievers.bm25 import BM25Retriever
from llama_index.core.retrievers import QueryFusionRetriever
from llama_index.core.retrievers.fusion_retriever import FUSION_MODES

  
  

"""
* BM25 (anahtar kelime araması), çok geniş bir döküman havuzu içinde anlamlıdır.
* Sen zaten vektör arama ile en iyi 5 parçayı seçmişsin; BM25'i sadece bu 5 parça içinden seçim yapmaya zorlamak "hibrit arama" mantığına aykırıdır.
* Hibrit aramanın amacı, vektörün kaçırdığı bir kelimeyi tüm dökümanlar arasından yakalamaktır.
"""

def retrievar_ozellestirme(index : BaseIndex):

    # 1. Vektör tabanlı arama (Anlamsal/Semantic)
    vector_retriever = VectorIndexRetriever(index=index, similarity_top_k=5)
    # 2. BM25 tabanlı arama (Anahtar Kelime/Keyword)
    
    # DİKKAT: nodes=nodes değil, tüm index'teki docstore kullanılmalı
    bm25_retriever = BM25Retriever.from_defaults(
        docstore=index.docstore, # Tüm döküman havuzuna bakar
        similarity_top_k=5
    )

    # 3. Hibrit Birleştirici (Fusion)

    retriever_list = [vector_retriever, bm25_retriever]
    hybrid_retriever = QueryFusionRetriever(
        retrievers=retriever_list,
        similarity_top_k=5,
        num_queries=1,
        mode=FUSION_MODES.RECIPROCAL_RANK,
        verbose=True
    )

    return hybrid_retriever
```



**Neden Bu yapıyı Kullanmalısın?**
Vektör modelleri eğitildikleri kelime dağarcığına dayanır. Çok yeni bir terim veya marka adı söz konusu olduğunda vektör araması yanılabilir.
	
Örnek: Bugün piyasaya çıkan "XyloZorp" isimli bir ürün hakkında arama yaptığınızda, vektör modeli bu kelimeyi tanımadığı için saçma sonuçlar verebilir. 
Ancak Hibrit arama, kelime eşleşmesi sayesinde bu yeni terimi içeren dokümanları hemen bulur.
	
**hybrid_retriever Kullanmanın artısı**
**Güvenilirlik:** Halüsinasyon riskini (yanlış döküman getirme kaynaklı) azaltır
	