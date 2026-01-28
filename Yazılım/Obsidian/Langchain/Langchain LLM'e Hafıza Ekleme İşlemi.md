
* Bu kod, llm'in önceki cevapları hatırlaması için yapılmıştır 
* Opsiyonel olarak, **LongContextReorder** ile dökümanları sıralanabilir ya da **trim_messages** ile mesajların belli bir kısmı ortadan kaldırılabilir


```python
from langchain.chains.history_aware_retriever import create_history_aware_retriever
from langchain_classic.chains.retrieval import create_retrieval_chain
from langchain.chains.combine_documents import create_stuff_documents_chain
from langchain_core.prompts import ChatPromptTemplate, MessagesPlaceholder
from langchain_core.retrievers import BaseRetriever
from langchain_core.language_models.base import LanguageModelLike

async def retriever_and_chat_history_async(retriever : BaseRetriever,llm : LanguageModelLike,query : str):
    contextualize_q_system_prompt = ("Geçmiş sohbeti ve son soruyu kullanarak", "geçmişi bilmeden de anlaşılabilecek bir soru oluştur.")

    contextualize_q_prompt = ChatPromptTemplate.from_messages([
    ("system", contextualize_q_system_prompt),
    MessagesPlaceholder("chat_history"),  # 👈 Önceki mesajlar
    ("human", "{input}")                  # 👈 Son kullanıcı sorusu
])

    history_aware_retriever = create_history_aware_retriever(llm=llm,prompt=contextualize_q_prompt,retriever=retriever)

    system_prompt = """
    Sen bir yapay zeka asistanısın ve görevlerin;
    * Kullanıcılar sana bir soruc gönderecek
    * Bu sorunun cevabını ise bu verilen bağlamdan yararlanarak bul {context}
    """

  

    qa_prompt = ChatPromptTemplate.from_messages
    ([
    ("system", system_prompt),
    MessagesPlaceholder("chat_history"),  # 👈 Geçmiş yine veriliyor
    ("human", "{input}")                  # 👈 Kullanıcı sorusu
    ])
  

    question_answer_chain = create_stuff_documents_chain(llm=llm,prompt=qa_prompt)
    rag_chain = create_retrieval_chain(retriever=history_aware_retriever,combine_docs_chain=question_answer_chain)
    chat_history = []
    
    rag_chain_execute = {"input" : query,"chat_history" : chat_history,"context" : context}
    response = await rag_chain.ainvoke(rag_chain_execute)
    model_str_cevap = response["answer"]
    return model_str_cevap

```
