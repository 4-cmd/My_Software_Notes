Sistemin içini izleme & müdahale aracı olarak kullanılır 

1️⃣ **LlamaDebugHandler**

* Hangi node’lar geldi
* Similarity skorları
* Retriever ne döndürdü
* LLM çağrısı oldu mu

2️⃣ **TokenCountingHandler**

* Kaç token harcandığını görürsün
* Memory şişiyor mu anlarsın
* Maliyet kontrolü


```python
from llama_index.core.callbacks import CallbackManager, LlamaDebugHandler
from llama_index.core.callbacks import TokenCountingHandler
import tiktoken

debug_handler = LlamaDebugHandler(print_trace_on_end=True)

token_counter = TokenCountingHandler(
tokenizer=tiktoken.encoding_for_model("gpt-4o-mini")
)

callback_manager = CallbackManager([
    debug_handler,
    token_counter
])

```
