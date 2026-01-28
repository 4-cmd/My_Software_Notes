
```python
from langchain_core.messages import trim_messages

Args and their explanations in trim_messages

messages = trim_messages(mesajlar,max_tokens = 250,strategy = "last",token_counter = llm,allow_partial=True)


```

**📜 mesajlar (Args)**
Anlamı: Kırpılacak olan sohbet geçmişi listesidir (içinde HumanMessage, AIMessage vb. nesneler bulunur).

**🔢 max_tokens**
Anlamı: Kırpma işleminden sonra geriye kalan tüm mesajların toplam token sayısının maksimum sınırı (250 token).

**🎯 strategy**
Anlamı: Kırpma stratejisinin "en son olanı tut" olduğunu belirtir. Fonksiyon, toplam token sayısı 250'i geçmeyecek şekilde, listenin sonundan başlayarak mümkün olduğunca çok mesajı koruyacaktır.

**🧮 token_counter**
Anlamı: Token sayımını yapacak olan aracı veya fonksiyonu belirtir.

**🤏 allow_partial**
Bu, kırpma işleminin esnekliğini artırır.
Eğer bir mesajın tamamı 250 token sınırına sığmıyorsa, fonksiyon o mesajı bölerek yalnızca bir kısmını (parçalı içeriğini) sohbete dahil eder.

strategy="last" olduğu için, sığabilen son parçası alınır.