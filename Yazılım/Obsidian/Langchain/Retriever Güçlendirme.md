
```python
from langchain_community.document_transformers import LongContextReorder,EmbeddingsRedundantFilter
from langchain_community.retrievers import BM25Retriever
from langchain_classic.retrievers.document_compressors import DocumentCompressorPipeline
from langchain_classic.retrievers.ensemble import EnsembleRetriever
from langchain_classic.retrievers.contextual_compression import ContextualCompressionRetriever
from langchain_core.embeddings import Embeddings
from langchain_core.vectorstores.base import VectorStore

  
def retriever_yaratma_ve_guclendirme(vector_db : VectorStore,embedding_model : Embeddings,query : str):
    """
    Docstring for retriever_yaratma_ve_guclendirme
    * Bu fonksiyonda vector_db,embedding_model ve kullanıcı sorgusu parametre olarak alınmaktadır
    * retrievar ilk önce yaratılır ardından da belirli işlemler ve güçlendirmelerden sonra döndürülür
    * ek olarak burada daha iyi bir döküman bulmak için EmbeddingsRedundantFilter ve LongContextReorder kullanılmıştır
    """

    vector_retriever = vector_db.as_retriever(search_type="mmr", search_kwargs={"k": 10})
    documents = vector_retriever.invoke(query)
    bm25_retriever = BM25Retriever.from_documents(documents) # 'documents' semantik parçaların
    bm25_retriever.k = 10
  
    # 2. Hibrit Arama (Ensemble)
    ensemble_retriever = EnsembleRetriever(retrievers=[bm25_retriever, vector_retriever],weights=[0.4, 0.6])
    
    # 3. Filtreleme ve Yeniden Sıralama Hattı (Pipeline)
    redundant_filter = EmbeddingsRedundantFilter(embeddings=embedding_model)
    reordering = LongContextReorder()

    # Pipeline oluşturma
    pipeline_compressor = DocumentCompressorPipeline(transformers=[redundant_filter, reordering])

  

    # 4. Final Retriever (Süper Retriever)
    compression_retriever = ContextualCompressionRetriever(base_compressor=pipeline_compressor,base_retriever=ensemble_retriever)

  

    # transformers : Sıralaması kritiktir ilk olarak filtreleme ardından da sıralama yapılmaktadır
    # Eğer hafıza eklenmek istiyorsa
    # compression_retriever bu retriever
    # (create_history_aware_retriever) Bunun içine gomulmeli

    return compression_retriever



```
