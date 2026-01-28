
Burada system prompt ve user prompt yazılıdır ek bir işlem yapmana gerek yok


```python
from llama_index.core.llms import ChatMessage, MessageRole
from llama_index.core import ChatPromptTemplate,PromptTemplate

TEXT_QA_PROMPT = PromptTemplate(

    """Sen bir yapay zeka asistanısın ve görevlerin:
• Kullanıcı sana bir bağlam gönderecek ve ek olarak sana bir soru soracak  

• Bu bağlamı kullanarak soruyu cevapla  

• Eğer bağlamda gönderilen soruyla ilişkili değilse, o zaman kendi iç hafızandan cevap ver

Not : Yukarıdaki Görevlerine Sadık kal ve başka bir işlem yapma

---------------------

Bağlam:

{context_str}

---------------------

Soru:

{query_str}


Cevap:

"""

)

text_qa_template = TEXT_QA_PROMPT