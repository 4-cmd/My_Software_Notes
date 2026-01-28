
**** Dikkat Edilmesi Gerekenler ****
* Embed işlemi sırasında bilgisayardaki dosyaları ortadan kaldıran cmd kodları sakın yazma örneğin (%temp%)
* Bu yapıldığı takdirde, Cache ya da Embedler zarar görebilir 
* Ya da bir interpeter değiştirme
* Burada **SentenceWindowNodeParser** kullanmıştır tam olarak artısı görebilmek için **MetadataReplacementPostprocessor** kullanmalısın



```python
from llama_index.core import SimpleDirectoryReader,StorageContext
from llama_index.core.ingestion import IngestionPipeline
from llama_index.vector_stores.faiss import FaissVectorStore
from llama_index.embeddings.ollama import OllamaEmbedding
from llama_index.core import SimpleDirectoryReader,VectorStoreIndex,StorageContext,Settings
import faiss
from os import getenv
from llama_index.core import SimpleDirectoryReader,load_index_from_storage,VectorStoreIndex,StorageContext
from llama_index.core.storage.docstore import SimpleDocumentStore
import os
from llama_index.core.node_parser import SentenceWindowNodeParser


embedding_model_str = "bge-m3:latest"
 
ollama_embedding_model = OllamaEmbedding(model_name=embedding_model_str)
Settings.embed_model = ollama_embedding_model

# Bir test metni için embedding al

sample_embedding = ollama_embedding_model.get_text_embedding("test")  
dimesion = len(sample_embedding) 

faiss_index = faiss.IndexFlatL2(dimesion) 
vector_store = FaissVectorStore(faiss_index=faiss_index) 

sentence_windows_node_parser = SentenceWindowNodeParser.from_defaults(window_size=3)

transformation_list = [
sentence_windows_node_parser,ollama_embedding_model]
path_pdf = "./ekrem-imamoglu-ibb-iddianame.pdf"

reader = SimpleDirectoryReader(input_files=[path_pdf])
documents_list = reader.load_data(show_progress=True)

kısaltılmış_documents_list = documents_list[3300:3700]
print(f"Kaç tane döküman yer alıyor pdf'de : {len(documents_list)}")
persis_path = "./storage"

# 1. Önbelleğin kaydedileceği klasör yolu
PIPELINE_DIR = "./pipeline_storage"


pipeline = IngestionPipeline(

    transformations=transformation_list,

    docstore=SimpleDocumentStore() # Pipeline ne işlendiğini HATIRLAR, embed tekrar OLMAZ

)

if os.path.exists(PIPELINE_DIR)
	pipeline.load(PIPELINE_DIR) # load() → cache + docstore + pipeline state
	print("📦 Pipeline cache + docstore yüklendi")

else:
	print("🆕 Yeni pipeline başlatılıyor")

  

nodes = pipeline.run(documents=kısaltılmış_documents_list,show_progress=True)
  
pipeline.persist(PIPELINE_DIR)
print("💾 Pipeline cache + docstore diske kaydedildi")

  

if os.path.exists(persis_path): # Mevcut olan indeksi yükler
    vector_store = FaissVectorStore.from_persist_dir(persist_dir=persis_path)
    storage_context = StorageContext.from_defaults(vector_store=vector_store,persist_dir=persis_path)
    index = load_index_from_storage(storage_context=storage_context,embed_model = ollama_embedding_model)

    print(f"mevcut index yükleniyor")

else: # Klasör yoksa
    storage_context = StorageContext.from_defaults(vector_store=vector_store)
    
    index = VectorStoreIndex(nodes=[],storage_context=storage_context,embed_model=ollama_embedding_model,show_progress=True)
    print(f"Yeni bir index meydana getiriliyor")


print(f"Yeni Node adedi : {len(nodes)}")
index.insert_nodes(nodes=nodes)
print("📌 Node'lar index'e eklendi")
index.storage_context.persist(persist_dir=persis_path)
print(f"Storage klasörüne değerler eklenmiştir")

```
