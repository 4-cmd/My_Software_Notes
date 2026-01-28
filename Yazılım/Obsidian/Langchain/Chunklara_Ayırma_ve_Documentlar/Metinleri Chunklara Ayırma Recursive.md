**Ne Zaman Kullanmalısın?**
**Genel Bilgi/Makaleler:** **`RecursiveCharacterTextSplitter`** (yukarıdaki separators listesiyle). En hızlı ve sorunsuz yöntemdir.




```python

from langchain_text_splitters import RecursiveCharacterTextSplitter

text_splitter = RecursiveCharacterTextSplitter(
    # Bölme önceliği: Paragraf -> Cümle -> Boşluk
    separators=["\n\n", "\n", ". ", "? ", "! ", " ", ""],
    chunk_size=600,
    chunk_overlap=100,
    length_function=len,
    is_separator_regex=False,
)

```




