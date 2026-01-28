

```python
import threading
import time

  

def print_message():
    for i in range(5):
        print(f"Herkese merhaba")
        time.sleep(1)

  

thread = threading.Thread(target=print_message)
thread.start() # İşçiye "Hadi başla" demektir.
thread.join() # Ana programa "İşçi işini bitirene kadar burada bekle, sonra devam et" demektir. thread bitti mesajı iş bittikten sonra verilecek

print(f"thread bitti")

  

def thread_printers(name,count):
    for i in range(count):
        info = f"{name} says : {i}"
        print(info)
        time.sleep(1)
  

name = "thread"
count = 5
thread_2 = threading.Thread(
    target=thread_printers,
    args=(name,count)
) 
  
print(f"thread başlatılıyor")
thread_2.start()

```