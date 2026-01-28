
```python
from sklearn.metrics.pairwise import cosine_similarity
from langchain_community.docstore.document import Document
from langchain_ollama.embeddings import OllamaEmbeddings
import re

embedding_model = OllamaEmbeddings(model="mxbai-embed-large:latest")


markdown = MarkItDown()
markdown_convert = markdown.convert(tmp_path)
markdown_text = markdown_convert.text_content

pattern = r'(?<=[.!?]) +'
texts : list = re.split(pattern,markdown_text)
semantic_final_chunks = create_semantic_chunks(texts=texts,threshold=0.5)

if len(semantic_final_chunks) != 0:
	print(f"chunklara metadata atanıyor")
	for chunk in semantic_final_chunks:
		chunk : Document = chunk
		chunk.metadata = {"source" : file_name}

```

* Burada sematic ayrılan parçalara metadata (isteğe bağlı) atanır 
* Ardından da bu dökümanlar faiss'e kaydedilir 



```python
def create_semantic_chunks(texts : list[str],threshold : float = 0.5):
    """
    Cümleler arası anlamsal benzerliğe bakarak metni parçalara ayırır.
    son olarak eğer texts ifadeleri boş değilse bir list[document] döndürür
    """

    if not texts:
        return []

    chunks = []
    # embedding_model'in embed_documents fonksiyonu tüm listeyi tek seferde işler
    embeddings = embedding_model.embed_documents(texts=texts)
    current_chunk = [texts[0]]

    for i in range(1,len(texts)):
        # İki vektör arasındaki benzerliği hesapla (0 ile 1 arası bir değer)
        # [0][0] matris çıktısından tekil değeri almak içindir
        similitary = cosine_similarity([embeddings[i-1]], [embeddings[i]])[0][0]

        if similitary > threshold: # Eğer benzerlik eşiğin üzerindeyse, aynı konudur; mevcut parçaya ekle
            current_chunk.append(texts[i])
        
        else:
            # Benzerlik düşükse konu değişmiştir; eski parçayı kapat, yenisini başlat
            chunks.append(" ".join(current_chunk))
            current_chunk = [texts[i]]

    # Son kalan parçayı da listeye ekle

    if current_chunk:
        chunks.append(" ".join(current_chunk))

  

    final_documents = [Document(page_content=chunk) for chunk in chunks]
    print(f"final documents : {final_documents}")
    return final_documents