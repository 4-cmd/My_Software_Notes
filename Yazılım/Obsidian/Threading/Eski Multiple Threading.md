```python
import threading
import requests


"""
Eğer illa klasik threading.Thread kullanacaksan,
veriyi geri döndüremezsin ama dışarıdaki bir listeye ekleyebilirsin.
"""

sayfa_verileri = [] # Sonuçları burada toplayacağız
  
def sayfa_kaynagi_al(site_adi, url):
    print(f"{site_adi} indiriliyor...")
    res = requests.get(url)
    # Veriyi global veya dışarıdaki listeye ekliyoruz
    sayfa_verileri.append({site_adi: res.text[:100]})

threadler = []
siteler = {"Google": "https://google.com", "Python": "https://python.org"}  

for isim, link in siteler.items():
    t = threading.Thread(target=sayfa_kaynagi_al, args=(isim, link))
    threadler.append(t)
    t.start()  # İşlemi başlatır (arka plana atar)
  
# join() döngüsü: Ana programın, tüm threadler bitmeden kapanmasını engeller
for t in threadler:
    t.join()

print("İşlem bitti. Veri sayısı:", len(sayfa_verileri))

```
