
* **Başlıklara Göre Bölme (`MarkdownHeader`):** Metni rastgele bir yerden (örneğin bir cümlenin ortasından) değil, mantıksal bölümlerden (Giriş, Yöntem, Sonuç gibi) ayırır.

* **İkincil Parçalama (`Recursive`):** Eğer bir başlığın altındaki metin çok uzunsa (örneğin 5000 karakter), bu metin modelin hafızasına (context window) sığmayabilir ve hata alınması ihtimali yüksektir. İkinci adım, bu uzun bölümleri 1000 karakterlik yönetilebilir parçalara böler ve olası hatanın önüne geçer



```python
from langchain_text_splitters import MarkdownHeaderTextSplitter,RecursiveCharacterTextSplitter
        # 1. Başlıklara göre bölme kuralını tanımla

headers_to_split_on = [("#", "Header 1"),("##", "Header 2"),("###", "Header 3")]

text_splitter = RecursiveCharacterTextSplitter(chunk_size = 1000,chunk_overlap = 100)

markdown_splitter =
MarkdownHeaderTextSplitter(headers_to_split_on=headers_to_split_on)

# MarkItDown ile Markdown'a dönüştür
result = markdown.convert(tmp_path)
markdown_text = result.text_content
header_splits = markdown_splitter.split_text(text=markdown_text)
chunks = text_splitter.split_documents(documents=header_splits)
                
                
```           